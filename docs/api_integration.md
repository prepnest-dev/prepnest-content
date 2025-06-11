# Prepnest API Integration Guide

## Overview

This document details the integration of external APIs in the Prepnest content pipeline, including configuration, usage patterns, error handling, and best practices for cost optimization.

## API Services

### OpenAI GPT-4

#### Purpose
- Content rewriting and enhancement
- Quiz question generation
- Curriculum alignment verification
- Educational content optimization

#### Configuration
```javascript
// config/openai.js
const { Configuration, OpenAIApi } = require('openai');

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
  organization: process.env.OPENAI_ORG_ID, // Optional
});

const openai = new OpenAIApi(configuration);

// Rate limiting configuration
const RATE_LIMITS = {
  requestsPerMinute: 50,
  tokensPerMinute: 40000,
  maxRetries: 3,
  retryDelay: 5000 // milliseconds
};
```

#### Usage Patterns

**Content Rewriting**
```javascript
async function rewriteContent(originalText, subject, gradeLevel) {
  const prompt = `
Rewrite this ${subject} content for ${gradeLevel} students in Ghana.
Make it engaging, clear, and curriculum-aligned.

Original: ${originalText}

Rewritten:`;

  try {
    const response = await openai.createChatCompletion({
      model: 'gpt-4-turbo',
      messages: [{
        role: 'system',
        content: 'You are an expert Ghanaian educator specializing in WASSCE curriculum.'
      }, {
        role: 'user',
        content: prompt
      }],
      max_tokens: 2000,
      temperature: 0.7,
      presence_penalty: 0.1
    });

    return {
      success: true,
      content: response.data.choices[0].message.content,
      usage: response.data.usage
    };
  } catch (error) {
    return handleOpenAIError(error);
  }
}
```

**Quiz Generation**
```javascript
async function generateQuiz(lessonContent, questionCount = 5) {
  const prompt = `
Create ${questionCount} WASSCE-style multiple choice questions based on this lesson.
Return as valid JSON array.

Lesson: ${lessonContent}

Questions:`;

  const response = await openai.createChatCompletion({
    model: 'gpt-4-turbo',
    messages: [{
      role: 'system',
      content: 'You are a WASSCE exam expert. Create high-quality assessment questions.'
    }, {
      role: 'user',
      content: prompt
    }],
    max_tokens: 1500,
    temperature: 0.5
  });

  // Parse and validate JSON response
  try {
    const questions = JSON.parse(response.data.choices[0].message.content);
    return validateQuizQuestions(questions);
  } catch (parseError) {
    throw new Error('Failed to parse quiz questions from GPT response');
  }
}
```

#### Error Handling
```javascript
function handleOpenAIError(error) {
  const errorResponse = {
    success: false,
    error: error.message,
    retryable: false
  };

  if (error.response?.status === 429) {
    // Rate limit exceeded
    errorResponse.retryable = true;
    errorResponse.retryAfter = error.response.headers['retry-after'];
  } else if (error.response?.status >= 500) {
    // Server error
    errorResponse.retryable = true;
  } else if (error.response?.status === 401) {
    // Authentication error
    errorResponse.error = 'Invalid OpenAI API key';
  }

  return errorResponse;
}
```

### ElevenLabs Text-to-Speech

#### Purpose
- Generate natural-sounding audio narrations
- Create subject-specific voice content
- Support multiple languages and accents

#### Configuration
```javascript
// config/elevenlabs.js
const axios = require('axios');

const ELEVENLABS_CONFIG = {
  baseURL: 'https://api.elevenlabs.io/v1',
  apiKey: process.env.ELEVENLABS_API_KEY,
  defaultVoice: 'Rachel', // For English content
  rateLimits: {
    requestsPerMinute: 20,
    charactersPerMonth: 10000
  }
};

// Voice mapping for different subjects
const VOICE_MAPPING = {
  mathematics: 'Daniel',
  english: 'Rachel',
  science: 'Antoni',
  social_studies: 'Elli'
};
```

#### Usage Example
```javascript
async function generateAudio(text, subject, outputPath) {
  const voiceId = getVoiceForSubject(subject);
  
  try {
    const response = await axios({
      method: 'post',
      url: `${ELEVENLABS_CONFIG.baseURL}/text-to-speech/${voiceId}`,
      headers: {
        'Accept': 'audio/mpeg',
        'xi-api-key': ELEVENLABS_CONFIG.apiKey,
        'Content-Type': 'application/json'
      },
      data: {
        text: text,
        model_id: 'eleven_monolingual_v1',
        voice_settings: {
          stability: 0.5,
          similarity_boost: 0.75
        }
      },
      responseType: 'arraybuffer'
    });

    // Save audio file
    fs.writeFileSync(outputPath, response.data);
    
    return {
      success: true,
      audioPath: outputPath,
      duration: await getAudioDuration(outputPath)
    };
  } catch (error) {
    return handleElevenLabsError(error);
  }
}
```

### AWS Textract

#### Purpose
- Extract text from PDF documents
- Preserve document structure and layout
- Handle complex formatting and tables

#### Configuration
```javascript
// config/aws.js
const AWS = require('aws-sdk');

AWS.config.update({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: process.env.AWS_REGION || 'us-east-1'
});

const textract = new AWS.Textract();

const TEXTRACT_CONFIG = {
  maxDocumentSize: 10485760, // 10MB
  supportedFormats: ['PDF', 'PNG', 'JPEG', 'TIFF'],
  rateLimits: {
    synchronousRequests: 5, // per second
    asynchronousRequests: 100 // per day
  }
};
```

#### Usage Example
```javascript
async function extractTextFromPDF(pdfPath) {
  const documentBytes = fs.readFileSync(pdfPath);
  
  const params = {
    Document: {
      Bytes: documentBytes
    },
    FeatureTypes: ['TABLES', 'FORMS']
  };

  try {
    const result = await textract.analyzeDocument(params).promise();
    
    return {
      success: true,
      text: extractTextFromBlocks(result.Blocks),
      tables: extractTablesFromBlocks(result.Blocks),
      confidence: calculateAverageConfidence(result.Blocks)
    };
  } catch (error) {
    return handleTextractError(error);
  }
}
```

### Supabase Storage

#### Purpose
- Store processed content and media
- Manage content versioning
- Provide CDN delivery

#### Configuration
```javascript
// config/supabase.js
const { createClient } = require('@supabase/supabase-js');

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_SERVICE_ROLE_KEY
);

const STORAGE_CONFIG = {
  buckets: {
    content: 'processed-content',
    audio: 'audio-narrations',
    video: 'video-content',
    images: 'content-images'
  },
  maxFileSize: 100 * 1024 * 1024, // 100MB
  allowedTypes: {
    content: ['application/json', 'text/markdown'],
    audio: ['audio/mpeg', 'audio/wav'],
    video: ['video/mp4', 'video/webm'],
    images: ['image/png', 'image/jpeg', 'image/webp']
  }
};
```

#### Usage Example
```javascript
async function uploadContent(filePath, bucket, fileName) {
  const fileBuffer = fs.readFileSync(filePath);
  
  try {
    const { data, error } = await supabase.storage
      .from(bucket)
      .upload(fileName, fileBuffer, {
        contentType: detectContentType(filePath),
        upsert: true
      });

    if (error) throw error;

    return {
      success: true,
      url: supabase.storage.from(bucket).getPublicUrl(fileName).data.publicUrl,
      path: data.path
    };
  } catch (error) {
    return handleSupabaseError(error);
  }
}
```

## Rate Limiting and Cost Management

### Rate Limiting Implementation
```javascript
// utils/rateLimiter.js
class RateLimiter {
  constructor(requestsPerMinute) {
    this.requestsPerMinute = requestsPerMinute;
    this.requests = [];
  }

  async waitForSlot() {
    const now = Date.now();
    const oneMinuteAgo = now - 60000;
    
    // Remove old requests
    this.requests = this.requests.filter(time => time > oneMinuteAgo);
    
    if (this.requests.length >= this.requestsPerMinute) {
      const oldestRequest = Math.min(...this.requests);
      const waitTime = 60000 - (now - oldestRequest);
      await new Promise(resolve => setTimeout(resolve, waitTime));
      return this.waitForSlot();
    }
    
    this.requests.push(now);
  }
}

// Usage
const openaiLimiter = new RateLimiter(50); // 50 requests per minute

async function makeOpenAIRequest(params) {
  await openaiLimiter.waitForSlot();
  return openai.createChatCompletion(params);
}
```

### Cost Tracking
```javascript
// utils/costTracker.js
class CostTracker {
  constructor() {
    this.costs = {
      openai: { tokens: 0, cost: 0 },
      elevenlabs: { characters: 0, cost: 0 },
      aws: { requests: 0, cost: 0 }
    };
  }

  trackOpenAI(usage) {
    const inputCost = usage.prompt_tokens * 0.00003; // $0.03 per 1K tokens
    const outputCost = usage.completion_tokens * 0.00006; // $0.06 per 1K tokens
    
    this.costs.openai.tokens += usage.total_tokens;
    this.costs.openai.cost += inputCost + outputCost;
  }

  trackElevenLabs(characterCount) {
    const cost = characterCount * 0.000011; // $0.011 per 1K characters
    
    this.costs.elevenlabs.characters += characterCount;
    this.costs.elevenlabs.cost += cost;
  }

  generateReport() {
    const totalCost = Object.values(this.costs)
      .reduce((sum, service) => sum + service.cost, 0);
    
    return {
      totalCost: totalCost.toFixed(4),
      breakdown: this.costs,
      date: new Date().toISOString()
    };
  }
}
```

## Error Handling and Retry Logic

### Exponential Backoff
```javascript
// utils/retryHandler.js
async function withRetry(operation, maxRetries = 3, baseDelay = 1000) {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await operation();
    } catch (error) {
      if (attempt === maxRetries || !isRetryableError(error)) {
        throw error;
      }
      
      const delay = baseDelay * Math.pow(2, attempt);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

function isRetryableError(error) {
  // Network errors
  if (error.code === 'ENOTFOUND' || error.code === 'ECONNRESET') {
    return true;
  }
  
  // HTTP 5xx errors
  if (error.response?.status >= 500) {
    return true;
  }
  
  // Rate limiting
  if (error.response?.status === 429) {
    return true;
  }
  
  return false;
}
```

## Monitoring and Alerts

### API Health Monitoring
```javascript
// monitoring/apiHealth.js
class APIHealthMonitor {
  constructor() {
    this.metrics = {
      openai: { requests: 0, errors: 0, avgResponseTime: 0 },
      elevenlabs: { requests: 0, errors: 0, avgResponseTime: 0 },
      aws: { requests: 0, errors: 0, avgResponseTime: 0 }
    };
  }

  recordRequest(service, duration, success) {
    const metric = this.metrics[service];
    metric.requests++;
    
    if (!success) {
      metric.errors++;
    }
    
    // Update average response time
    metric.avgResponseTime = (
      (metric.avgResponseTime * (metric.requests - 1) + duration) / 
      metric.requests
    );
  }

  getHealthStatus() {
    const alerts = [];
    
    Object.entries(this.metrics).forEach(([service, metric]) => {
      const errorRate = metric.errors / metric.requests;
      
      if (errorRate > 0.1) { // 10% error rate threshold
        alerts.push({
          service,
          type: 'high_error_rate',
          value: errorRate,
          threshold: 0.1
        });
      }
      
      if (metric.avgResponseTime > 10000) { // 10 second threshold
        alerts.push({
          service,
          type: 'slow_response',
          value: metric.avgResponseTime,
          threshold: 10000
        });
      }
    });
    
    return {
      status: alerts.length === 0 ? 'healthy' : 'degraded',
      alerts,
      metrics: this.metrics
    };
  }
}
```

## Best Practices

### 1. API Key Security
- Store keys in environment variables, never in code
- Use least-privilege access policies
- Rotate keys regularly
- Monitor usage for unusual patterns

### 2. Cost Optimization
- Implement caching to avoid duplicate requests
- Use appropriate model sizes for tasks
- Batch requests when possible
- Monitor usage and set budget alerts

### 3. Performance
- Implement connection pooling
- Use compression for large payloads
- Cache responses when appropriate
- Process requests in parallel where safe

### 4. Reliability
- Implement circuit breakers for failing services
- Use fallback mechanisms
- Log all API interactions for debugging
- Set appropriate timeouts

---

*This document is regularly updated as we integrate new services and optimize existing integrations. For questions, contact isaac@prepnest.com*
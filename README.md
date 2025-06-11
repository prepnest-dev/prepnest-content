# Prepnest Content Pipeline

> AI-powered educational content transformation for WASSCE/BECE preparation in Ghana

## Overview

Prepnest transforms traditional textbooks into engaging, interactive digital content optimized for offline mobile learning. This repository contains the content processing pipeline that converts source materials through OCR, AI rewriting, multimedia generation, and content bundling.

## Architecture

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│                     │     │                     │     │                     │
│    Source Content   │────▶│   Content Ingestion │────▶│  Content Processing │
│                     │     │                     │     │                     │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
                                                                │
                                                                ▼
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│                     │     │                     │     │                     │
│ Content Bundling &  │◀────│   Content Delivery  │◀────│ Content Enhancement │
│      Storage        │     │                     │     │                     │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
```

## Directory Structure

### Source Content
- `Prepnest_Content/` - Raw textbooks and educational materials
  - `01_Core_Mathematics/` - Mathematics textbooks and resources
  - `02_English_Language/` - English language materials
  - `03_Integrated_Science/` - Science content
  - [Additional WASSCE subjects...]
  - `_Shared_Assets/` - Cross-subject resources

### Processing Pipeline
- `scripts/` - Content processing automation
- `pipeline/` - AI processing workflows
- `config/` - Configuration files and templates

### Output Directories (Generated)
- `OCR_Ready/` - Preprocessed PDFs for text extraction
- `Textract_Results/` - Raw OCR output from AWS Textract
- `Structured_Content/` - Parsed and structured content
- `Rewritten_Content/` - AI-enhanced educational content
- `Generated_Quizzes/` - Automatically generated assessments
- `Generated_Audio/` - ElevenLabs audio narrations
- `Generated_Video/` - Video explanations and tutorials
- `Content_Bundles/` - Packaged content for distribution

## Quick Start

```bash
# Clone the repository
git clone https://github.com/prepnest-dev/prepnest-content.git
cd prepnest-content

# Install dependencies
npm install
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your API keys

# Process content pipeline
npm run pipeline
```

## Content Processing Steps

1. **Catalog Source Materials**: `npm run catalog`
2. **Preprocess PDFs**: `npm run preprocess`
3. **Extract Structure**: `npm run extract`
4. **AI Rewriting**: `npm run rewrite`
5. **Generate Quizzes**: `npm run quiz`
6. **Generate Audio**: `npm run audio`
7. **Create Bundles**: `npm run bundle`

## Features

- ✅ OCR Pipeline (AWS Textract + PaddleOCR)
- ✅ AI Content Rewriting (GPT-4 + LangChain)
- ✅ Automated Quiz Generation
- ✅ Audio Narration (ElevenLabs TTS)
- ✅ Content Bundling for Offline Use
- 🚧 Embedded AI (GGUF models)
- 🚧 Semantic Search (Typesense embedded)

## License

Proprietary - Prepnest Educational Technology

## Support

Technical issues: isaac@prepnest.com  
Documentation: [Prepnest Developer Hub](https://developers.prepnest.app)

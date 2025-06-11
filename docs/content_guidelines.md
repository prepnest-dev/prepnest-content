# Prepnest Content Creation Guidelines

## Overview

This document outlines the standards and best practices for creating educational content within the Prepnest ecosystem. All content must align with WASSCE/BECE curriculum requirements while maintaining accessibility for Ghanaian students.

## Content Quality Standards

### Educational Effectiveness

#### Learning Objectives
- Every lesson must have clear, measurable learning objectives
- Objectives should align with WASSCE/BECE syllabus requirements
- Use Bloom's taxonomy for objective classification
- Include both knowledge and application-level objectives

#### Content Structure
```markdown
# Lesson Title

## Learning Objectives
- Students will be able to...
- Students will understand...
- Students will apply...

## Introduction
- Hook to engage students
- Connection to prior knowledge
- Preview of lesson content

## Main Content
- Concept explanation with examples
- Step-by-step processes
- Visual aids and diagrams
- Real-world applications

## Practice Examples
- Worked examples with detailed solutions
- Graduated difficulty levels
- Common mistake identification

## Summary
- Key points recap
- Connection to broader concepts
- Next lesson preview

## Assessment
- Practice questions
- Self-check activities
- WASSCE-style problems
```

### Language and Accessibility

#### Writing Style
- **Clear and Simple**: Use straightforward language accessible to students with varying English proficiency
- **Active Voice**: Prefer active over passive voice
- **Concise**: Eliminate unnecessary words and complex sentences
- **Consistent Terminology**: Use WASSCE-standard terms throughout

#### Vocabulary Guidelines
- Define technical terms on first use
- Provide Ghanaian context for examples
- Use metric units throughout
- Include pronunciation guides for difficult terms

#### Cultural Sensitivity
- Use Ghanaian names, places, and contexts in examples
- Avoid cultural assumptions or bias
- Include diverse perspectives where appropriate
- Respect local customs and values

### Technical Standards

#### File Formats
- **Text Content**: Markdown (.md) with proper heading structure
- **Images**: PNG/WebP for diagrams, JPEG for photos
- **Audio**: MP3 format, 22kHz sample rate, mono
- **Video**: MP4 format, H.264 codec, 720p maximum

#### File Naming Conventions
```
# Lessons
[subject_code]_[unit_number]_[lesson_number]_[title].md
Example: MATH_01_03_linear_equations.md

# Images
[lesson_id]_[image_type]_[sequence].png
Example: MATH_01_03_diagram_01.png

# Audio
[lesson_id]_audio.mp3
Example: MATH_01_03_linear_equations_audio.mp3

# Video
[lesson_id]_video_[topic].mp4
Example: MATH_01_03_linear_equations_video_solving.mp4
```

#### Content Organization
```
Subject/
├── Unit_01_[Unit_Name]/
│   ├── Lesson_01_[Lesson_Name]/
│   │   ├── content.md
│   │   ├── images/
│   │   ├── audio/
│   │   └── video/
│   └── assessments/
│       ├── practice_questions.json
│       └── unit_test.json
└── resources/
    ├── formulas.md
    ├── glossary.md
    └── past_questions/
```

## Subject-Specific Guidelines

### Mathematics
- Use LaTeX notation for equations: `$x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}$`
- Include step-by-step solutions for all worked examples
- Provide multiple solution methods where applicable
- Use graphical representations for visual learners
- Include calculator usage instructions for appropriate problems

### Sciences (Biology, Chemistry, Physics, Integrated Science)
- Include clear, labeled diagrams for all concepts
- Use scientific notation consistently
- Provide safety warnings for practical activities
- Include real-world applications and examples
- Connect concepts to Ghanaian environment and context

### English Language
- Include audio pronunciation for difficult words
- Provide examples from Ghanaian literature where appropriate
- Use varied sentence structures in examples
- Include grammar rules with clear explanations
- Provide writing templates and frameworks

### Social Studies
- Use current and relevant examples from Ghana and West Africa
- Include maps and geographical references
- Provide historical context and timelines
- Include diverse perspectives on historical events
- Connect concepts to current affairs

## Assessment Creation Guidelines

### Question Types

#### Multiple Choice Questions
- Include 4 options (A, B, C, D)
- Ensure only one clearly correct answer
- Make distractors plausible but clearly incorrect
- Avoid "all of the above" or "none of the above" options
- Test understanding, not memorization

#### Short Answer Questions
- Require 1-3 sentence responses
- Test application of concepts
- Include clear marking criteria
- Provide sample acceptable answers

#### Essay Questions
- Include clear rubrics for assessment
- Specify expected length and structure
- Provide outline templates
- Include sample responses at different grade levels

### WASSCE Alignment
- Follow official WASSCE question formats
- Use appropriate mark allocations
- Include timing guidelines
- Reflect exam difficulty distribution (30% easy, 50% medium, 20% hard)
- Include past question analysis and patterns

## Quality Assurance Process

### Content Review Stages

1. **Initial Creation**
   - Subject matter expert creates content
   - Self-review against guidelines
   - Peer review by colleague

2. **Technical Review**
   - Format and structure check
   - Link and media verification
   - Accessibility testing
   - Mobile responsiveness check

3. **Educational Review**
   - Curriculum alignment verification
   - Learning objective assessment
   - Difficulty level evaluation
   - Cultural sensitivity review

4. **Student Testing**
   - Pilot testing with target students
   - Feedback collection and analysis
   - Iterative improvements
   - Final approval and publication

### Review Checklist

#### Content Quality
- [ ] Learning objectives clearly stated
- [ ] Content aligns with WASSCE syllabus
- [ ] Language appropriate for target grade level
- [ ] Examples relevant to Ghanaian context
- [ ] Concepts explained clearly and completely
- [ ] Practice opportunities provided

#### Technical Quality
- [ ] Proper markdown formatting
- [ ] All images optimized and accessible
- [ ] Audio quality meets standards
- [ ] Video content clear and audible
- [ ] File naming conventions followed
- [ ] Mobile-friendly presentation

#### Educational Effectiveness
- [ ] Engages student interest
- [ ] Builds on prior knowledge
- [ ] Includes varied learning activities
- [ ] Provides immediate feedback
- [ ] Connects to real-world applications
- [ ] Prepares students for assessments

## Continuous Improvement

### Feedback Integration
- Regular student surveys and feedback collection
- Teacher input on content effectiveness
- Performance data analysis from assessments
- Continuous content updates and improvements

### Analytics and Metrics
- Track completion rates and engagement
- Monitor assessment performance
- Identify challenging concepts
- Measure time spent on content
- Analyze offline usage patterns

### Update Procedures
- Monthly content reviews and updates
- Curriculum change adaptations
- Technology integration improvements
- Performance optimization
- Bug fixes and error corrections

## Compliance and Legal

### Copyright and Attribution
- All content must be original or properly licensed
- Provide proper attribution for external sources
- Ensure compliance with Ghanaian copyright laws
- Document all permissions and licenses

### Data Privacy
- Follow GDPR and local privacy regulations
- Minimize personal data collection
- Provide clear privacy notices
- Implement data security measures

### Accessibility
- Follow WCAG 2.1 AA guidelines
- Provide alternative text for images
- Ensure keyboard navigation support
- Include captions for video content
- Support screen readers and assistive technologies

---

*This document is a living guideline that evolves with our understanding of effective educational content creation. For questions or suggestions, contact the content team at isaac@prepnest.com*
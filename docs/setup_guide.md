# Prepnest Content Pipeline Setup Guide

## Prerequisites

### System Requirements
- **Node.js**: Version 18.0.0 or higher
- **Python**: Version 3.9 or higher
- **FFmpeg**: For audio/video processing
- **Git**: For version control
- **Operating System**: macOS, Linux, or Windows 10+

### Required Software

#### macOS Setup
```bash
# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install required packages
brew install node python ffmpeg git
brew install tesseract  # For OCR processing
```

#### Ubuntu/Debian Setup
```bash
# Update package manager
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install Python and FFmpeg
sudo apt-get install -y python3 python3-pip ffmpeg tesseract-ocr

# Install additional dependencies
sudo apt-get install -y poppler-utils ghostscript
```

### 4. Verify Installation

```bash
# Test Node.js setup
node --version
npm --version

# Test Python setup
python3 --version
pip --version

# Test FFmpeg
ffmpeg -version

# Test OCR
tesseract --version

# Test pipeline dependencies
node -e "console.log('Node.js setup complete')"
python3 -c "import requests, pandas, numpy; print('Python setup complete')"
```

### 5. Test Content Processing

```bash
# Run a quick test
npm run catalog -- --test

# Verify environment configuration
node -e "require('dotenv').config(); console.log('Environment loaded:', !!process.env.OPENAI_API_KEY)"
```

## Directory Structure After Setup

```
prepnest-content/
├── Prepnest_Content/           # Your source content goes here
│   ├── 01_Core_Mathematics/
│   ├── 02_English_Language/
│   └── ...
├── scripts/                    # Processing automation
├── config/                     # Configuration files
├── docs/                       # Documentation
├── package.json               # Node.js dependencies
├── requirements.txt           # Python dependencies
├── .env                       # Environment variables (create from .env.example)
└── .gitignore                # Git ignore rules
```

## Next Steps

1. **Upload Your Content**: Place your source materials in `Prepnest_Content/`
2. **Configure Subjects**: Edit `config/subjects.json` if needed
3. **Run Content Catalog**: `npm run catalog`
4. **Start Processing**: `npm run pipeline`

## Troubleshooting

### Common Issues

#### "Command not found" errors
- Ensure all prerequisites are installed
- Restart terminal after installation
- Check PATH environment variable

#### Python import errors
- Activate virtual environment: `source venv/bin/activate`
- Reinstall requirements: `pip install -r requirements.txt`
- Check Python version: `python3 --version`

#### API key errors
- Verify .env file exists and contains correct keys
- Test API connectivity:
  ```bash
  curl -H "Authorization: Bearer $OPENAI_API_KEY" https://api.openai.com/v1/models
  ```

#### FFmpeg not found
- Install FFmpeg for your platform
- Ensure it's in your PATH
- Test: `ffmpeg -version`

### Getting Help

- **Documentation**: Check `docs/` directory
- **Issues**: Create GitHub issue
- **Email**: isaac@prepnest.com
- **Slack**: #prepnest-dev (if you have access)

## Performance Optimization

### For Large Content Processing

1. **Increase Memory Limits**:
   ```bash
   export NODE_OPTIONS="--max-old-space-size=8192"
   ```

2. **Parallel Processing**:
   ```bash
   # Edit .env
   MAX_CONCURRENT_PROCESSES=8
   ```

3. **SSD Storage**: Use SSD for temporary files

4. **Monitor Resources**:
   ```bash
   # Install htop for monitoring
   htop
   ```

### Production Deployment

- Use Docker for consistent environment
- Set up monitoring with Prometheus
- Configure log rotation
- Use process managers like PM2
- Set up automated backups

## Security Considerations

- Never commit .env files
- Rotate API keys regularly
- Use least-privilege AWS IAM roles
- Enable 2FA on all accounts
- Monitor API usage and costs

## Updates and Maintenance

```bash
# Update Node.js dependencies
npm update

# Update Python dependencies
pip install -r requirements.txt --upgrade

# Check for security vulnerabilities
npm audit
pip-audit  # Install with: pip install pip-audit
```
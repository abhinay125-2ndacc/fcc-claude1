# FCC Claude Setup

Configuration guide for FCC (Firebase Cloud Code?) Claude integration.

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Configuration](#configuration)
- [Provider Setup](#provider-setup)
- [Usage Examples](#usage-examples)
- [Troubleshooting](#troubleshooting)

---

## Overview

**Note**: This appears to be a placeholder configuration. "FCC Claude" may refer to a specific integration. This document covers the general setup pattern for Claude Code with cloud providers.

---

## Installation

### Prerequisites
```bash
# Ensure Claude Code is installed
claude --version

# Ensure provider CLI is installed
# e.g., for Firebase:
npm install -g firebase-tools

# For AWS:
pip install awscli

# For GCP:
# Install gcloud CLI
```

---

## Configuration

### Environment Variables
```bash
# Add to .env (gitignored) or Codespaces secrets
export FCC_PROJECT_ID="your-project-id"
export FCC_API_KEY="your-api-key"
export FCC_REGION="us-central1"
```

### Configuration File
Create `.claude/fcc-config.json` (gitignored):
```json
{
  "projectId": "your-project-id",
  "region": "us-central1",
  "apiKey": "your-api-key",
  "endpoints": {
    "functions": "https://your-region-your-project.cloudfunctions.net",
    "firestore": "https://firestore.googleapis.com"
  }
}
```

---

## Provider Setup

### Firebase Setup
```bash
# 1. Login
firebase login

# 2. Select project
firebase use your-project-id

# 3. Initialize functions
firebase init functions

# 4. Deploy
firebase deploy --only functions
```

### AWS Setup
```bash
# 1. Configure credentials
aws configure

# 2. Verify
aws sts get-caller-identity

# 3. Deploy Lambda (example)
aws lambda update-function-code \
  --function-name your-function \
  --zip-file fileb://function.zip
```

### GCP Setup
```bash
# 1. Authenticate
gcloud auth login

# 2. Set project
gcloud config set project your-project-id

# 3. Deploy Cloud Function
gcloud functions deploy your-function \
  --runtime python310 \
  --trigger-http \
  --allow-unauthenticated
```

---

## Usage Examples

### Deploy from Claude Code
```
> Deploy the current project to FCC
```

### Invoke Function
```
> Call the FCC function with test payload
```

### View Logs
```
> Show recent FCC function logs
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Authentication failed | Re-run provider login command |
| Permission denied | Check IAM roles in cloud console |
| Deployment timeout | Increase timeout in config |
| Function not found | Verify function name and region |

---

## Related
- [Claude Code Setup](claude-code-setup.md)
- [MCP Servers](mcp-servers.md)
- [Environment Variables](environment-variables.md)

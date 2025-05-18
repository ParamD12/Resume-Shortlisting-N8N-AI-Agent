# Resume Shortlisting AI-Agent

An automated CV/resume screening and evaluation system built with n8n workflow automation and Groq's LLM capabilities.

## 📋 Overview

The Resume Shortlisting AI-Agent is an intelligent workflow that automatically processes job applications, evaluates candidate resumes against job requirements, and categorizes them based on qualification match. This solution eliminates the manual work of initial resume screening while maintaining consistent evaluation standards.

![N8N Workflow](<Screenshot 2025-05-14 141851.png>)
N8N Workflow

## 🎯 Key Features

- **Automated Screening**: Automatically processes new resumes uploaded to a designated folder
- **AI-Powered Evaluation**: Leverages meta-llama-4-maverick-17b-128e-instruct model via Groq API for intelligent resume analysis
- **Smart Classification**: Sorts candidates into Shortlisted, Keep-in-View, or Rejected categories
- **Document Organization**: Automatically moves resumes to appropriate folders based on evaluation
- **Tracking Integration**: Records all assessments in a centralized spreadsheet with scores and rationales
- **Model Context Protocol**: Implements MCP for structured, reliable AI interactions

## 🛠️ Technology Stack

- **[n8n](https://n8n.io/)**: Core workflow automation platform
- **[Groq API](https://groq.com/)**: AI provider with advanced LLM capabilities
- **[Google Workspace](https://workspace.google.com/)**:
  - Google Drive (file storage and organization)
  - Google Docs (job description reference)
  - Google Sheets (candidate tracking)
- **Model Context Protocol (MCP)**: Framework for enhanced AI interaction governing how context is provided to AI Models

## 🔄 Workflow Process

1. **Trigger**: New PDF file uploaded to "Unfiltered" Google Drive folder
2. **Download**: PDF file retrieved for processing
3. **Extract**: Text content extracted from PDF document
4. **Context**: Job description retrieved from Google Docs
5. **Analysis**:
   - Resume evaluated against job description using Groq LLM
   - MCP-formatted prompt ensures structured evaluation
   - Candidate scored on scale of 1-100
6. **Classification**: 
   - Shortlisted
   - Keep-in-View (KIV)
   - Rejected
7. **File Management**: Resume moved to appropriate category folder
8. **Tracking**: Assessment recorded in Google Sheet with score and rationale

## 📊 Expected Outcomes

- 80% reduction in resume screening time
- Consistent evaluation criteria across all applications
- Improved organization of candidate pool
- Detailed documentation of assessment rationale
- Freed HR resources for higher-value activities

## 📋 Prerequisites

- Active n8n instance (cloud or self-hosted)
- Google Workspace account with Drive, Docs, and Sheets access
- Groq API account with API key
- Configured folder structure in Google Drive:
  - Unfiltered
  - Shortlisted
  - Keep-in-View (KIV)
  - Rejected


![alt text](image.png)
**Google Drive Directory Structure**

## 🚀 Installation & Setup

1. **n8n Setup**:
   - Install n8n via your preferred method ([n8n installation guide](https://docs.n8n.io/hosting/))
   - Import the workflow JSON file from this repository

2. **Credential Configuration**:
   - Set up Google OAuth2 credentials in n8n
   - Add Groq API credentials in n8n

3. **Google Workspace Configuration**:
   - Create the required folder structure in Google Drive
   - Create job description document in Google Docs
   - Set up tracking spreadsheet in Google Sheets

4. **Workflow Configuration**:
   - Update node configurations with your specific Google Drive folder IDs
   - Update Google Docs document ID for job description
   - Update Google Sheets spreadsheet ID for tracking
   - Customize AI prompt if needed

5. **Testing & Activation**:
   - Run a test with a sample resume
   - Verify proper file movement and tracking
   - Activate workflow for production use

## 💻 Implementation Prompt

### AI Agent Prompt (MCP Format)

```
You are an expert in the recruitment and hiring industry, specializing in candidate evaluation, resume screening, and applicant tracking. You will evaluate the candidate’s resume against the given job description and respond with the following:

1. A decision: [REJECTED / KIV / SHORTLISTED]
2. A reason for the decision
3. A score (out of 100) indicating how well the candidate matches the job description
4. Extract the LinkedIn profile URL, Email address, and Contact number from the resume if available.

Important: When providing the contact number, format it as text with a leading single quote (') to avoid spreadsheet formula errors. For example: '+1234567890.

After you identify a decision, use the tools in the following sequence:

1. Move the resume PDF file to the correct folder using:
    - GoogleDrive:MoveFileToReject (for REJECTED)
    - GoogleDrive:MoveFileToShortlisted (for SHORTLISTED)
    - GoogleDrive:MoveFileToKIV (for KIV)
2. Update the tracker sheet using Gsheet:UpdateTracker tool.

==[JOB-DESC]==
{{ $json.content }}

==[/JOB-DESC]==

==[CANDIDATE-DESC]==
{{ $('Extract from File').item.json.text }}

==[/CANDIDATE-DESC]==
```


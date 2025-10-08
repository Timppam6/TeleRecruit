# Job Application Processor - n8n Workflow

An automated job application processing system that evaluates candidates through Telegram submissions using AI analysis and internet research.

## Overview

This n8n workflow automatically processes job applications submitted via Telegram. It supports multiple resume formats (PDF, TXT, Google Docs links, and plain text), conducts mandatory internet research on candidates using Tavily, and provides detailed evaluations to employers while preventing duplicate submissions.

## Features

- **Multi-format Resume Support**: Accepts PDF files, TXT files, Google Docs links, and plain text resumes
- **AI-Powered Evaluation**: Uses Cohere AI to analyze resumes against 6 key criteria
- **Mandatory Internet Research**: Automatically searches for candidates using Tavily AI
- **Social Media Verification**: Cross-references resume claims with GitHub, LinkedIn, and other professional profiles
- **Duplicate Prevention**: Tracks processed candidates in Google Sheets to prevent multiple submissions
- **Automatic Disqualification**: Rejects applications with inconsistent social media profiles
- **Self-Submission Rejection**: Prevents candidates from submitting their own resumes

## Workflow Flow

```
Telegram Trigger → File Type Detection → Content Extraction → Duplicate Check → AI Processing → Result Distribution
```

## Node Breakdown

### Input Handling
- **Telegram Trigger**: Receives all Telegram messages with document support
- **Switch**: Routes messages based on file type (PDF, TXT, or text content)
- **PDF Extract**: Processes PDF resume files
- **TXT Extract**: Processes TXT resume files

### Duplicate Prevention
- **Get row(s) in sheet**: Checks Google Sheets for existing candidate submissions
- **Switch1**: Determines if candidate is new or already processed

### AI Processing
- **Resume man (Agent)**: Main AI processor using the comprehensive evaluation prompt
- **Cohere Chat Model**: AI language model for resume analysis
- **Simple Memory**: Maintains conversation context
- **Tavily**: Internet search tool for candidate research
- **Google Docs Tool**: Processes Google Docs resume links
- **Think**: Additional AI reasoning tool

### Output Distribution
- **Who to send message to**: Routes responses based on content (employer vs candidate)
- **Code in JavaScript**: Cleans output by removing special markers
- **resume summary**: Sends evaluation results to employer (Telegram ID: 7876488681)
- **retry**: Sends responses back to candidate when needed
- **ty message**: Sends confirmation message to candidate
- **Append row in sheet**: Records processed applications in Google Sheets

## Setup Requirements

### Credentials Needed
1. **Telegram Bot Token** (Job applicator)
2. **Telegram Bot Token** (Bot timbo) 
3. **Cohere API Key**
4. **Tavily API Key**
5. **Google Sheets OAuth2**
6. **Google Docs OAuth2**

### Google Sheets Configuration
- Sheet ID: `1a66HH4VDYu3gEo06lOl1KHpfSUxoUiul-fAbfUjpFYY`
- Tracks processed candidates by username to prevent duplicates

### Telegram Setup
- Configure bot to accept document uploads
- Set up webhook for message triggering
- Two bots used for different messaging purposes

## Evaluation Criteria

The AI evaluates each candidate across 6 mandatory areas:
1. Technical Skills/Competency
2. Communication Abilities  
3. Problem-Solving & Critical Thinking
4. Work Experience & Achievements
5. Cultural Fit & Soft Skills
6. Online Presence & Internet Research Findings

## Security Features

- **Self-Submission Detection**: Automatically rejects resumes sent by the candidate themselves
- **Social Media Verification**: Disqualifies applications when online profiles don't match resume claims
- **Duplicate Tracking**: Prevents processing the same candidate multiple times

## Response Types

1. **Standard Evaluation**: Complete candidate assessment for employers
2. **Disqualification Alert**: When social media profiles contradict resume claims
3. **Self-Submission Rejection**: When candidates try to submit their own resumes
4. **Candidate Confirmation**: Acknowledgment message to applicants

## Usage

1. Candidates submit resumes through third parties via Telegram
2. System automatically detects file type and extracts content
3. Checks for duplicate submissions in Google Sheets
4. AI processes resume with mandatory internet research
5. Results sent to employer (Telegram user 7876488681)
6. Candidate receives confirmation message
7. All processed applications recorded in Google Sheets

## Requirements

- n8n instance with required nodes installed
- Telegram bots configured with proper permissions
- API keys for Cohere and Tavily
- Google account with Sheets and Docs access
- Internet connectivity for all API calls

## Notes

- The workflow uses Telegram ID `7876488681` as the primary employer recipient
- Duplicate prevention uses username matching in Google Sheets
- All resume claims must be verified through independent social media research
- Self-submissions are automatically rejected to maintain application integrity

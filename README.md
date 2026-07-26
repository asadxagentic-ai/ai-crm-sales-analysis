## Overview
This n8n workflow automates the process of analyzing sales call transcripts from PDF files stored in Google Drive. It uses AI to extract structured insights (persona, red flags, opportunities, qualification score, deal probability) and contact information, then automatically creates or updates HubSpot contacts and deals.

## How It Works
1. **Trigger**: Workflow starts manually via "Execute Workflow" button
2. **Download PDF**: Retrieves a specific PDF file (Zoom Meeting Summary) from Google Drive
3. **Extract Text**: Converts PDF content to readable text
4. **AI Analysis (Parallel)**:
   - **Sales Analysis Agent**: Uses Qwen Cloud Chat Model with structured output parser to analyze transcript for persona, red flags, opportunities, qualification score, deal analysis, probability, priority, and sales feedback
   - **Contact Extraction**: Uses Alibaba Cloud model (or OpenAI alternative) to extract client name and email from transcript
5. **Data Formatting**: Two Set nodes structure the AI outputs into consistent fields
6. **Merge**: Combines sales analysis data with contact information
7. **HubSpot Integration**: Creates/updates contact with custom properties and creates a deal with analysis details

## Nodes & Tools Used
| Node | Type | Purpose |
|------|------|---------|
| Manual Trigger | n8n-nodes-base.manualTrigger | Manual workflow execution |
| Google Drive | n8n-nodes-base.googleDrive | Download PDF from Google Drive |
| Extract from File | n8n-nodes-base.extractFromFile | Extract text from PDF |
| AI Agent | @n8n/n8n-nodes-langchain.agent | Analyze transcript with structured output |
| Structured Output Parser | @n8n/n8n-nodes-langchain.outputParserStructured | Enforce JSON schema for AI output |
| Qwen Cloud Chat Model | @n8n/n8n-nodes-langchain.lmChatAlibabaCloud | LLM for sales analysis |
| Alibaba Cloud Model | @n8n/n8n-nodes-langchain.alibabaCloud | Extract name/email from transcript |
| OpenAI Model | @n8n/n8n-nodes-langchain.openAi | Alternative for name/email extraction |
| Set (Edit Fields) | n8n-nodes-base.set | Format AI analysis output |
| Set (Edit Fields1) | n8n-nodes-base.set | Format contact extraction output |
| Merge | n8n-nodes-base.merge | Combine analysis and contact data |
| HubSpot Contact | n8n-nodes-base.hubspot | Create/update contact with custom properties |
| HubSpot Deal | n8n-nodes-base.hubspot | Create deal with analysis details |

## Prerequisites
- n8n instance (self-hosted or cloud)
- Google Drive OAuth2 credentials with file access
- Alibaba Cloud API credentials (for Qwen model and Alibaba Cloud node)
- OpenAI API credentials (optional, alternative for contact extraction)
- HubSpot OAuth2 credentials with contacts and deals write permissions
- A PDF file in Google Drive named "Zoom Meeting Summary.pdf" (or update file ID in workflow)

## Setup & Usage
1. **Import Workflow**: Copy the workflow JSON and import into n8n via Workflows → Import
2. **Configure Credentials**: Replace all `REPLACE_WITH_YOUR_CREDENTIAL_ID` placeholders with actual credential IDs:
   - Google Drive OAuth2
   - Alibaba Cloud API (for both Qwen and Alibaba Cloud nodes)
   - OpenAI API (if using alternative extraction)
   - HubSpot OAuth2
3. **Update File ID**: In the "Download file" node, replace `YOUR_VALUE_HERE` with your actual Google Drive file ID
4. **Test Run**: Click "Execute Workflow" to run manually
5. **Verify Results**: Check HubSpot for new/updated contact and deal with custom properties

## Use Cases
- **Sales Teams**: Automate CRM data entry from recorded sales calls
- **Revenue Operations**: Standardize deal qualification and probability scoring
- **Sales Managers**: Get AI-powered feedback on sales call performance
- **Marketing Teams**: Enrich contact profiles with persona and opportunity data
- **Business Development**: Identify cross-sell/upsell opportunities from conversations
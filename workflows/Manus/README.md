# 🤖 Manus AI Agent Collection

**Manus** is a comprehensive AI agent system designed for n8n that provides intelligent automation, conversation handling, task execution, and file processing capabilities across multiple platforms.

## 🌟 Overview

Manus consists of three interconnected workflows that work together to provide a complete AI assistant experience:

1. **Multi-Platform Conversational Agent** - Handles conversations across Slack, Discord, Telegram, Email, and webhooks
2. **Task Automation Agent** - Executes various business tasks automatically based on scheduled triggers
3. **Intelligent File Processor** - Analyzes and processes files with AI-powered insights

## 🚀 Workflows Included

### 1. Multi-Platform AI Agent (`2054_Manus_MultiPlatform_AI_Agent_Webhook.json`)

**Purpose**: Central conversational AI that handles user interactions across multiple platforms.

**Key Features**:
- 🔄 **Multi-Platform Support**: Slack, Discord, Telegram, Email, Webhook
- 🧠 **Conversation Memory**: Maintains context using vector store retrieval
- 🎯 **Smart Routing**: Automatically routes responses to the correct platform
- 📊 **Conversation Logging**: Tracks all interactions in Google Sheets
- 🤖 **GPT-4 Powered**: Uses OpenAI's latest models for intelligent responses

**Trigger**: Webhook endpoint `/manus-ai-agent`

**Input Format**:
```json
{
  "message": "Your message here",
  "user_id": "user@example.com",
  "platform": "slack|discord|telegram|email|webhook",
  "channel_id": "channel_or_chat_id"
}
```

### 2. Task Automation Agent (`2055_Manus_Task_Automation_Agent_Scheduled.json`)

**Purpose**: Automated task execution system that processes pending tasks from a task queue.

**Key Features**:
- ⏰ **Scheduled Execution**: Runs every 15 minutes to check for pending tasks
- 📧 **Email Automation**: Send emails with dynamic content
- 🔄 **Data Synchronization**: Sync data between different systems
- 📊 **Report Generation**: Create and distribute automated reports
- 🔔 **Multi-Platform Notifications**: Send alerts across various services
- 📅 **Calendar Management**: Create and manage calendar events
- 📁 **File Processing**: Process and analyze files
- 🔗 **API Integration**: Make calls to external services

**Supported Task Types**:
- `email_send` - Send emails to specified recipients
- `data_sync` - Synchronize data between systems
- `report_generate` - Generate and send reports
- `notification_send` - Send notifications across platforms
- `calendar_event` - Create or update calendar events
- `file_process` - Process files (convert, analyze, etc.)
- `api_call` - Make API calls to external services

**Task Queue Format** (Google Sheets):
| TaskID | TaskType | Description | Priority | RequestedBy | DueDate | Parameters | Status |
|--------|----------|-------------|----------|-------------|----------|------------|--------|
| T001 | email_send | Send welcome email | High | user@example.com | 2024-09-06 | {"recipients": "new@user.com", "subject": "Welcome!", "message": "Welcome to our platform!"} | pending |

### 3. Intelligent File Processor (`2056_Manus_Intelligent_File_Processor_Webhook.json`)

**Purpose**: AI-powered file analysis and processing system.

**Key Features**:
- 📄 **Multi-Format Support**: PDF, Word, Excel, CSV, Images, Text files
- 🧠 **AI Analysis**: Uses GPT-4 Vision for comprehensive file analysis
- 🔍 **OCR Capabilities**: Extract text from images and scanned documents
- 📊 **Data Insights**: Analyze spreadsheets for patterns and trends
- 📋 **Report Generation**: Create detailed analysis reports
- ☁️ **Cloud Storage**: Save reports to Google Drive
- 📧 **Email Notifications**: Send analysis results via email
- 📝 **Processing Logs**: Track all file processing activities

**Trigger**: Webhook endpoint `/manus-file-processor`

**Input Format**:
```json
{
  "file_url": "https://example.com/file.pdf",
  "file_name": "document.pdf",
  "file_type": "application/pdf",
  "request": "Please analyze this document and extract key insights",
  "user_id": "user@example.com"
}
```

## 🛠 Setup Instructions

### Prerequisites

1. **n8n Instance** (version 1.0+)
2. **Required Credentials**:
   - OpenAI API (for GPT-4 and GPT-4 Vision)
   - Platform APIs (Slack, Discord, Telegram)
   - Google Services (Sheets, Drive, Calendar)
   - SMTP for email sending
   - Any additional service APIs based on your use cases

### Installation Steps

1. **Import Workflows**:
   ```bash
   # Import each workflow into your n8n instance
   # workflows/Manus/2054_Manus_MultiPlatform_AI_Agent_Webhook.json
   # workflows/Manus/2055_Manus_Task_Automation_Agent_Scheduled.json
   # workflows/Manus/2056_Manus_Intelligent_File_Processor_Webhook.json
   ```

2. **Configure Credentials**:
   - Set up OpenAI API credentials
   - Configure platform-specific APIs (Slack, Discord, etc.)
   - Set up Google Services OAuth2 credentials
   - Configure SMTP settings for email

3. **Create Supporting Sheets**:
   
   **Manus Conversations Sheet**:
   | Timestamp | Platform | User ID | Channel | User Message | AI Response |
   |-----------|----------|---------|---------|--------------|-------------|
   
   **Manus Tasks Sheet**:
   | TaskID | TaskType | Description | Priority | RequestedBy | DueDate | Parameters | Status | CompletedAt | Result |
   |--------|----------|-------------|----------|-------------|----------|------------|--------|-------------|--------|
   
   **File Processing Log Sheet**:
   | Timestamp | File Name | File Type | User ID | Processing Request | Status | Agent |
   |-----------|-----------|-----------|---------|-------------------|--------|-------|

4. **Update Configuration**:
   - Replace `YOUR_GOOGLE_SHEET_ID` with actual Google Sheets IDs
   - Update `YOUR_DRIVE_FOLDER_ID` with your Google Drive folder ID
   - Configure webhook URLs and endpoints
   - Set up any custom API endpoints

5. **Activate Workflows**:
   - Activate the Multi-Platform Agent workflow
   - Activate the Task Automation Agent workflow
   - Activate the File Processor workflow

## 💡 Usage Examples

### Conversational AI
```bash
# Send a message via webhook
curl -X POST https://your-n8n-instance.com/webhook/manus-ai-agent \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Hello Manus, can you help me schedule a meeting?",
    "user_id": "john@example.com",
    "platform": "slack",
    "channel_id": "#general"
  }'
```

### Task Automation
Add a row to your Manus Tasks Google Sheet:
```
TaskID: T123
TaskType: email_send
Description: Send project update email
Priority: High
RequestedBy: manager@example.com
DueDate: 2024-09-06
Parameters: {"recipients": "team@example.com", "subject": "Project Update", "message": "Weekly project status update"}
Status: pending
```

### File Processing
```bash
# Process a file
curl -X POST https://your-n8n-instance.com/webhook/manus-file-processor \
  -H "Content-Type: application/json" \
  -d '{
    "file_url": "https://example.com/report.pdf",
    "file_name": "quarterly-report.pdf",
    "file_type": "application/pdf",
    "request": "Analyze this quarterly report and extract key financial metrics",
    "user_id": "analyst@example.com"
  }'
```

## 🔧 Customization

### Adding New Task Types

1. Open the Task Automation Agent workflow
2. Add a new case in the "Task Type Router" switch node
3. Create a new processing node for your task type
4. Connect it to the "Update Task Status" node
5. Update the AI prompt to include the new task type

### Adding New Platforms

1. Open the Multi-Platform Agent workflow
2. Add a new case in the "Platform Response Router" switch node
3. Create a new platform-specific response node
4. Configure the appropriate credentials and parameters

### Extending File Processing

1. Open the File Processor workflow
2. Add new file type detection in the "Detect File Type" switch
3. Create specialized processing nodes for new file types
4. Update the AI analyzer prompt to handle new file formats

## 🔒 Security Considerations

- **Credential Management**: Store all API keys and credentials securely in n8n's credential system
- **Input Validation**: Validate all incoming webhook data
- **Access Control**: Implement proper access controls for webhook endpoints
- **Data Privacy**: Ensure compliance with data protection regulations
- **Rate Limiting**: Implement rate limiting for webhook endpoints
- **Logging**: Monitor and log all activities for security auditing

## 🚀 Advanced Features

### Integration with Other Workflows
Manus can trigger other n8n workflows using the `executeWorkflow` node, enabling complex automation chains.

### Custom AI Tools
Add custom tools to the AI agents by extending the LangChain agent configuration with additional tool nodes.

### Multi-Language Support
Configure the AI models to respond in different languages based on user preferences.

### Advanced Analytics
Extend the logging system to provide detailed analytics and insights about agent usage and performance.

## 🐛 Troubleshooting

### Common Issues

1. **Webhook Not Responding**:
   - Check if the workflow is activated
   - Verify webhook URL configuration
   - Check n8n logs for errors

2. **AI Responses Not Working**:
   - Verify OpenAI API credentials
   - Check API rate limits and quotas
   - Review AI model configuration

3. **Platform Integration Issues**:
   - Verify platform-specific credentials
   - Check API permissions and scopes
   - Review platform-specific documentation

4. **File Processing Errors**:
   - Verify file format support
   - Check file size limits
   - Ensure proper file access permissions

### Debug Mode
Enable debug mode in n8n to see detailed execution logs and identify issues.

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [LangChain n8n Integration](https://docs.n8n.io/integrations/langchain/)
- [Google APIs Documentation](https://developers.google.com/)

## 🤝 Contributing

To contribute improvements to Manus:

1. Fork the repository
2. Create a feature branch
3. Make your improvements
4. Test thoroughly
5. Submit a pull request with detailed descriptions

## 📄 License

This Manus AI Agent collection is part of the n8n-workflows repository and follows the same licensing terms.

---

**🎯 Perfect for**: Developers, automation engineers, business analysts, and teams looking to implement comprehensive AI-driven automation with multi-platform support.

*Manus - Making AI automation accessible and powerful for everyone.*
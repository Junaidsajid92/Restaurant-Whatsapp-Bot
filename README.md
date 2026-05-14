# 🍽️ Restaurant WhatsApp Bot

An intelligent WhatsApp chatbot built with **n8n** that serves as an AI concierge for restaurants. The bot leverages OpenAI's GPT-4 to provide friendly, efficient customer service directly through WhatsApp.

## ✨ Features

- **AI-Powered Conversations** - Uses GPT-4 mini for intelligent, context-aware responses
- **Message Type Handling** - Supports text messages, replies, and image processing
- **Interactive Buttons** - Sends dynamic button-based responses for better UX
- **Message Status Tracking** - Marks messages as read and shows typing indicators
- **Conversation Memory** - Maintains chat history for better context understanding
- **Multi-User Support** - Handles multiple conversations simultaneously
- **Google Sheets Integration** - Access restaurant data via Google Sheets MCP tool
- **User Filtering** - Prevents bot from responding to its own messages

## 🏗️ Architecture

The workflow consists of several key components:

### 1. **Webhook Entry Point**
- Receives incoming WhatsApp messages from Whapi.cloud
- Webhook configured in your n8n instance

### 2. **Message Processing Pipeline**
- **User Filter** - Validates message isn't from the bot
- **Message Type Router** - Routes based on message type (text/reply/image)
- **Message Status** - Marks messages as read
- **Typing Indicator** - Shows bot is composing

### 3. **AI Engine**
- **AI Agent** - Orchestrates conversation with system prompt
- **OpenAI Chat Model** - GPT-4 mini for generating responses
- **Simple Memory** - Maintains conversation context per chat session
- **Google Sheets MCP** - Accesses restaurant menu and information

### 4. **Response Generation**
- **Code Processing** - Validates and formats AI output
- **Button Detection** - Determines response format
- **JSON Formatting** - Prepares interactive messages

### 5. **Message Delivery**
- **Send Simple Message** - Delivers text-only responses
- **Interactive Messages** - Sends button-based responses
- Both use Whapi.cloud API with Bearer authentication

## 🤖 AI Concierge Persona

The bot operates as an AI concierge for your restaurant with these traits:
- Friendly and efficient
- Helpful and professional
- Restaurant-specific knowledge
- Capability to handle bookings, menu inquiries, and general assistance

### System Prompt Includes:
- Current date/time context
- Restaurant-specific guidelines
- Response format requirements
- Capabilities and limitations

## 🔧 Configuration

### Required Credentials
1. **OpenAI API Key** - For GPT-4 access
2. **Whapi.cloud Bearer Token** - WhatsApp message sending
3. **Whapi.cloud Header Auth** - Message read status and presence

> ⚠️ **Important**: Store all credentials securely in n8n's credential management system. Never commit API keys or tokens to version control.

### Environment Setup
- Configure admin number filtering in the workflow
- Session management uses chat ID from incoming messages
- Conversation memory window maintains context

## 📊 Data Flow

```
WhatsApp Message
       ↓
    Webhook
       ↓
  User Filter
       ↓
Message Type Switch
       ↓
  Read Status + Typing Indicator
       ↓
    AI Agent (with Memory)
       ↓
   Output Processing
       ↓
Format Detection (Buttons or Text)
       ↓
   HTTP Request to Whapi.cloud
       ↓
  Message Delivered to User
```

## 🚀 Getting Started

### Prerequisites
- n8n instance (self-hosted or n8n Cloud)
- OpenAI API key
- Whapi.cloud account with WhatsApp integration
- Google Sheets for restaurant data (optional)

### Installation
1. Import the workflow JSON into your n8n instance
2. Configure credentials in n8n:
   - OpenAI API key
   - Whapi.cloud Bearer token
   - Whapi.cloud Header authentication
3. Enable the webhook and get the unique URL from n8n
4. Configure Whapi.cloud webhook to point to your n8n instance
5. Update the AI Agent node with your restaurant details
6. Configure admin number filtering as needed

### Testing
1. Send a WhatsApp message to your configured number
2. Check n8n execution history for workflow progress
3. Verify response appears in WhatsApp

## 📝 Message Types Supported

| Type | Handler | Example |
|------|---------|---------|
| **Text** | Direct text processing | "What are your opening hours?" |
| **Reply** | Button reply handling | User selects from menu buttons |
| **Image** | Image attachment detection | User sends food photo |

## 🎯 Workflow Nodes

### Decision Nodes
- **If** - Validates text or reply messages exist
- **User Filter** - Filters messages from admin/bot
- **If Buttons not found** - Checks if buttons were generated
- **Buttons OR simple Text** - Determines response format

### Communication Nodes
- **Send Simple Message** - Text responses
- **Interactive Messages** - Button-based responses
- **Read message** - Mark as read status
- **Send Typing** - Typing indicator

### Processing Nodes
- **AI Agent** - Main conversation logic
- **Simple Memory** - Context management
- **Code** - Output validation and formatting
- **Google Sheets MCP** - Data access

## ⚙️ Response Formats

### Simple Text Response
```json
{
  "to": "<RECIPIENT_PHONE>",
  "body": "Your response text",
  "typing_time": 1
}
```

### Interactive Button Response
```json
{
  "to": "<RECIPIENT_PHONE>",
  "output": {
    "buttons": [
      {"title": "Option 1"},
      {"title": "Option 2"}
    ],
    "body": "Please select an option:"
  }
}
```

## 🔐 Security Considerations

- **Store Credentials Securely**: Use n8n's credential management, never hardcode API keys
- **Environment Variables**: Use environment variables for sensitive data
- **Admin Filtering**: Configure admin number in the workflow to prevent unauthorized access
- **Session Isolation**: Each chat has isolated conversation memory
- **HTTPS Only**: All communication with external APIs uses HTTPS
- **Rate Limiting**: Implement rate limiting to prevent abuse
- **Audit Logging**: Monitor n8n execution logs for suspicious activity

## 📈 Monitoring & Debugging

- Check n8n execution history for workflow runs
- View logs in Code nodes for JSON validation
- Monitor Whapi.cloud API response codes
- Verify conversation memory in Simple Memory node
- Set up alerts for failed executions

## 🛠️ Customization

### Modify AI Behavior
Edit the system message in the **AI Agent** node to change:
- Restaurant name and details
- Available services
- Response tone and guidelines
- Special instructions
- Operating hours and policies

### Add New Features
- Connect additional APIs in the workflow
- Add more message type handlers in Switch nodes
- Extend Google Sheets integration for dynamic data
- Implement analytics tracking
- Add payment processing integration

### Extend Capabilities
- Integrate with reservation systems
- Connect to POS systems
- Add inventory management
- Implement loyalty programs
- Enable multi-language support

## 📞 Support

For issues or questions:
- Check Whapi.cloud documentation for API errors
- Review OpenAI API status for model availability
- Verify n8n webhook URLs are correctly configured
- Test credential authentication in credential edit panel
- Review n8n community forums and documentation

## 🔄 Best Practices

1. **Regular Testing** - Test the bot regularly with various inputs
2. **Update AI Persona** - Keep restaurant information up-to-date
3. **Monitor Logs** - Review execution logs for errors or unusual patterns
4. **Backup Configuration** - Export workflow regularly
5. **Update Dependencies** - Keep n8n and AI models current
6. **Security Audits** - Periodically review security settings

## 📄 License

This project is open source. Modify and use as needed for your restaurant.

## 📋 Environment Variables Reference

Consider using these environment variables for configuration:

```
OPENAI_API_KEY=<your-api-key>
WHAPI_BEARER_TOKEN=<your-token>
WHAPI_HEADER_AUTH=<your-header-auth>
ADMIN_PHONE_NUMBER=<admin-number>
RESTAURANT_NAME=<restaurant-name>
N8N_WEBHOOK_URL=<your-webhook-url>
```

---

**Created with ❤️ for restaurant automation**

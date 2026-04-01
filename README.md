# Local AI Email Auto-Responder with n8n and Ollama

Build a fully local, private AI email auto-responder using n8n for workflow automation and Ollama for on-device LLM inference.

## Prerequisites
- Docker and Docker Compose
- Email account with IMAP/SMTP access (e.g., App Passwords for Gmail)

## Setup Instructions

### 1. Environment Configuration
Copy `.env.example` to `.env` and fill in your credentials:
```bash
cp .env.example .env
```

### 2. Start Services
Run the following command to start n8n and Ollama:
```bash
docker-compose up -d
```

### 3. Initialize AI Model
Pull the `llama3:8b` model into your Ollama instance:
```bash
docker exec -it ollama_responder ollama pull llama3:8b
```

### 4. Import Workflow
1. Access n8n at `http://localhost:5678`.
2. Create a new workflow and import the `workflow.json` file from the root of this repository.
3. Configure your credentials in the IMAP and SMTP nodes.

## Testing & Verification
1. Send an email to the configured address.
2. Verify n8n triggers and generates a response via Ollama.
3. Confirm the reply is sent and maintains the email thread.

## Loop Prevention
The workflow includes an IF node that checks for the sender's address to prevent infinite auto-reply loops.

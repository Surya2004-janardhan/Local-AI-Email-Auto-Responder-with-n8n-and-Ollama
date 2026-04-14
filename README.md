# Local AI Email Auto-Responder with n8n and Ollama

Build a fully local, private email auto-responder. n8n handles the workflow automation and Ollama runs the LLM on your machine.

## What this workflow does
1. Watches your inbox via IMAP for unread emails.
2. Blocks replies to your own address to prevent loops.
3. Formats a prompt from the incoming email.
4. Calls Ollama (`llama3:8b` by default) to generate a reply.
5. Sends the response via SMTP, keeping the same email thread.

## Project contents
- `docker-compose.yml` — Runs n8n and Ollama.
- `workflow.json` — n8n workflow to import.
- `.env.example` — Reference for IMAP/SMTP values.

## Prerequisites
- Docker Engine + Docker Compose
- An email account with IMAP and SMTP enabled
- App password (recommended for Gmail and other providers)

## Quick start
1. **Create your local env file**
   ```bash
   cp .env.example .env
   ```
   Use it as a reference when creating credentials in n8n.

2. **Start services**
   ```bash
   docker compose up -d
   ```
   The first run pulls the `llama3:8b` model and may take a few minutes.

3. **(Optional) Pull the model manually**
   ```bash
   docker exec -it ollama_responder ollama pull llama3:8b
   ```

4. **Import the workflow in n8n**
   1. Open `http://localhost:5678`.
   2. Create a new workflow and import `workflow.json`.
   3. Set up IMAP and SMTP credentials using your provider details.
   4. Update the **Loop Prevention** node and **Send Reply** node with your actual email address.
   5. Activate the workflow.

## Configuration reference
Use `.env` as a source of truth for values you will enter into n8n credentials.

| Variable | Description |
| --- | --- |
| `IMAP_HOST` / `IMAP_PORT` | IMAP server and port (e.g., `imap.gmail.com:993`) |
| `IMAP_USER` / `IMAP_PASSWORD` | IMAP username and app password |
| `SMTP_HOST` / `SMTP_PORT` | SMTP server and port (e.g., `smtp.gmail.com:465`) |
| `SMTP_USER` / `SMTP_PASSWORD` | SMTP username and app password |

### Swap the model (optional)
Update the model in both places:
- `docker-compose.yml` (`ollama pull ...`)
- `workflow.json` or the **Call Ollama** node (`model` parameter)

## Verify it works
1. Send a test email to the configured inbox.
2. Check executions in n8n for a successful run.
3. Confirm the reply appears in the same thread.

## Troubleshooting
- **Model download is slow**: check `docker compose logs -f ollama`.
- **Authentication errors**: ensure IMAP/SMTP is enabled and you are using an app password.
- **No replies**: confirm the workflow is active and the loop-prevention check uses your exact email address.

## Security notes
- Keep `.env` private and do not commit it.
- All inference stays local within Ollama containers.

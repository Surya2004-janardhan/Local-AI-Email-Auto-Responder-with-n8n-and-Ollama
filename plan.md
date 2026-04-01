# Project Plan - Local AI Email Auto-Responder

A 5-phase approach to building a fully local, private AI email auto-responder using n8n and Ollama.

## Phase 1: Foundation & Infrastructure
- [ ] Create `docker-compose.yml` with n8n and Ollama services.
- [ ] Configure custom bridge network `ai_responder_net`.
- [ ] Set up persistent volumes and healthchecks.
- [ ] Create `.env.example` and `submission.json` infrastructure.

## Phase 2: Local AI Service (Ollama)
- [ ] Pull and verify `llama3:8b` model.
- [ ] Test Ollama API connectivity.
- [ ] Implement or document model persistence/automated pulling.

## Phase 3: n8n Workflow - Inbound & Logic
- [ ] Configure IMAP Trigger for unread emails.
- [ ] Implement Loop Prevention IF node (don't reply to self/replies).
- [ ] Design the prompt template using email context.

## Phase 4: n8n Workflow - AI & Outbound
- [ ] Setup HTTP Request node to call Ollama API.
- [ ] Parse Ollama JSON response.
- [ ] Configure SMTP Send node with threading support (Message-ID).

## Phase 5: Verification & Delivery
- [ ] End-to-end testing with real emails.
- [ ] Verify loop prevention logic.
- [ ] Export final `workflow.json`.
- [ ] Finalize documentation in `README.md`.

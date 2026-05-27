# 🏗️ Arquitetura do StudyFlow

## Visão Geral

```
                    ┌─────────────────────────────────────────┐
                    │              USUÁRIO                     │
                    │         (Telegram Mobile/Desktop)        │
                    └───────────────────┬─────────────────────┘
                                        │
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │           TELEGRAM SERVERS               │
                    │         api.telegram.org                 │
                    └───────────────────┬─────────────────────┘
                                        │ webhook POST
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │         CLOUDFLARE TUNNEL                │
                    │    studyflow.jessicakacz.com.br          │
                    └───────────────────┬─────────────────────┘
                                        │
                                        ▼
                    ┌─────────────────────────────────────────┐
                    │           n8n (Docker)                   │
                    │         localhost:5678                   │
                    │                                          │
                    │  ┌─────────────┐  ┌──────────────────┐  │
                    │  │   Webhook   │  │  Callback Trigger │  │
                    │  │  (message)  │  │  (inline buttons) │  │
                    │  └──────┬──────┘  └────────┬─────────┘  │
                    │         │                   │            │
                    │         ▼                   ▼            │
                    │  ┌─────────────────────────────────────┐ │
                    │  │          AI Agent (Jessy AI)        │ │
                    │  │        Groq — Llama 3.3 70B         │ │
                    │  └─────────────────────────────────────┘ │
                    └──────────┬──────────────────┬────────────┘
                               │                  │
                               ▼                  ▼
              ┌────────────────────┐  ┌────────────────────┐
              │    PostgreSQL 16   │  │       Trello        │
              │   (persistência)   │  │  (gestão de tarefas)│
              └────────────────────┘  └────────────────────┘
```

---

## Fluxo de mensagem (usuário externo)

1. Usuário envia mensagem ao bot no Telegram
2. Telegram entrega via webhook para o Cloudflare Tunnel
3. n8n recebe e identifica o tipo de interação
4. Se for mensagem de texto → encaminha para o AI Agent (Groq)
5. Se for callback (botão inline) → roteamento direto para resposta estática
6. Resposta enviada de volta via API do Telegram (através do Cloudflare Worker)

---

## Fluxo de mensagem (Jessica — uso pessoal)

1. Jessica envia comando pelo Telegram
2. n8n identifica pelo chat ID que é a Jessica
3. Aciona fluxo personalizado (gestão de Trello, consultas, etc.)
4. Resposta personalizada enviada de volta

---

## Decisões técnicas

| Decisão | Motivo |
|---|---|
| n8n self-hosted | Controle total, sem limites de execução, dados locais |
| Groq (Llama 3.3 70B) | Alta performance, API gratuita generosa |
| PostgreSQL | Persistência confiável para dados do n8n |
| Cloudflare Tunnel | Exposição segura sem abrir portas no roteador |
| Cloudflare Worker | Contornar bloqueio de ISP ao api.telegram.org |
| Docker Compose | Facilidade de manutenção e reprodutibilidade |

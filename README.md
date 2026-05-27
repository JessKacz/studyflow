# 🤖 StudyFlow

> Assistente pessoal self-hosted de **Jessica Kaczmarkiewicz** — combinando automação, IA e gestão de estudos em uma única plataforma.

[![n8n](https://img.shields.io/badge/n8n-self--hosted-orange)](https://n8n.io)
[![Telegram](https://img.shields.io/badge/Telegram-Bot-blue)](https://t.me/JessicaKacz_StudyFlow_bot)
[![Groq](https://img.shields.io/badge/Groq-Llama%203.3%2070B-green)](https://groq.com)
[![Docker](https://img.shields.io/badge/Docker-Compose-blue)](https://docker.com)
[![Cloudflare](https://img.shields.io/badge/Cloudflare-Tunnel%20%2B%20Workers-orange)](https://cloudflare.com)

---

## 🎯 O Projeto

O **StudyFlow** nasceu de uma necessidade real: acompanhar de forma organizada e inteligente a transição de carreira de Jessica para a área de **Quality Assurance (QA)**.

O projeto tem dois papéis simultâneos:

### 📚 Uso Pessoal
Um assistente de estudos acessível direto pelo Telegram, que:
- Gerencia tarefas e progresso no **Trello** (cria e move cards automaticamente)
- Responde dúvidas e auxilia nos estudos com IA (Groq + Llama 3.3 70B)
- Funciona como central de controle da rotina de estudos

### 💼 Portfólio Profissional
O mesmo bot tem uma face pública como **Jessy AI** — uma IA que se apresenta, fala sobre Jessica e demonstra na prática as habilidades técnicas desenvolvidas:
- Automação com n8n
- Integração de APIs (Telegram, Groq, Trello)
- Infraestrutura self-hosted com Docker e Cloudflare
- Desenvolvimento de bots com IA

---

## 🏗️ Arquitetura

```
Telegram ──► Cloudflare Tunnel ──► n8n (Docker)
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                   │
              Groq API           PostgreSQL            Trello API
           (Llama 3.3 70B)      (persistência)       (gestão tarefas)
```

---

## 🛠️ Stack Técnica

| Componente | Tecnologia |
|---|---|
| Automação | n8n (self-hosted) |
| IA | Groq — Llama 3.3 70B Versatile |
| Interface | Telegram Bot |
| Banco de dados | PostgreSQL 16 |
| Containerização | Docker + Docker Compose |
| Gerenciamento | Portainer |
| Exposição pública | Cloudflare Tunnel |
| Proxy de API | Cloudflare Workers |
| Gestão de tarefas | Trello |

---

## 📁 Estrutura do Repositório

```
studyflow/
├── docs/
│   ├── architecture.md        # Diagrama e decisões de arquitetura
│   ├── setup.md               # Guia completo de instalação
│   ├── cloudflare-worker.md   # Configuração do Worker proxy
│   └── troubleshooting.md     # Problemas conhecidos e soluções
├── n8n/
│   └── workflows/             # Workflows exportados do n8n
├── docker/
│   └── docker-compose.yml     # Configuração dos containers
└── README.md
```

---

## 🚀 Como Executar

Veja o guia completo em [docs/setup.md](docs/setup.md).

---

## 👩‍💻 Sobre

Projeto desenvolvido por **Jessica Kaczmarkiewicz** como parte de sua transição de carreira para QA, documentando na prática o aprendizado em automação, infraestrutura e integração de sistemas.

- 🔗 [LinkedIn](https://linkedin.com/in/jessica-kaczmarkiewicz)
- 🤖 [Telegram Bot](https://t.me/JessicaKacz_StudyFlow_bot)

# ⚙️ Guia de Instalação

## Pré-requisitos

- Docker e Docker Compose instalados
- Domínio com Cloudflare (para exposição pública)
- Conta no Telegram (para criar o bot via @BotFather)
- Conta no Groq (para a API de IA)
- Conta no Trello (para gestão de tarefas)

---

## 1. Clonar o repositório

```bash
git clone https://github.com/JessKacz/studyflow.git
cd studyflow
```

---

## 2. Configurar o docker-compose

Copie o arquivo de exemplo e edite com suas credenciais:

```yaml
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: n8n
      POSTGRES_PASSWORD: SUA_SENHA
      POSTGRES_DB: n8n
    volumes:
      - ./postgres_data:/var/lib/postgresql/data
    restart: always

  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - TZ=America/Sao_Paulo
      - N8N_SECURE_COOKIE=false
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=SUA_SENHA
      - WEBHOOK_URL=https://SEU_DOMINIO
    volumes:
      - ./n8n_data:/home/node/.n8n
    depends_on:
      - postgres
    restart: always

  portainer:
    image: portainer/portainer-ce:latest
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./portainer_data:/data
    restart: always
```

---

## 3. Subir os containers

```bash
docker compose up -d
```

---

## 4. Configurar Cloudflare Tunnel

1. Acesse o Cloudflare Zero Trust Dashboard
2. Crie um novo Tunnel apontando para `localhost:5678`
3. Configure o domínio público desejado

---

## 5. Configurar o Cloudflare Worker (proxy Telegram)

Necessário para contornar bloqueios de ISP ao `api.telegram.org`.

Veja o guia completo em [cloudflare-worker.md](cloudflare-worker.md).

---

## 6. Importar os workflows no n8n

1. Acesse o n8n em `https://SEU_DOMINIO`
2. Vá em **Settings → Import workflow**
3. Importe os arquivos da pasta `n8n/workflows/`
4. Configure as credenciais (Telegram, Groq, Trello)
5. Ative os workflows

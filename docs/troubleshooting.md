# 🔧 Troubleshooting

## Telegram não recebe mensagens

### Sintoma
Mensagens enviadas ao bot não chegam como execuções no n8n.

### Causas e soluções

**1. ISP bloqueando api.telegram.org**
- Verifique: `ping 149.154.166.110` — se houver 100% de perda, é bloqueio de IP
- Solução: Configure o Cloudflare Worker como proxy (ver [cloudflare-worker.md](cloudflare-worker.md))

**2. Webhook apontando para URL errada**
- Verifique: `https://SEU-WORKER.workers.dev/botTOKEN/getWebhookInfo`
- O campo `url` deve apontar para `https://SEU_DOMINIO/webhook/WEBHOOK_ID/webhook`

**3. Workflow inativo**
- O n8n só registra webhooks quando o workflow está ativo
- Desative e reative o workflow após qualquer alteração

**4. WebhookId desatualizado**
- Ao atualizar o workflow via API/SDK, o webhookId pode mudar
- Verifique o ID atual no editor do n8n e re-registre manualmente se necessário

---

## Erro "Provides secret is not valid"

O n8n usa um secret token interno para validar requisições do Telegram.
Deixe sempre o n8n registrar o webhook automaticamente (via ativação do workflow).
Nunca registre o webhook manualmente sem incluir o secret correto.

---

## Container n8n não conecta ao Telegram

```bash
# Teste de conectividade de dentro do container
docker exec n8n node -e "
const https = require('https');
https.get('https://api.telegram.org', r => console.log('OK:', r.statusCode))
  .on('error', e => console.error('ERRO:', e.message));
"
```

Se retornar `ECONNREFUSED`, configure o Cloudflare Worker.

---

## Logs do n8n

```bash
docker logs n8n --tail 50 -f
```

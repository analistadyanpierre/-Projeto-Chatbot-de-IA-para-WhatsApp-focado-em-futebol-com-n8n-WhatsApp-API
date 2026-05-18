# -Projeto-Chatbot-de-IA-para-WhatsApp-focado-em-futebol-com-n8n-WhatsApp-API

# Workflow n8n para Lovable

Este pacote cria um workflow n8n simples para receber dados de um sistema feito no Lovable via webhook.

## Arquivos

- `workflows/lovable-atendimento-n8n.json`: workflow importavel no n8n.

## O que o workflow faz

1. Recebe um `POST` no webhook `/webhook/lovable/atendimento`.
2. Valida `name`, `email` e `message`.
3. Cria um `ticketId` interno.
4. Responde ao Lovable com sucesso ou erro em JSON.

## Como importar no n8n

1. Abra o n8n.
2. Va em **Workflows**.
3. Clique em **Import from File**.
4. Selecione `workflows/lovable-atendimento-n8n.json`.
5. Ative o workflow.

Depois de ativar, a URL de producao normalmente fica assim:

```text
https://SEU-N8N/webhook/lovable/atendimento
```

Em modo de teste no editor do n8n, use a URL exibida pelo proprio node **Webhook Lovable**.

## Contrato para usar no Lovable

Configure o formulario ou a action do Lovable para enviar:

```json
{
  "name": "Maria Silva",
  "email": "maria@email.com",
  "message": "Quero saber mais sobre o servico.",
  "source": "lovable"
}
```

Exemplo de chamada:

```js
const response = await fetch('https://SEU-N8N/webhook/lovable/atendimento', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name,
    email,
    message,
    source: 'lovable'
  })
});

const result = await response.json();
```

Resposta de sucesso:

```json
{
  "ok": true,
  "ticketId": "LV-ABC123",
  "message": "Mensagem recebida com sucesso."
}
```

Resposta de erro:

```json
{
  "ok": false,
  "errors": [
    "Informe um e-mail valido."
  ]
}
```

## Proximos passos recomendados

- Adicionar envio de e-mail depois do node **Criar ticket**.
- Adicionar cadastro em CRM ou planilha.
- Adicionar um node de IA para classificar a mensagem antes de responder.

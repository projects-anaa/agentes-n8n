# Configuracion WhatsApp + Telegram

Este workflow conserva Telegram y agrega WhatsApp Cloud API como segundo canal.

## Archivo

Importa en n8n:

`agente_dental_multicanal_telegram_whatsapp.json`

## Variables necesarias

Configura estas variables de entorno en n8n:

- `WHATSAPP_ACCESS_TOKEN`: token permanente o temporal de Meta.
- `WHATSAPP_PHONE_NUMBER_ID`: ID del numero de WhatsApp Business.
- `WHATSAPP_VERIFY_TOKEN`: texto secreto que usaras al registrar el webhook en Meta.

## Webhook de WhatsApp

El workflow agrega estos nodos:

- `WhatsApp Verify Webhook`: responde el challenge inicial de Meta.
- `WhatsApp Trigger`: recibe mensajes entrantes por POST.
- `Enviar WhatsApp`: manda la respuesta usando Graph API.

La ruta del webhook es:

`/webhook/whatsapp-clinica-dental`

En produccion, Meta necesita una URL publica HTTPS. Si n8n corre localmente, usa una URL publica temporal como ngrok, Cloudflare Tunnel o el dominio de n8n Cloud.

## Como funciona

Telegram y WhatsApp pasan por `Edit Fields 1`, que normaliza los campos internos:

- `channel`
- `message.text`
- `message.chat.id`
- `message.from.id`
- `whatsapp.phone_number_id`
- `whatsapp.from`

Despues ambos canales usan el mismo `AI Agent`, Google Calendar, Google Sheets y RAG de politicas. Al final, `Canal de salida` decide si responder por Telegram o por WhatsApp.

## Pendientes recomendados

- Validar `WHATSAPP_VERIFY_TOKEN` antes de responder el challenge.
- Ignorar webhooks de estados de WhatsApp que no traen texto.
- Agregar confirmacion doble antes de cancelar o reagendar.
- Probar con un numero sandbox antes de conectarlo a una agenda real.

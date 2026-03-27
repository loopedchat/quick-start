---
layout: default
title: API Reference
description: Queue contacts programmatically with the Looped.chat API.
---

# API Reference

## Overview

[Looped.chat](https://looped.chat) is an autonomous AI follow-up agent built for sales teams — primarily lead-heavy businesses. Its agent, **Solano**, engages new leads in seconds, nurtures cold prospects over weeks or months, and books meetings on your calendar — all via natural SMS and iMessage conversations from real phone numbers.

The API lets you feed contacts into Solano programmatically. With a single call you queue a contact, and from that point forward the system is fully autonomous: it initializes conversation context, generates a personalized first message, delivers it, handles inbound replies, and follows up over time — no further API calls needed.

---

## Authentication

Pass your API key via the `X-API-Key` header on every request.

```
X-API-Key: YOUR_API_KEY
```

---

## Queue a Contact

```
POST /queue-contact
```

This is the only endpoint you need. It creates the contact, initializes conversation context, generates the first message, and sends it to the device.

### Example Request

```bash
curl -X POST https://looped.chat/api/queue-contact \
  -H "Content-Type: application/json" \
  -H "X-API-Key: 63bd2e5c-356c-4505-9d35-aae5ee5ed700" \
  -d '{
  "first_name": "John",
  "last_name": "Doe",
  "phone": "+16031234567",
  "channel_number": "+19419465812",
  "prospect_enrichment": {
    "address": "123 Main St, Springfield"
  }
}'
```

---

## Request Body

| Field | Type | Required | Description |
|:------|:-----|:--------:|:------------|
| `first_name` | string | **Yes** | Contact's first name |
| `last_name` | string | **Yes** | Contact's last name |
| `phone` | string | **Yes** | Phone number in E.164 format (e.g. `+16031234567`) |
| `channel_number` | string | **Yes** | Your sending channel's phone number |
| `prospect_enrichment` | object | No | Extra context about the prospect as a JSON object |

---

## Response

A successful request returns the following JSON:

```json
{
  "success": true,
  "message": "Contact queued successfully",
  "data": {
    "contact_uuid": "abc-123-...",
    "phone": "+16031234567",
    "channel_number": "+19419465812",
    "context_key": "+1XXXXXXXXXX:+16031234567_context",
    "first_text": "Hey John! ...",
    "request_id": "...",
    "message_enqueued": true,
    "enqueue_error": null
  }
}
```

| Field | Description |
|:------|:------------|
| `contact_uuid` | Unique identifier for the newly created contact |
| `phone` | The contact's phone number |
| `channel_number` | The sending channel used |
| `context_key` | Internal key for the conversation context |
| `first_text` | The AI-generated first message sent to the contact |
| `request_id` | Unique identifier for this API request |
| `message_enqueued` | Whether the first message was successfully queued for delivery |
| `enqueue_error` | Error details if enqueueing failed, otherwise `null` |

---

## What Happens Next

Once queued, the system is **fully autonomous**:

1. The first message is generated and delivered to the contact.
2. Inbound replies are processed automatically.
3. AI responses are generated and sent back — no further API calls needed.

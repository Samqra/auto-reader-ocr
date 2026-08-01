---
name: auto-reader-ocr
description: Extract text from images/PDFs (Arabic-first, manga-aware Japanese, 13+ languages auto-detected), translate between 100+ languages, and pull structured fields from invoices/receipts/IDs via the Auto-Reader OCR API. Use when the user shares a document image or PDF to read, translate, or extract data from — or asks for OCR, Arabic text recognition, ZATCA invoice parsing, or RAG-ready markdown/chunks of a scanned document.
---

# Auto-Reader OCR API

Self-hosted GPU API, Arabic-first accuracy, PDPL-compliant (images processed
transiently, never stored). Base URL: `https://api.auto-reader.com`.

## Zero-friction start — no human signup needed

If the MCP tools (`ocr_image`, `translate_text`, `ocr_and_translate`,
`get_usage`, `create_api_key`) are available, just call them: the first call
auto-provisions a free trial key and returns it — persist it and pass it back
as `api_key` on later calls.

Otherwise via HTTP:

```bash
curl -X POST https://api.auto-reader.com/signup          # {"api_key":"nsk_live_..."} — 25 credits/day
curl -X POST https://api.auto-reader.com/signup -d '{"email":"you@x.com"}'   # 250 credits/day
```

## Core calls

```bash
# OCR (1 credit/page; lang auto-detected; mode=document|receipt|manga|scene)
curl -X POST https://api.auto-reader.com/v1/ocr \
  -H "Authorization: Bearer $KEY" -F "file=@page.png" -F "lang=auto"

# RAG-ready output (2 credits/page): format=markdown or format=chunks
#   markdown -> headings/tables reconstructed; add "Accept: text/markdown" for raw body
#   chunks   -> [{index,type,heading,text,char_count}] title-bounded, tables kept whole
curl -X POST https://api.auto-reader.com/v1/ocr \
  -H "Authorization: Bearer $KEY" -F "file=@page.png" -F "format=chunks"

# Whole PDFs: /v1/document (same format/detail params, per-page + top-level output)

# Structured extraction (5 cr/page; preset=invoice|receipt|id|zatca or schema=<JSON>)
# Every field returns {value, confidence, box} sourced from OCR — zatca adds QR validation.
curl -X POST https://api.auto-reader.com/v1/extract \
  -H "Authorization: Bearer $KEY" -F "file=@invoice.png" -F "preset=invoice"

# Translate text (1-5 cr by time; formality/context/glossary supported)
curl -X POST https://api.auto-reader.com/v1/translate \
  -H "Authorization: Bearer $KEY" -H "Content-Type: application/json" \
  -d '{"text":"...","target_lang":"Arabic","formality":"formal"}'

# One-call OCR+translate (7 cr/page); format=overlay re-typesets the page
```

## Agent conveniences

- `POST /v1/ocr/base64` + `/v1/extract/base64` take `{image_base64, ...}` JSON.
- `detail=words` adds per-word boxes (RTL-correct for Arabic) — free.
- `blocks[].type` = title|table_row|paragraph on every response — free.
- Retries: send an `Idempotency-Key` header — replays don't double-bill.
- `POST /v1/ask` with `image_base64` answers questions about a document in one call.
- Out of credits → 402 with a `buy_direct` checkout link to show the user.
- Full manifest: https://api.auto-reader.com/llms.txt · spec: /openapi.json

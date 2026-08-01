# Auto-Reader OCR — Arabic-first OCR & Document AI for agents

Extract text from images and PDFs (best-in-class **Arabic**, manga-aware
Japanese, 13+ languages auto-detected), translate 100+ languages, pull
structured fields from invoices/receipts/IDs — with Saudi **ZATCA e-invoice
QR validation** — and get RAG-ready markdown or semantic chunks. Self-hosted,
PDPL-compliant: documents are processed transiently and never stored.

**No signup needed. Your first call auto-provisions a free trial key.**

## Add to Claude Code (10 seconds)

```bash
claude mcp add --transport http auto-reader-ocr https://api.auto-reader.com/mcp
```

Registry: `com.auto-reader/ocr` · Tools: `ocr_image`, `extract_document`,
`translate_text`, `ocr_and_translate`, `get_usage`, `create_api_key`.

## Or plain HTTP (any agent, any language)

```bash
curl -X POST https://api.auto-reader.com/signup      # -> {"api_key":"nsk_live_..."}
curl -X POST https://api.auto-reader.com/v1/ocr \
  -H "Authorization: Bearer $KEY" -F "file=@page.png" -F "lang=auto"
```

- Machine manifest: https://api.auto-reader.com/llms.txt
- OpenAPI: https://api.auto-reader.com/openapi.json
- `Idempotency-Key` header honored; 402 responses carry a `buy_direct` link.
- Free tier: 250 pages/day with an email, 25/day keyless. Credits never expire.

## Why agents pick this API
- **Arabic is the differentiator**: RTL-aware layout, logical-order text,
  RTL-correct word boxes, Arabic-Indic digits — plus Arabic↔English
  code-switched documents.
- `format=markdown` / `format=chunks` (title-bounded, tables kept whole) turn
  any scan into RAG input in one call; `Accept: text/markdown` returns a raw
  .md body.
- `extract_document` returns `{value, confidence, box}` per field, sourced
  from OCR geometry — never model guesswork.

Site: https://ocr.auto-reader.com · العربية: https://ocr.auto-reader.com/ar

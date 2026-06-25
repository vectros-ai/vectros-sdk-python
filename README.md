# Vectros SDK for Python

[![pypi](https://img.shields.io/pypi/v/vectros)](https://pypi.org/project/vectros/)
[![license](https://img.shields.io/pypi/l/vectros)](https://www.apache.org/licenses/LICENSE-2.0)

The official Python client for the [Vectros API](https://vectros.ai) — hybrid
search, document ingestion, structured records, and grounded inference for your
application.

## Installation

```bash
pip install vectros
```

Requires Python 3.8+.

## Quick start

```python
import os
from vectros import VectrosApi

client = VectrosApi(
    base_url="https://api.vectros.ai",
    token=os.environ["VECTROS_API_KEY"],  # sk_live_... or sk_test_...
)

# Hybrid (keyword + semantic) search over your indexed content
results = client.search.content(
    query="patient intake form diabetes",
)

# Ingest a document — extracted, chunked, and indexed for search + RAG
doc = client.documents.ingest_document(
    title="Patient Intake Form — Jane Doe",
)

# Write a structured record against one of your schemas
record = client.records.create_record(
    type_name="intake_form",
    schema_id="6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    payload={"first_name": "Jane", "email": "jane@example.com"},
)
```

An `AsyncVectrosApi` with the same surface is available for `asyncio`.

## Authentication

The SDK sends whatever credential you pass in the `Authorization: Bearer <token>`
header. Two credential types are accepted:

| Type | Prefix | Lifetime | Use from |
|------|--------|----------|----------|
| **API key** | `sk_live_*` / `sk_test_*` | Long-lived | **Server only** — full tenant access |
| **Scoped token** | `st_*` | Short-lived | **Server or browser** — narrowed scope, auto-expiring |

Keep API keys server-side only. For untrusted runtimes, mint a short-lived
scoped token on your backend and pass it as `token`. See the
[authentication guide](https://docs.vectros.ai) for the full pattern.

## What you can do

- **Hybrid search & RAG** — `client.search`, `client.inference` — vector + keyword
  search and grounded document Q&A over your indexed corpus.
- **Documents & folders** — `client.documents`, `client.folders` — ingest,
  organize, retrieve, and look documents up by field.
- **Structured records** — `client.records` — create, read, update (full and
  partial), delete, and look records up by indexed field.
- **Schemas** — `client.schemas` — define and evolve record/document schemas.
- **Identity & access** — `client.identity`, `client.auth` — manage clients,
  organizations, and users; mint and revoke scoped credentials.

## Full API reference

Every method, parameter, and type is documented in
[`reference.md`](./reference.md).

## Documentation

- **Guides & reference:** [docs.vectros.ai](https://docs.vectros.ai)
- **Product:** [vectros.ai](https://vectros.ai)

## License

[Apache License 2.0](./LICENSE).

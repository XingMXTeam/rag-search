# RAG Search API

> A small FastAPI service that combines web search, vector reranking, detail fetching, and content filtering into one RAG-oriented search endpoint.

This repository is a learning and experimentation project inspired by ThinkAny-style RAG search systems. It is useful as a compact reference for building a search-enhanced AI backend.

## What it does

The API exposes a `/rag-search` endpoint that can:

- call a web search provider through Serper
- optionally rerank search results with embeddings / vector similarity
- optionally fetch page details from result URLs
- optionally filter detailed content against the original query
- return normalized search results for downstream RAG workflows

## Workflow

```text
User query
  -> Web search
  -> Optional vector reranking
  -> Optional detail fetching
  -> Optional content filtering
  -> RAG-ready search results
```

## Main endpoint

```text
POST /rag-search
Authorization: Bearer <AUTH_API_KEY>
```

Request body:

```json
{
  "query": "ThinkAny.AI",
  "locale": "en",
  "search_n": 10,
  "search_provider": "google",
  "is_reranking": true,
  "is_detail": true,
  "detail_min_score": 0.7,
  "detail_top_k": 3,
  "is_filter": true,
  "filter_min_score": 0.8,
  "filter_top_k": 6
}
```

## Environment variables

Create a `.env` file in the project root:

```ini
SERPER_API_KEY=

OPENAI_BASE_URL=
OPENAI_API_KEY=
OPENAI_MODEL=gpt-3.5-turbo
OPENAI_EMBED_MODEL=text-embedding-ada-002

ZILLIZ_URI=
ZILLIZ_TOKEN=
ZILLIZ_DIM=1536
ZILLIZ_COLLECTION=

AUTH_API_KEY=
```

## Quick start

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the FastAPI server:

```bash
uvicorn main:app --reload --port 8069
```

Health check:

```http
GET http://127.0.0.1:8069/
```

Example request:

```http
POST http://127.0.0.1:8069/rag-search
Content-Type: application/json
Authorization: Bearer <AUTH_API_KEY>

{
  "query": "ThinkAny.AI",
  "search_n": 10,
  "is_reranking": true,
  "is_detail": true,
  "is_filter": true
}
```

## Implementation notes

- `main.py` creates the FastAPI app and mounts the RAG router.
- `handlers/rag_search.py` contains the `/rag-search` workflow.
- Search results are first fetched from the configured search provider.
- Reranking stores search results into a vector index and queries them by semantic similarity.
- Detail fetching loads content from selected URLs.
- Filtering keeps the most relevant detailed content for the original query.

## Project status

This is a compact RAG backend experiment, not a production-ready service yet.

Potential next steps:

- Add tests for the search / reranking / filtering flow.
- Add provider abstraction for more search engines.
- Add better error handling and observability.
- Add Dockerfile and deployment example.
- Add an example client for agent workflows.

## Related

- GitHub profile: [XingMXTeam](https://github.com/XingMXTeam)
- Personal site: [maoxunxing.com](https://maoxunxing.com)

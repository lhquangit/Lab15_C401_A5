# Model Routing Matrix

| Use Case | Request Type | Router Logic | Model | Reranker | Judge |
|---|---|---|---|---|---|
| Admin Q&A | Common Question | Cache/Vector Search | GPT-4o-mini | No | No |
| Admin Q&A | Policy Question | RAG | GPT-4o-mini | No | No |
| SOP Q&A | Standard | RAG | GPT-4o | Yes | Yes |
| Record Summary | Patient Record | Template RAG | GPT-4o | No | Yes |
| All | Fallback | Provider Error/Latency | GPT-4o-mini | No | No |

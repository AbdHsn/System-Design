# 🏗️ System Design স্টাডি নোটস (বাংলা)

৬০টি বাস্তব system design সমস্যা নিয়ে বাংলায় লেখা স্টাডি নোটস। প্রতিটি টপিকে আছে — সমস্যার প্রেক্ষাপট, মূল ধারণা, diagram, trade-off বিশ্লেষণ, আর interview টিপস।

> **Note:** এগুলো original নোট — কোনো বইয়ের অনুবাদ নয়। কনসেপ্টগুলো industry-standard system design টপিক।

## 📖 কীভাবে পড়বেন (Study Guideline)

1. **প্রতিদিন ১টি টপিক** — তাড়াহুড়ো করবেন না। একটা টপিক পড়ে নিজের ভাষায় সমস্যাটা explain করার চেষ্টা করুন।
2. **আগে নিজে ভাবুন** — টপিকের "সমস্যা" অংশ পড়ে থেমে যান। নিজে হলে কী করতেন, সেটা ভেবে তারপর সমাধান পড়ুন।
3. **Diagram নিজে আঁকুন** — কাগজে বা whiteboard-এ diagram টা memory থেকে আঁকার চেষ্টা করুন। Interview-তে এটাই করতে হবে।
4. **Trade-off মুখস্থ নয়, বুঝুন** — "X ভালো" এমন উত্তর interview-তে fail করে। "X ভালো যখন..., কিন্তু Y ভালো যখন..." — এভাবে ভাবা practice করুন।
5. **সপ্তাহে ১ দিন revision** — প্রতি ৭ দিনে আগের ৬টা টপিক ঝালিয়ে নিন।
6. **নিজের প্রজেক্টের সাথে মেলান** — প্রতিটা কনসেপ্ট পড়ে ভাবুন: আমার কাজ করা কোন প্রজেক্টে এই সমস্যাটা ছিল বা হতে পারত?

## 🗂️ Index

### Week 1–2: Foundation Patterns
| Day | Topic | Status |
|-----|-------|--------|
| 01 | [Mobile থেকে Backend Decoupling (API Gateway vs BFF)](notes/day-01-api-gateway-bff.md) | ✅ |
| 02 | [N+1 Query Problem](notes/day-02-n-plus-1-query.md) | ✅ |
| 03 | [Rate Limiting ও Boundary Burst](notes/day-03-rate-limiting.md) | ✅ |
| 04 | [Duplicate Payment ঠেকানো (Idempotency)](notes/day-04-duplicate-payment.md) | ✅ |
| 05 | [Database Sharding Strategy](notes/day-05-sharding-strategy.md) | ✅ |
| 06 | [Cron Job-এর জন্য Distributed Lock](notes/day-06-distributed-locks.md) | ✅ |
| 07 | [Message Queue-তে Event Ordering](notes/day-07-event-ordering.md) | ✅ |
| 08 | [Cache আর Database Sync রাখা](notes/day-08-cache-db-sync.md) | ✅ |
| 09 | [Read/Write Model আলাদা করা (CQRS)](notes/day-09-cqrs.md) | ✅ |
| 10 | [Distributed Transactions (Saga)](notes/day-10-distributed-transactions.md) | ✅ |

### Week 3–4: Scaling & Reliability
| Day | Topic | Status |
|-----|-------|--------|
| 11 | Webhook Retry Idempotently হ্যান্ডেল করা | ⬜ |
| 12 | High-Ingest Table-এ Indexing | ⬜ |
| 13 | Shared Connection Pool ম্যানেজ করা | ⬜ |
| 14 | Risky Change নিরাপদে Rollout করা | ⬜ |
| 15 | দ্রুত "User কি এটা দেখেছে?" চেক (Bloom Filter) | ⬜ |
| 16 | Hot Partition Key সামলানো | ⬜ |
| 17 | Overwhelmed Consumer-এ Backpressure | ⬜ |
| 18 | Cache Stampede থেকে বাঁচা | ⬜ |
| 19 | Read Replica-তে Read-Your-Writes | ⬜ |
| 20 | Failing Downstream Dependency আটকানো (Circuit Breaker) | ⬜ |

### Week 5–6: Messaging, Feeds & Data
| Day | Topic | Status |
|-----|-------|--------|
| 21 | Real-Time Streaming Transport বাছাই (WebSocket/SSE) | ⬜ |
| 22 | Service-এর মধ্যে Reliable Messaging (Outbox) | ⬜ |
| 23 | Celebrity Account-এর Feed Fanout | ⬜ |
| 24 | বড় Result Set-এ Efficient Pagination | ⬜ |
| 25 | Traffic Spike-এ Queue Backpressure | ⬜ |
| 26 | Write-Path Cache Consistency | ⬜ |
| 27 | LLM-এর উত্তর Up-to-Date রাখা (RAG) | ⬜ |
| 28 | Semantic Search-এর জন্য Vector Store বাছাই | ⬜ |
| 29 | Multi-Agent Workflow Coordination | ⬜ |
| 30 | File Storage Backend বাছাই | ⬜ |

### Week 7–8: Infrastructure & Advanced Patterns
| Day | Topic | Status |
|-----|-------|--------|
| 31 | Cross-Region Latency কমানো | ⬜ |
| 32 | Secrets ও Credentials ম্যানেজমেন্ট | ⬜ |
| 33 | Event Sourcing দিয়ে State Reconstruction | ⬜ |
| 34 | LLM Classification Accuracy বাড়ানো | ⬜ |
| 35 | Geospatial "Nearby Drivers" at Scale | ⬜ |
| 36 | Edge-এ HTTP/3 vs HTTP/2 | ⬜ |
| 37 | Collaborative Editing-এ Multi-Writer Conflict (CRDT) | ⬜ |
| 38 | বড় Document LLM-এ ফিট করানো | ⬜ |
| 39 | Concurrent Balance Overspend ঠেকানো | ⬜ |
| 40 | Main Thread থেকে ভারী কাজ সরানো | ⬜ |

### Week 9–10: Streaming, AI & Migration
| Day | Topic | Status |
|-----|-------|--------|
| 41 | Batch থেকে Real-Time Streaming-এ যাওয়া | ⬜ |
| 42 | AI Agent-এর Memory ডিজাইন | ⬜ |
| 43 | Deploy-এ Reliable CDN Cache Invalidation | ⬜ |
| 44 | Offline Edit Sync (Data Loss ছাড়া) | ⬜ |
| 45 | Full-Text Search Scale করা | ⬜ |
| 46 | LLM থেকে Structured Output আদায় | ⬜ |
| 47 | Monolith-কে ধীরে ধীরে ভাঙা (Strangler Fig) | ⬜ |
| 48 | Agent কীভাবে Tool বাছাই করে | ⬜ |
| 49 | Data-Warehouse Query Cost নিয়ন্ত্রণ | ⬜ |
| 50 | High Throughput-এ ML Model Serving | ⬜ |

### Week 11–12: SaaS, Payments & Operations
| Day | Topic | Status |
|-----|-------|--------|
| 51 | Multi-Tenant SaaS-এ Noisy Tenant Isolation | ⬜ |
| 52 | Client না ভেঙে API Versioning | ⬜ |
| 53 | Zero-Downtime Schema Migration | ⬜ |
| 54 | RAG-এ Embedding Drift হ্যান্ডেল করা | ⬜ |
| 55 | Shared State-সহ Parallel Agents | ⬜ |
| 56 | Long-Running Async Job Tracking | ⬜ |
| 57 | Payment-Confirmation Read-এর Consistency | ⬜ |
| 58 | LLM Agent Pipeline-এর Observability | ⬜ |
| 59 | Payment-এর জন্য Idempotency Key ডিজাইন | ⬜ |
| 60 | Scratch থেকে একটা SaaS Platform আর্কিটেক্ট করা | ⬜ |

---

*Diagram গুলো Mermaid-এ লেখা — GitHub-এ automatically render হবে।*

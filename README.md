# 🚀 TransactFlow — Modern Distributed Payment Processing Platform

<div align="center">

[![Go Version](https://img.shields.io/badge/Go-1.26+-00ADD8?style=for-the-badge&logo=go)](https://golang.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react)](https://reactjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-316192?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis)](https://redis.io/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

**A production-grade distributed e-commerce and payment processing system built with Go**

[Features](#-key-features) • [Architecture](#-architecture) • [Tech Stack](#-tech-stack) • [Quick Start](#-quick-start) • [Documentation](#-documentation)

</div>

---

## 📋 Overview

TransactFlow is a modern, full-stack order and payment processing platform designed with distributed systems principles at its core. Built by **Keshav**, this project demonstrates advanced concepts in payment engineering, event-driven architectures, and cloud-native application design.

### 🎯 What It Does

TransactFlow handles the complete order-to-payment lifecycle with real-time status updates:

1. **Product Browsing** — Customers explore the product catalog
2. **Cart Management** — Add items and proceed to checkout
3. **Order Processing** — Distributed saga handles the order workflow
4. **Inventory Management** — Optimistic locking prevents overselling
5. **Payment Processing** — Secure Stripe integration with idempotency
6. **Webhook Handling** — Asynchronous payment confirmation
7. **Real-time Updates** — Server-Sent Events stream order status changes
8. **Admin Dashboard** — Monitor system health, queue depths, and AI-powered insights

### 🎭 Demo Credentials

| Role | Email | Password |
|------|-------|----------|
| 👨‍💼 Admin | `admin@transactflow.dev` | `demo-admin-123` |
| 🛒 Customer | `customer@transactflow.dev` | `demo-customer-123` |

---

## ✨ Key Features

### 🔄 Distributed Systems Patterns

- **Saga Pattern** — Multi-step order processing with automatic compensation
- **Event Sourcing** — Complete audit trail of all state transitions
- **CQRS** — Separate read and write operations for scalability
- **Circuit Breaker** — Resilient external API calls with fallback logic

### 💳 Payment Engineering Best Practices

- **Idempotency Keys** — Safe retry logic for all payment operations
- **Webhook Deduplication** — Reliable event processing without duplicates
- **Money in Cents** — Precise financial calculations with integer arithmetic
- **Daily Reconciliation** — Automated discrepancy detection against Stripe
- **Distributed Locking** — Prevent race conditions in payment creation
- **Partial Refunds** — Flexible refund handling with separate idempotency

### 🎨 Modern Frontend Experience

- **React 19** — Latest React with concurrent features
- **Custom Design System** — "Organic" theme with Tailwind CSS
- **Real-time Updates** — Server-Sent Events for instant order tracking
- **Responsive Design** — Mobile-first UI with elegant animations
- **TanStack Query** — Intelligent data fetching and caching

### 🤖 AI-Powered Insights

- **Claude Integration** — AI-generated payment analytics
- **Anomaly Detection** — Automatic identification of unusual patterns
- **Smart Caching** — Redis-backed insight cache for performance
- **Admin-Only Access** — Secure, role-based feature access

### 📊 Observability & Monitoring

- **OpenTelemetry** — Distributed tracing across all services
- **Prometheus Metrics** — Real-time system performance monitoring
- **Grafana Dashboards** — Visual system health analytics
- **Structured Logging** — JSON-formatted logs for easy parsing
- **Jaeger Tracing** — End-to-end request visualization

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                     React 19 Frontend                         │
│   🛍️ Catalog  │  🛒 Checkout  │  📦 Tracking  │  ⚙️ Admin   │
└─────────────────────────────┬────────────────────────────────┘
                              │ HTTPS + SSE
┌─────────────────────────────▼────────────────────────────────┐
│                      Go API Service                           │
│  /products  /orders  /payments  /webhooks  /admin  /metrics  │
└──┬──────────┬──────────────┬────────────┬───────────────────┘
   │          │              │            │
   ▼          ▼              ▼            ▼
┌─────┐  ┌──────┐      ┌────────┐  ┌────────────┐
│ 🐘  │  │  ⚡  │      │  💳    │  │     📡     │
│ Pg  │  │Redis │      │ Stripe │  │  OTel      │
└─────┘  └──────┘      └────────┘  └────────────┘
  │         │              │              │
  │    Cache, Queues,      │         Traces → Jaeger
  │    Locks, Pub/Sub      │         Metrics → Prometheus
  │                        │                  → Grafana
  │                        │
  └────────────────────────┴───────────────────────┐
                                                    │
                       ┌────────────────────────────▼─────┐
                       │     Go Worker Service             │
                       │  • inventory_worker               │
                       │  • payment_worker                 │
                       │  • webhook_worker                 │
                       │  • refund_worker                  │
                       │  • reconcile_worker               │
                       └───────────────────────────────────┘
```

### 📊 Order State Machine

Orders flow through a strictly enforced state machine with automatic rollback on failures:

```
created → inventory_reserved → payment_processing → 
payment_captured → confirmed → fulfilled
                    ↓ (on failure)
            cancelled + inventory released
```

---

## 🔧 Tech Stack

<table>
<tr>
<td><strong>Category</strong></td>
<td><strong>Technology</strong></td>
</tr>
<tr>
<td>🖥️ Backend</td>
<td>Go 1.26, Chi Router, pgx/v5</td>
</tr>
<tr>
<td>💾 Database</td>
<td>PostgreSQL 16</td>
</tr>
<tr>
<td>⚡ Cache & Queue</td>
<td>Redis 7 (Streams, Pub/Sub, Locks)</td>
</tr>
<tr>
<td>💳 Payments</td>
<td>Stripe (Test Mode)</td>
</tr>
<tr>
<td>🎨 Frontend</td>
<td>React 19, TanStack Query, React Router v7</td>
</tr>
<tr>
<td>🎭 Styling</td>
<td>Tailwind CSS v3, Custom "Organic" Design System</td>
</tr>
<tr>
<td>🤖 AI</td>
<td>Anthropic Claude (claude-sonnet-5)</td>
</tr>
<tr>
<td>📊 Observability</td>
<td>OpenTelemetry, Prometheus, Grafana, Jaeger</td>
</tr>
<tr>
<td>🐳 Containers</td>
<td>Docker, Docker Compose</td>
</tr>
<tr>
<td>🚀 CI/CD</td>
<td>GitHub Actions, GHCR</td>
</tr>
<tr>
<td>🧪 Testing</td>
<td>Go testing, k6 load testing</td>
</tr>
<tr>
<td>🔍 Linting</td>
<td>golangci-lint v2</td>
</tr>
</table>

---

## 🚀 Quick Start

### Prerequisites

- **Go 1.26+**
- **Docker & Docker Compose**
- **Node.js 20+**
- **Stripe Account** (for payment processing)

### 🏃 Local Development

```bash
# 1️⃣ Clone the repository
git clone https://github.com/keshav/transactflow.git
cd transactflow

# 2️⃣ Start infrastructure services
make dev            # PostgreSQL, Redis, Prometheus, Grafana, Jaeger

# 3️⃣ Run database migrations
make migrate

# 4️⃣ Seed demo data
make seed

# 5️⃣ Configure environment
cp .env.example .env
# Edit .env and add your Stripe keys

# 6️⃣ Start the API (terminal 1)
make api            # http://localhost:8080

# 7️⃣ Start the worker (terminal 2)
make worker

# 8️⃣ Start the frontend (terminal 3)
make frontend-install
make frontend       # http://localhost:5173

# 9️⃣ Forward Stripe webhooks (terminal 4, optional)
make stripe-listen
```

### 🎯 Available Make Commands

```bash
make dev              # Start infrastructure
make api              # Run API server
make worker           # Run background workers
make frontend         # Run frontend dev server
make migrate          # Run database migrations
make seed             # Seed demo data
make test             # Run all tests
make test-race        # Run tests with race detector
make lint             # Run golangci-lint
make build            # Build binaries
```

---

## 🏛️ Project Structure

```
transactflow/
├── 📁 cmd/
│   ├── api/              # API server entrypoint
│   ├── worker/           # Background worker entrypoint
│   ├── migrate/          # Database migration runner
│   └── seed/             # Demo data seeder
├── 📁 internal/
│   ├── api/
│   │   ├── handlers/     # HTTP request handlers
│   │   ├── middleware/   # Auth, CORS, rate limiting
│   │   └── server.go     # Router setup
│   ├── domain/
│   │   ├── order/        # Order business logic
│   │   ├── payment/      # Payment processing
│   │   ├── product/      # Product & inventory
│   │   ├── reconciliation/  # Payment reconciliation
│   │   └── user/         # User management
│   ├── worker/
│   │   ├── inventory_worker.go    # Stock management
│   │   ├── payment_worker.go      # Payment creation
│   │   ├── webhook_worker.go      # Stripe event handling
│   │   ├── refund_worker.go       # Refund processing
│   │   └── reconcile_worker.go    # Daily reconciliation
│   ├── store/
│   │   ├── postgres/     # PostgreSQL implementations
│   │   └── redis/        # Redis cache & locks
│   ├── insights/         # AI-powered analytics
│   ├── queue/            # Redis Streams wrapper
│   ├── stripe/           # Stripe client
│   └── observability/    # Logging & tracing
├── 📁 frontend/          # React application
├── 📁 migrations/        # SQL migrations
├── 📁 k6/               # Load testing scripts
├── 📁 .github/workflows/ # CI/CD pipelines
├── 🐳 docker-compose.yml
└── 📄 README.md
```

---

## 🧪 Testing

### Unit & Integration Tests

```bash
# Run all tests
make test

# Run with race detector (detects concurrency issues)
make test-race

# Run specific package tests
go test ./internal/domain/product/... -v

# Test coverage
go test ./... -cover
```

### Load Testing

```bash
# Install k6
brew install k6  # macOS
# or download from https://k6.io/

# Run load test
k6 run --out json=k6/results.json k6/load-test.js

# Custom configuration
k6 run --vus 50 --duration 5m k6/load-test.js
```

**Load Test Results** (20 VUs, 3 minutes):

| Metric | Value |
|--------|-------|
| Total Orders | 3,388 |
| Throughput | 18.75 req/s |
| p50 Latency | 39 ms |
| p95 Latency | 52 ms |
| p99 Latency | 78 ms |
| Error Rate | 0% |

---

## 🔍 Technical Highlights

### 1. Optimistic Locking for Inventory

Prevents overselling without distributed locks:

```sql
UPDATE inventory 
SET reserved = reserved + $amount, version = version + 1
WHERE product_id = $id 
  AND version = $expected_version
  AND (quantity - reserved) >= $amount;
```

If another worker updated first, the `version` check fails — retry up to 3 times.

### 2. Idempotency at Every Layer

- **API Level**: All state-changing requests require `Idempotency-Key` header
- **Database Level**: Unique constraints on idempotency keys
- **Redis Cache**: Fast-path check before database lookup
- **Stripe Level**: Deterministic keys (`payment:{order_id}`)

### 3. Event Sourcing for Audit Trail

```go
// Every state change is append-only
type OrderEvent struct {
    ID        uuid.UUID
    OrderID   uuid.UUID
    Status    string
    CreatedAt time.Time
    Metadata  json.RawMessage
}
```

Full history queryable for compliance and debugging.

### 4. Webhook Deduplication

```
1. Validate Stripe signature
2. Check if event ID exists in processed_webhook_events
3. If new: INSERT event ID, enqueue to Redis Streams
4. Return 200 OK in <5ms
5. Worker processes asynchronously
```

### 5. Real-time Order Tracking

Server-Sent Events stream order updates to browsers:

```javascript
const eventSource = new EventSource('/orders/123/events/stream');
eventSource.addEventListener('status', (e) => {
    const { status, timestamp } = JSON.parse(e.data);
    updateUI(status, timestamp);
});
```

### 6. Distributed Locking

Prevent duplicate payments with Redis atomic operations:

```go
// Only one worker can acquire the lock
locked, err := redis.SetNX(
    ctx, 
    fmt.Sprintf("payment:lock:%s", orderID),
    "1",
    30*time.Second,
).Result()
```

---

## 📚 Documentation

- [**ARCHITECTURE.md**](./ARCHITECTURE.md) — Detailed system design and patterns
- [**DEPLOY.md**](./DEPLOY.md) — Production deployment guide
- [**k6/LOAD_TEST_CASE_STUDY.md**](./k6/LOAD_TEST_CASE_STUDY.md) — Load testing insights

---

## 🔄 CI/CD Pipeline

### Continuous Integration

Every push triggers automated checks:

```
Push → Lint (golangci-lint) → Test (go test -race) → 
Build (API + Worker) → Frontend Build (tsc + vite)
```

### Continuous Deployment

Merge to `main` automatically deploys:

```
CI Pass → Build Docker Images → Push to GHCR → 
SSH to Server → docker compose pull → docker compose up
```

Images tagged with both `:latest` and `:<git-sha>` for easy rollback.

---

## 🎨 Design System — "Organic"

TransactFlow uses a custom Tailwind-based design system:

### Color Palette

- 🧡 **Terracotta Accent**: `#c67139`
- 🌿 **Sage Accent**: `#7a8a5e`
- 🏜️ **Warm Cream Background**: `#f5ead8`

### Typography

- **Headings**: Caprasimo (Google Fonts)
- **Body**: Figtree (Google Fonts)

### Border Radius Scale

- `pf-sm`: 8px
- `pf-md`: 16px
- `pf-lg`: 28px

### Custom Animations

- `pf-pop`, `pf-pulse`, `pf-spin`, `pf-shimmer`, `pf-fade`

---

## 🔐 Security Features

- ✅ **HTTPS Only** — All production traffic encrypted
- ✅ **JWT Authentication** — Secure session management
- ✅ **CORS Protection** — Configured allowed origins
- ✅ **Rate Limiting** — Prevent API abuse
- ✅ **SQL Injection Protection** — Parameterized queries
- ✅ **Webhook Signature Validation** — Verify Stripe events
- ✅ **Password Hashing** — bcrypt with appropriate cost
- ✅ **Secret Management** — Environment-based configuration

---

## 📈 Performance Optimization

- **Redis Caching** — Product catalog cached for fast reads
- **Connection Pooling** — Reused database connections
- **Horizontal Scalability** — Stateless API and workers
- **Optimistic Locking** — No blocking on inventory operations
- **Async Workers** — Non-blocking order processing
- **CDN-Ready** — Static assets optimized for CDN delivery

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Keshav**

- 🌐 Portfolio: [Your Portfolio URL]
- 💼 LinkedIn: [Your LinkedIn]
- 🐙 GitHub: [@keshav](https://github.com/keshav)
- 📧 Email: keshav@example.com

---

## 🙏 Acknowledgments

- Built with Go and modern cloud-native technologies
- Inspired by distributed systems patterns from industry leaders
- Powered by open-source software

---

<div align="center">

**⭐ Star this repository if you find it helpful!**

Made with ❤️ by Keshav

</div>

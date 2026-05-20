<div align="center">

# AI Engineering Portfolio

### Production-grade AI systems — built, tested, and Azure-deployable

[![Azure Ready](https://img.shields.io/badge/Azure-Container_Apps_%7C_OpenAI_%7C_Key_Vault-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com)
[![FastAPI](https://img.shields.io/badge/FastAPI-Production_APIs-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![LangGraph](https://img.shields.io/badge/LangGraph-Multi--Agent_Orchestration-6366f1)](https://langchain-ai.github.io/langgraph)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF?logo=githubactions&logoColor=white)](https://github.com/features/actions)

**Every project ships with authenticated APIs, observable infrastructure, Azure Bicep IaC, CI/CD pipelines, and handoff documentation.**

[LLM Observatory](#1-llm-observatory) · [Enterprise RAG](#2-enterprise-rag) · [Document Intelligence](#3-document-intelligence-workbench) · [ProcureAI](#4-procureai) · [Multi-Agent Orchestration](#5-multi-agent-orchestration-platform)

> Live demo using your sample data available on request.

</div>

---

## What separates these from typical AI demos

Most AI prototypes solve the easy part — getting an LLM to return an answer. These projects solve the production part:

| Typical demo | This portfolio |
|---|---|
| Hardcoded API keys | Azure Key Vault + Managed Identity |
| SQLite, no migrations | PostgreSQL with Alembic migrations |
| No auth | JWT + API keys + OIDC/Entra ID |
| `print()` for logging | Structured JSON logs + Application Insights |
| No tests | pytest + integration tests + golden eval datasets |
| `docker run` instructions | Bicep IaC, Container Apps, deploy pipelines |
| "It works on my machine" | CI builds, staging deploys, smoke tests |

---

## Projects

<br>

### 1. LLM Observatory

**Production monitoring for LLM-powered features — hallucination risk, latency, cost, and evaluation drift.**

![LLM Observatory Dashboard](projects/llm-observatory/llm-observatory-dashboard.png)

#### The problem
LLM features degrade silently. A prompt change, model update, or traffic spike raises hallucination rates and cost before any health check notices. Engineering teams running multiple LLM endpoints — support, sales, research — have no visibility into which endpoint is expensive, which responses are low quality, or whether quality is trending down.

#### What it delivers

| Capability | Detail |
|---|---|
| **Gateway + ingest modes** | Route traffic through the gateway or submit completed traces — works with any LLM provider without changing application code |
| **Automated quality evaluation** | Faithfulness, relevancy, and hallucination scoring applied per trace using LLM-as-judge with deterministic fallback |
| **Alerting with incident lifecycle** | Configurable thresholds trigger incidents; incidents tracked through acknowledge → resolve with Slack notification |
| **Per-endpoint cost tracking** | Token usage and USD cost per endpoint, model, and time window |
| **Role-based access** | Owner, admin, engineer, and analyst roles with scoped permissions |

#### Azure deployment target
Azure Container Apps · Azure OpenAI · PostgreSQL Flexible Server · Key Vault · Application Insights

[→ Read the full brief](projects/llm-observatory/brief.md)

---

### 2. Enterprise RAG

**Secure document Q&A — citation-backed answers with role-based access control enforced at the retrieval layer.**

![Permission Test](projects/enterprise-rag/enterprise-rag-permission-test.png)
*Same question, two roles. The finance forecast is never retrieved for the employee role — not filtered from the response, but excluded from the candidate set entirely.*

![Citations](projects/enterprise-rag/enterprise-rag-citations.png)
*Every answer names the source document, chunk, and retrieval score. No answer without a citation.*

#### The problem
Enterprise knowledge is scattered across policy documents, finance reports, security runbooks, and HR guides. Employees waste time searching or get inconsistent answers from colleagues. The deeper problem: most RAG systems apply access control after retrieval — a contractor can still retrieve restricted documents if the question is phrased correctly.

#### What it delivers

| Capability | Detail |
|---|---|
| **Pre-retrieval access control** | Restricted documents are excluded from the candidate set before any AI processing — not filtered from the response after the fact |
| **Citation-backed answers** | Every response identifies the source document, chunk ID, and retrieval score — answers are traceable and auditable |
| **Measurable retrieval quality** | Built-in evaluation set tracks citation coverage, answer accuracy, and restricted document leakage |
| **Enterprise auth** | API keys, JWT, OIDC, and Azure Entra ID — configurable per deployment |
| **Rate limiting** | Per-client rate limiting with configurable thresholds |

#### Azure deployment target
Azure Container Apps · Azure OpenAI · Azure AI Search · Entra ID · Key Vault · PostgreSQL

[→ Read the full brief](projects/enterprise-rag/brief.md)

---

### 3. Document Intelligence Workbench

**Structured extraction from invoices and contracts — with confidence scoring, rule validation, and human review routing.**

![Dashboard](projects/document-intelligence/workbench-dashboard.png)
*Extraction metrics, per-document confidence scores, field-level validation results, and review queue — one view.*

![Review Table](projects/document-intelligence/workbench-review-table.png)
*Reviewers see exactly which fields need attention and why — corrections logged to an immutable audit trail.*

#### The problem
Finance and legal teams manually key data from invoices and contracts into downstream systems. The work is repetitive, error-prone, and produces no audit record of what was extracted, what was corrected, or who changed it. Reconciliation issues surface downstream — totals that don't match, dates that contradict each other, no traceable history.

#### What it delivers

| Capability | Detail |
|---|---|
| **Confidence score per field** | Every extracted field carries a confidence score — not a document-level pass/fail |
| **Business rule validation** | Invoice totals must match line items; contract expiry must follow effective date; required fields must be present |
| **Human review queue** | Low-confidence and failed records route to reviewers before reaching downstream systems |
| **Immutable audit trail** | Every extraction, correction, and approval logged with timestamp, actor, and structured payload |
| **Async processing** | Queue-based pipeline with retry logic and dead-letter handling for large document volumes |

#### Azure deployment target
Azure Container Apps · Azure Document Intelligence · Blob Storage · PostgreSQL · Key Vault

[→ Read the full brief](projects/document-intelligence/brief.md)

---

### 4. ProcureAI

**Five specialist agents validate purchase requests against policy, budget, vendor risk, and approval authority — with a full evidence trail.**

![Dashboard](projects/procureai/procureai-dashboard.png)
![Workflow Result](projects/procureai/procureai-workflow-result.png)
*Every approval decision comes with a structured evidence packet — policy citations, budget figures, vendor status, risk scores, and required approvers.*

#### The problem
Purchase approvals at most organisations rely on institutional memory, not the actual policy document. Budget checks are done by emailing finance. Vendor risk is assumed. When a regulator asks why a $200,000 purchase was approved, the answer is in an email thread.

#### What it delivers

| Capability | Detail |
|---|---|
| **Five independent specialist agents** | Policy, Vendor, Budget, Risk, and Approval agents each validate one domain — typed inputs, structured outputs, no monolithic logic |
| **Policy-grounded decisions** | The Policy Agent retrieves the exact clause from indexed policy documents — decisions cite their source, not generic rules |
| **MCP-compatible tool layer** | Any external AI agent, copilot, or orchestration system can call individual checks without going through the full pipeline |
| **A2A agent coordination** | Agents communicate via typed message schemas — no string parsing, no hidden state |
| **Evidence packet per request** | Every approval or rejection produces a structured document — auditable, exportable, regulator-ready |

#### Azure deployment target
Azure Container Apps · Azure OpenAI · PostgreSQL · Key Vault · Application Insights

[→ Read the full brief](projects/procureai/brief.md)

---

### 5. Multi-Agent Orchestration Platform

**LangGraph-based workflow automation with persistent human-in-the-loop approvals and tamper-evident audit trails.**

![Dashboard](projects/multi-agent-workflow/dashboard.png)

![Audit Trail](projects/multi-agent-workflow/audit-trail.png)
*Every agent action, rule evaluation, risk score, and human decision — timestamped, actor-tagged, and immutable.*

#### The problem
Enterprise workflows that cross systems rely on email chains and spreadsheet trackers. There is no persistent state, no guaranteed audit trail, and no operational visibility into where a workflow is, who touched it, or why a decision was made. When something goes wrong, the answer is in someone's inbox.

#### What it delivers

| Capability | Detail |
|---|---|
| **Persistent workflow state** | Workflows pause at human decision points and resume exactly where they stopped — across server restarts, deployments, and failures |
| **Human-in-the-loop as an architectural primitive** | Not a polling loop or a workaround — a genuine interrupt in the LangGraph state machine with typed resume payloads |
| **Pluggable workflow modules** | Compliance review and procurement workflows included; new workflow types addable without changing the platform |
| **Tamper-evident audit trail** | Every agent action and human decision logged with timestamp, actor, and structured payload — append-only |
| **56 passing tests** | Unit tests per agent node, integration tests per workflow, state transition tests |

#### Azure deployment target
Azure Container Apps · Azure OpenAI / Anthropic · PostgreSQL (LangGraph checkpoint store) · Key Vault

[→ Read the full brief](projects/multi-agent-workflow/brief.md)

---

## Technical baseline

Every project in this portfolio ships with the same production foundation:

<div align="center">

| Layer | Standard |
|---|---|
| **API** | FastAPI · typed Pydantic schemas · OpenAPI docs |
| **Auth** | JWT · API keys · OIDC · Azure Entra ID |
| **Database** | PostgreSQL · Alembic migrations · SQLite for local dev |
| **Containers** | Docker · docker-compose · named volumes |
| **CI/CD** | GitHub Actions · lint · test · build · deploy-staging · deploy-prod |
| **Infrastructure** | Azure Bicep · Container Apps · Key Vault · PostgreSQL · ACR |
| **Observability** | Prometheus metrics · Application Insights · structured JSON logs |
| **LLMOps** | Langfuse tracing · token cost tracking · golden eval datasets |
| **Security** | Managed Identity · no hardcoded secrets · RBAC · audit logging |
| **Testing** | pytest · integration tests · eval datasets per project |

</div>

---

## Engagement

<div align="center">

| | Fixed-scope delivery | Custom implementation | Azure deployment |
|---|---|---|---|
| **What it is** | Defined deliverables, timeline, acceptance criteria | Adapted to your tech stack, data, and compliance requirements | Provisioned to your Azure tenant with full handoff documentation |
| **Best for** | Well-defined use case, clear acceptance criteria | Existing systems to integrate with, specific constraints | Teams who need the system running in their own environment |

</div>

<br>

> **Source code available under NDA for serious engagements.**
> **Live demo using your sample data available on request.**

<br>

<div align="center">

[View LLM Observatory brief](projects/llm-observatory/brief.md) · [View Enterprise RAG brief](projects/enterprise-rag/brief.md) · [View Document Intelligence brief](projects/document-intelligence/brief.md) · [View ProcureAI brief](projects/procureai/brief.md) · [View Multi-Agent brief](projects/multi-agent-workflow/brief.md)

</div>

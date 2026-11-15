# Example experiments

These are illustrative designs, not results from an inspected repository. Thresholds and file names are examples; derive actual criteria from the task and available evidence.

## Feature ideation: self-service invoices

**Request:** Decide whether to add self-service invoice downloads before building the full feature.

**Hypothesis:** Target users can retrieve the correct invoice without contacting support using the proposed account-page design. The alternative is that invoice terminology or navigation prevents them from finding it.

**Experiment:** Use an authorized prototype session with five representative users, each given a realistic invoice retrieval task. Before observing them, set a provisional usability criterion: at least four complete the task unaided within two minutes. Record completion, time, errors, and prompts required. The prototype avoids building the invoice backend to answer a navigation question.

**Decision rules:** Support this limited usability claim if at least four succeed; reject the proposed usability criterion if fewer succeed in valid sessions. Missing sessions or a broken prototype are inconclusive. A small usability study does not establish a population-wide success rate or a reduction in support contacts.

**Development consequence:** Support can justify the next implementation slice and a separately designed test of actual support-contact reduction. Rejection calls for revising navigation or the proposed solution before investing in the backend. Keep the business-value hypothesis open until it has direct evidence.

## Change: add a search cache

**Request:** Speed up repeated searches using a cache.

**Hypothesis:** On the recorded repeated-query workload, a bounded cache will lower p95 search latency by at least 20% relative to the uncached implementation while respecting the existing freshness contract and memory limit. Database work, rather than network overhead, is the suspected bottleneck.

**Experiment:** Record the uncached baseline. Run a minimal cache implementation against the same workload, data, and environment, with warm-up and five independent runs per variant. Include cold queries and mutations to check freshness. Before running, define the latency comparison, uncertainty assessment, and memory ceiling from the project's performance budget. The 20% threshold is provisional until grounded in that budget.

**Decision rules:** Support if the predeclared comparison establishes the required improvement and correctness and memory guardrails hold. Reject if valid evidence rules out the required improvement or exposes a freshness violation. Treat overlapping uncertainty, unstable runs, or an unrepresentative workload as inconclusive.

**Development consequence:** Support justifies completing and verifying the requested cache implementation. Rejection directs attention to profiling or another design; it does not justify weakening freshness. Inconclusive evidence calls for a better workload or measurement. No outcome establishes that users find search more useful.

## Verify: check tenant isolation in exports

**Request:** Verify that an existing export endpoint respects tenant boundaries.

**Hypothesis:** For authenticated tenant A, exporting a record owned only by tenant B returns the contract's denial response and no record content. The alternative is that tenant scoping applies to listings but is bypassed by direct identifiers.

**Experiment:** Trace authentication and export lookup paths. In isolated fixtures, create two tenants and exercise a permitted own-tenant export, a cross-tenant identifier, and any alternative export path discovered during inspection. Assert response status and returned content at the endpoint boundary, using the existing contract as the oracle. Finish after the selected paths and cases are covered.

**Decision rules:** Support for the inspected paths requires permitted exports to succeed and cross-tenant requests to deny access without disclosing content. One reproducible cross-tenant disclosure rejects the claim. Fixtures that cannot reach the real authorization path leave it inconclusive.

**Development consequence:** Report covered paths and evidence. A rejection warrants a concrete defect report and a proposed correction; apply it only if fixes are authorized. Passing cases do not establish tenant isolation for the entire application.

## Understand: trace a failed webhook

**Request:** Explain whether failed webhook deliveries retry immediately or through a worker.

**Hypothesis:** A retryable delivery failure schedules a delayed queue job; the HTTP handler does not perform the next attempt. A plausible alternative is an inline retry loop before the handler returns.

**Experiment:** Trace the handler, delivery client, queue producer, and worker for a concrete retryable response. Inspect the queue delay configuration and terminal failure path. If execution is available, inject the failure in an isolated harness and observe enqueue arguments, outbound call count, and the worker consuming the scheduled job. Bound the probe to one failure and one retry.

**Decision rules:** Source evidence supports the described wiring if the failure path schedules a delayed job consumed by the worker and contains no inline reattempt. Runtime evidence strengthens the conclusion if the handler makes one delivery attempt and the worker makes the next. An inline second attempt contradicts the hypothesis. An unobserved queue consumer leaves the runtime account incomplete.

**Development consequence:** Produce a file-linked explanation that separates established wiring from observed execution. If the alternative wins, revise the explanation and identify the actual retry mechanism. Propose additional tracing only for unresolved branches that matter to the request; no refactor is implied.

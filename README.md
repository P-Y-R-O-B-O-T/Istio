# Istio

## Monolith vs Micro Services
<!-- * monoliths had all components in single service as modules and everything was coupled -->
<!-- * The monolith handles all the things like: authentication, authorization, networking, logging, monitoring, tracing -->
<!-- * Upgradations of modules were difficult -->
<!-- * Scaling led to scaling up of all the components even if only one component was having a big load, rest were not aupposed to upscale, this is bad because this will increase the cloud resource consumption and price -->
<!-- * Multi language support wasnt possible -->
<!-- * Version upgrading and experimantation was difficult because there cant be 2 versions of same module within the monolith -->
<!-- * Thsi becomes a big ball of mud. -->
<!---->
<!-- * Refactoring the monolith to microservice requires all independent services to be decoupled -->
<!-- * Pro of microservice: scalable, faster and smaller releases, multi lingual, resilient, easy to understand -->
<!-- * but the issue in microservices come that we need to take care of : authentication, authorization, networking, logging, monitoring, tracing, etcetc. we also cant code all these into microservices itself as well because this will disrupt the main reason for designing microservices. -->
<!-- * cons of microservices: complex netowrking, security, observability, overload traditional operation models -->
<!-- * These issues are handled by the devops teams outside the microservices -->

### Monolith
    Pros
        Easy to build, test, and deploy at the start
        Fewer parts to run and manage
        Faster internal calls; easier to debug
        Centralized cross-cutting concerns (auth, logging, monitoring) in one place

    Cons
        Must scale the whole app even if one part is busy (costly)
        Limited multi-language support (usually one stack for all)
        Hard to run two versions of the same module
        Releases slow down as code grows; becomes a “big ball of mud”
        One bug can crash the whole app
        Upgrading modules safely is difficult

### Microservices
    Pros
        Scale only hot services (cost-efficient)
        Strong multi-language support (polyglot per service)
        Choose best-fit databases per service
        Better fault isolation; resilience patterns (circuit breakers, bulkheads)
        Easier experimentation and versioning per service

    Cons
        More complex runtime (network issues, retries, timeouts)
        Harder end-to-end observability (logs/metrics/traces correlation)
        Broader security needs (mTLS, service-to-service auth, secrets)
        Data consistency challenges (eventual consistency, sagas, outbox)
        More infra/pipelines/on-call overhead
        Risk of a “distributed monolith” if not truly decoupled

### Cross-Cutting Concerns (who handles what)
    API Gateway (edge): auth, rate limits, routing, request/response transformation
    Service Mesh/Sidecars: mTLS, retries/timeouts, circuit breaking, telemetry
    Platform tools: centralized logs/metrics/traces, CI/CD, service discovery, config/secrets
    Services: focus on business logic; don’t duplicate platform features

### Migration: Monolith → Microservices
    Prepare
        Identify clear domain boundaries (DDD)
        Strengthen tests and observability in the monolith
        Define APIs and versioning strategy

    Execute
        Strangler pattern: move one capability at a time via a proxy
        Database per service; use CDC/outbox for events
        Set up gateway, discovery, CI/CD, telemetry early
        Keep APIs backward compatible during cutover

    Watch out
        Consistency: sagas, idempotency, compensations
        Latency/chatty calls: aggregation, BFF, async messaging
        Ops load: SRE/DevOps readiness, runbooks, SLOs



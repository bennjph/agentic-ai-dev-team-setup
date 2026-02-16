# AI Evals Course: Visual Decision Flowcharts

This document contains the key decision flowcharts and process diagrams that enhance the AI Evals for Everyone course with practical, visual guidance for building production AI systems.

---

## 1. End-to-End Evaluation Lifecycle

This flowchart shows the complete journey from product concept through production monitoring and continuous improvement.

```mermaid
graph TD
    A[Product Concept] --> B{Need AI Component?}
    B -->|Yes| C[Define Success Criteria]
    B -->|No| Z[Traditional Development]
    
    C --> D[Model Selection & Testing]
    D --> E{Model Capabilities Sufficient?}
    E -->|No| F[Consider Fine-tuning/RAG]
    E -->|Yes| G[Build Reference Dataset]
    
    F --> D
    
    G --> H[Implement Evaluation Metrics]
    H --> I[Code-Based Checks]
    H --> J[LLM Judges]
    H --> K[Human Review Process]
    
    I --> L[Pre-Deployment Validation]
    J --> L
    K --> L
    
    L --> M{Validation Pass Threshold?}
    M -->|No - Critical Failures| N[Iterate on Prompts/Architecture]
    M -->|No - Edge Cases| O[Expand Reference Dataset]
    M -->|Yes| P[Staged Production Rollout]
    
    N --> H
    O --> G
    
    P --> Q[Deploy with Guardrails]
    Q --> R[Monitor Production Traffic]
    
    R --> S{Guardrail Triggered?}
    S -->|Yes - P0/P1| T[Immediate Incident Response]
    S -->|No| U[Offline Batch Analysis]
    
    T --> V[Rollback/Throttle/Escalate]
    V --> W[Root Cause Analysis]
    W --> X{Fix Type?}
    X -->|Prompt/Config| N
    X -->|Data Quality| O
    X -->|Model Limitation| D
    
    U --> Y{New Pattern Detected?}
    Y -->|Yes - Quality Issue| AA[Update Evaluation Metrics]
    Y -->|Yes - User Behavior Shift| AB[Expand Reference Dataset]
    Y -->|No| AC[Generate Weekly Report]
    
    AA --> H
    AB --> G
    AC --> R
    
    style A fill:#e1f5ff
    style P fill:#c3f0c3
    style T fill:#ffcccc
    style Z fill:#f0f0f0
```

**Key Decision Points:**
- **Need AI Component?** - Not every problem requires AI; consider rule-based alternatives
- **Model Capabilities Sufficient?** - Benchmark performance vs. your reference dataset
- **Validation Pass Threshold?** - Define acceptable failure rates before deployment
- **Fix Type?** - Different root causes require different remediation paths

---

## 2. Build vs Buy Decision Tree for Evaluation Infrastructure

This flowchart helps teams decide whether to use open-source tools, managed platforms, or build custom evaluation infrastructure.

```mermaid
graph TD
    A[Evaluation Infrastructure Needed] --> B{Daily Interaction Volume?}
    
    B -->|< 1,000/day| C[Low Volume Path]
    B -->|1,000 - 100,000/day| D[Medium Volume Path]
    B -->|> 100,000/day| E[High Volume Path]
    
    C --> F{Team Size?}
    F -->|< 3 engineers| G[Open Source Tools]
    F -->|3-10 engineers| H{Budget Available?}
    
    H -->|< $500/mo| G
    H -->|$500-2k/mo| I[Lightweight Managed]
    H -->|> $2k/mo| J[Full-Featured Managed]
    
    D --> K{Compliance Requirements?}
    K -->|HIPAA/SOC2/PCI| L[Self-Hosted Solutions]
    K -->|Standard| M{Budget Available?}
    
    M -->|< $2k/mo| N[OSS + Cloud Hosting]
    M -->|$2k-10k/mo| O[Managed Platform]
    M -->|> $10k/mo| P{Engineering Capacity?}
    
    P -->|< 5 eng| O
    P -->|> 5 eng| Q[Consider Custom Build]
    
    E --> R{Unique Requirements?}
    R -->|Yes - Multi-Modal/Specialized| S[Custom Build Required]
    R -->|No - Standard LLM Apps| T[Enterprise Managed Platform]
    
    G --> U[Langfuse Community]
    I --> V[Weave / Helicone]
    J --> W[Langsmith]
    L --> X[Self-Hosted Langfuse]
    N --> Y[Langfuse Cloud]
    O --> Z[Arize / Humanloop]
    Q --> AA[Hybrid: OSS Core + Custom]
    S --> AB[Internal Platform Team]
    T --> AC[Arize Enterprise]
    
    style G fill:#c3f0c3
    style U fill:#c3f0c3
    style V fill:#fff4cc
    style W fill:#fff4cc
    style Z fill:#ffe0cc
    style AB fill:#ffcccc
    style AC fill:#ffe0cc
```

**Decision Criteria Summary:**

| Volume | Team | Budget | Compliance | Recommendation |
|--------|------|--------|------------|----------------|
| Low | Small | Minimal | No | OSS (Langfuse Community) |
| Low | Small | <$2k | No | Managed Lite (Weave) |
| Medium | Medium | <$2k | No | OSS + Cloud |
| Medium | Medium | $2-10k | No | Managed (Arize/Humanloop) |
| Medium | Any | Any | Yes | Self-Hosted OSS |
| High | Large | >$10k | No | Enterprise Managed |
| High | Large | >$10k | Unique needs | Custom Build |

---

## 3. Metric Selection Decision Flow

This flowchart guides the decision of whether to use code-based metrics, LLM judges, or human evaluation for specific behaviors.

```mermaid
graph TD
    A[Behavior to Evaluate] --> B{Is it Objectively Measurable?}
    
    B -->|Yes - Clear Pass/Fail| C{Examples: Structure, Format, Keywords}
    B -->|No - Subjective Quality| D{Examples: Tone, Helpfulness, Appropriateness}
    
    C --> E{Latency Requirement?}
    E -->|Real-time - Online Guardrail| F[Code-Based Metric - Online]
    E -->|Batch Analysis - Offline| G[Code-Based Metric - Offline]
    
    D --> H{Can Expert Define Clear Rubric?}
    H -->|No - Too Nuanced| I[Human Evaluation Only]
    H -->|Yes - Rubric Possible| J{Budget for Human Review?}
    
    J -->|Yes - High Stakes| K{Volume Manageable?}
    J -->|No| L[LLM Judge Candidate]
    
    K -->|< 100 reviews/day| M[Human Evaluation]
    K -->|> 100 reviews/day| N[Human Sample + LLM Judge]
    
    L --> O{Can Calibrate LLM Judge?}
    O -->|Yes - Have Human Labels| P[Calibration Process]
    O -->|No - No Ground Truth| Q{Is Metric Critical?}
    
    Q -->|Yes| R[Create Human Labels First]
    Q -->|No| S[Skip Metric or Redefine]
    
    P --> T{Agreement > 80%?}
    T -->|Yes| U[Deploy LLM Judge - Offline]
    T -->|No| V[Refine Rubric or Use Human]
    
    R --> P
    V --> H
    
    F --> W[Implementation: Simple Checks]
    G --> X[Implementation: Batch Scripts]
    M --> Y[Implementation: Review UI]
    N --> Z[Implementation: Hybrid System]
    U --> AA[Implementation: LLM Judge Pipeline]
    
    style F fill:#c3f0c3
    style G fill:#c3f0c3
    style W fill:#c3f0c3
    style M fill:#fff4cc
    style U fill:#ffe0cc
    style I fill:#ffcccc
    style S fill:#f0f0f0
```

**Cost-Benefit Analysis:**

| Approach | Setup Cost | Runtime Cost | Latency | Best For |
|----------|-----------|--------------|---------|----------|
| Code-Based | Low ($) | Very Low | <10ms | Structure, compliance, keywords |
| LLM Judge | Medium ($$) | Medium-High | 500ms-2s | Tone, relevance, quality (offline) |
| Human | Low ($) | High ($$$) | Minutes-Days | Nuanced judgment, calibration |

---

## 4. Incident Response Workflow

This state diagram shows the complete incident response process when evaluation guardrails trigger or quality issues are detected.

```mermaid
stateDiagram-v2
    [*] --> AlertTriggered
    
    AlertTriggered --> SeverityAssessment
    note right of SeverityAssessment: Use severity rubric
    
    SeverityAssessment --> P0Critical: Safety/Legal/Compliance
    SeverityAssessment --> P1High: Mass User Impact (>100)
    SeverityAssessment --> P2Medium: Quality Degradation
    SeverityAssessment --> P3Low: Edge Case / Cosmetic
    
    P0Critical --> ImmediateActions
    note right of ImmediateActions: <15 min response time
    ImmediateActions --> Rollback: Full service stop
    ImmediateActions --> FeatureDisable: Disable AI component
    ImmediateActions --> TrafficBlock: Block specific inputs
    
    Rollback --> NotifyExecutives
    FeatureDisable --> NotifyExecutives
    TrafficBlock --> NotifyExecutives
    
    NotifyExecutives --> RootCauseAnalysis
    
    P1High --> TrafficThrottle
    note right of TrafficThrottle: <1 hour response
    TrafficThrottle --> SampleUsers: 10% traffic only
    TrafficThrottle --> RateLimiting: Reduce load
    
    SampleUsers --> RootCauseAnalysis
    RateLimiting --> RootCauseAnalysis
    
    RootCauseAnalysis --> DiagnosisComplete
    DiagnosisComplete --> PromptIssue: Config/Prompt error
    DiagnosisComplete --> DataIssue: Training/Reference data
    DiagnosisComplete --> ModelIssue: Model limitation
    DiagnosisComplete --> InfraIssue: System/API failure
    
    PromptIssue --> HotfixDeploy
    DataIssue --> DatasetUpdate
    ModelIssue --> ModelSwitch
    InfraIssue --> SystemRestore
    
    HotfixDeploy --> ValidationTest
    DatasetUpdate --> ValidationTest
    ModelSwitch --> ValidationTest
    SystemRestore --> ValidationTest
    
    ValidationTest --> GradualRollout
    GradualRollout --> FullRestore
    
    P2Medium --> IncreasedMonitoring
    note right of IncreasedMonitoring: <24 hour response
    IncreasedMonitoring --> PriorityTicket
    PriorityTicket --> ScheduledFix
    ScheduledFix --> NextSprintDeploy
    
    P3Low --> BacklogTicket
    BacklogTicket --> TriageInPlanning
    
    FullRestore --> PostMortem
    NextSprintDeploy --> PostMortem
    TriageInPlanning --> [*]
    
    PostMortem --> DocumentLearnings
    DocumentLearnings --> UpdateMetrics
    UpdateMetrics --> UpdateRunbooks
    UpdateRunbooks --> [*]
    
    note left of PostMortem: Required for P0, P1
```

**Severity Definitions:**

| Priority | Definition | Response SLA | Notification | Examples |
|----------|-----------|--------------|--------------|----------|
| **P0** | Safety, legal, or compliance violation | <15 min | Page on-call + exec | Medical misinfo, PII leak, illegal advice |
| **P1** | Severe user impact affecting >100 users | <1 hour | Team lead + PM | Service outage, mass incorrect responses |
| **P2** | Quality degradation affecting <100 users | <24 hours | Async notification | Tone issues in subset, edge case failures |
| **P3** | Cosmetic or rare edge cases | Next sprint | Backlog ticket | Format glitches, minor typos |

---

## 5. RAG System Evaluation Pipeline

This diagram shows the complete evaluation flow for Retrieval-Augmented Generation systems, covering both retrieval and generation quality.

```mermaid
graph TB
    subgraph Input
        A[User Query] 
        B[Ground Truth Answer]
        C[Expected Relevant Docs]
    end
    
    subgraph Retrieval_Stage
        D[Vector Search / BM25]
        E[Retrieved Documents - Top K]
        F[Document Re-ranking]
        G[Final Context Window]
    end
    
    subgraph Retrieval_Evaluation
        H{Eval: Retrieval Quality}
        I[Metric: Precision@K]
        J[Metric: Recall@K]
        K[Metric: MRR - Mean Reciprocal Rank]
        L[Metric: NDCG - Normalized DCG]
        M[Metric: Context Sufficiency]
    end
    
    subgraph Generation_Stage
        N[LLM Generation with Context]
        O[Generated Answer]
    end
    
    subgraph Generation_Evaluation
        P{Eval: Answer Quality}
        Q[Metric: Faithfulness - Answer grounded in context?]
        R[Metric: Answer Relevance - Addresses query?]
        S[Metric: Correctness - Matches ground truth?]
        T[Metric: Completeness - All aspects covered?]
        U[Metric: Conciseness - Appropriate length?]
    end
    
    subgraph End_to_End_Evaluation
        V{Eval: System Performance}
        W[Metric: Latency - Retrieval + Generation]
        X[Metric: Cost - Tokens + Compute]
        Y[Metric: User Satisfaction - Implicit signals]
    end
    
    A --> D
    D --> E
    E --> F
    F --> G
    
    G --> N
    N --> O
    
    E --> H
    C --> H
    H --> I
    H --> J
    H --> K
    H --> L
    H --> M
    
    O --> P
    B --> P
    G --> P
    P --> Q
    P --> R
    P --> S
    P --> T
    P --> U
    
    O --> V
    A --> V
    V --> W
    V --> X
    V --> Y
    
    I --> AA[Aggregate Retrieval Score]
    J --> AA
    K --> AA
    L --> AA
    M --> AA
    
    Q --> AB[Aggregate Generation Score]
    R --> AB
    S --> AB
    T --> AB
    U --> AB
    
    AA --> AC[Combined RAG Quality Score]
    AB --> AC
    W --> AC
    X --> AC
    Y --> AC
    
    style H fill:#fff4cc
    style P fill:#ffe0cc
    style V fill:#e1f5ff
    style AC fill:#c3f0c3
```

**RAG-Specific Metrics Explained:**

**Retrieval Metrics:**
- **Precision@K**: Of the K documents retrieved, how many are relevant?
  - Formula: `(Relevant Retrieved) / K`
  - Use when: You care about quality over quantity
  
- **Recall@K**: Of all relevant documents, how many did we retrieve?
  - Formula: `(Relevant Retrieved) / (Total Relevant)`
  - Use when: Completeness is critical
  
- **MRR (Mean Reciprocal Rank)**: How soon does the first relevant doc appear?
  - Formula: `1 / (Rank of First Relevant Doc)`
  - Use when: First result matters most
  
- **Context Sufficiency**: Does the context contain enough info to answer?
  - Requires: LLM judge or human evaluation
  - Critical for: Preventing hallucination

**Generation Metrics:**
- **Faithfulness**: Is the answer grounded in retrieved context?
  - Detects: Hallucination
  - Method: LLM judge comparing answer to context
  
- **Answer Relevance**: Does it address the user's query?
  - Detects: Off-topic responses
  - Method: Semantic similarity or LLM judge

---

## 6. Multi-Turn Conversation Evaluation Flow

This sequence diagram shows how to evaluate conversational AI systems across multiple interaction turns with both turn-level and session-level metrics.

```mermaid
sequenceDiagram
    participant U as User
    participant S as AI System
    participant E as Eval System
    participant DB as Conversation DB
    
    Note over U,DB: Turn 1 - Initial Query
    U->>S: "I need to return my shoes"
    S->>S: Process & Generate
    S->>U: "I can help! Do you have order number?"
    S->>DB: Log Turn 1
    
    activate E
    E->>DB: Fetch Turn 1
    E->>E: Eval Turn 1: Relevance ✓
    E->>E: Eval Turn 1: Tone ✓
    E->>E: Eval Turn 1: Info Gathering ✓
    E->>DB: Store Turn 1 Scores
    deactivate E
    
    Note over U,DB: Turn 2 - Follow-up
    U->>S: "I lost my receipt"
    S->>S: Process with Turn 1 Context
    S->>U: "No problem! I can look it up by email"
    S->>DB: Log Turn 2
    
    activate E
    E->>DB: Fetch Turn 1 + Turn 2
    E->>E: Eval Turn 2: Context Retention ✓
    E->>E: Eval Turn 2: Consistency with Turn 1 ✓
    E->>E: Eval Turn 2: Solution Continuity ✓
    E->>DB: Store Turn 2 Scores
    deactivate E
    
    Note over U,DB: Turn 3 - Clarification
    U->>S: "email@example.com"
    S->>S: Process with Full Context
    S->>U: "Found order #12345. Processing return..."
    S->>DB: Log Turn 3 + Mark Session Complete
    
    activate E
    E->>DB: Fetch Full Session
    E->>E: Eval Turn 3: Task Completion ✓
    E->>E: Eval Turn 3: Efficiency ✓
    
    Note over E: Session-Level Evaluation
    E->>E: Session: Goal Completion (Success ✓)
    E->>E: Session: Turn Count (3 turns - Optimal)
    E->>E: Session: Escalation Needed? (No)
    E->>E: Session: Conversation Coherence ✓
    E->>E: Session: User Satisfaction Proxy
    
    E->>DB: Store Session-Level Scores
    E->>E: Check Thresholds
    
    alt Session Quality < Threshold
        E->>DB: Flag for Human Review
    else Session Quality Acceptable
        E->>DB: Archive as Training Example
    end
    deactivate E
```

**Multi-Turn Metrics Framework:**

| Metric Level | Metric Name | What It Measures | Evaluation Method |
|--------------|-------------|------------------|-------------------|
| **Turn-Level** | Relevance | Does response address current input? | LLM Judge |
| **Turn-Level** | Context Retention | Does it remember prior turns? | Code check for references |
| **Turn-Level** | Consistency | No contradictions with prior turns? | LLM Judge |
| **Turn-Level** | Tone Maintenance | Consistent personality/brand voice? | LLM Judge |
| **Session-Level** | Goal Completion | Did user achieve their objective? | Business logic + signals |
| **Session-Level** | Efficiency | Minimum turns to resolution? | Turn count |
| **Session-Level** | Escalation Timing | When did it hand off to human? | Code check |
| **Session-Level** | Coherence | Does full conversation make sense? | LLM Judge |
| **Session-Level** | User Satisfaction | Did user seem satisfied? | Implicit signals |

**Implicit Satisfaction Signals:**
- ✅ Conversation ended normally (not abandoned)
- ✅ No repeated rephrasing of same question
- ✅ No explicit frustration indicators
- ✅ User provided requested information
- ❌ Session abandoned mid-conversation
- ❌ User asked to speak to human
- ❌ Multiple "I don't understand" from user

---

## Usage Guide for These Flowcharts

### When to Use Each Flowchart:

1. **End-to-End Lifecycle** - Use during project planning to map your complete evaluation strategy
2. **Build vs Buy** - Use when evaluating evaluation platforms (meta!)
3. **Metric Selection** - Use when deciding how to implement each specific metric
4. **Incident Response** - Print and post for on-call engineers; use during drills
5. **RAG Evaluation** - Use when building or auditing RAG systems
6. **Multi-Turn Evaluation** - Use for conversational AI, chatbots, or agents

### Integration with Course Chapters:

- **Chapter 3** (Evaluation Framework) → Metric Selection Flow
- **Chapter 4** (Reference Datasets) → End-to-End Lifecycle (dataset creation loop)
- **Chapter 5** (Evaluation Metrics) → Metric Selection + RAG/Multi-Turn specific
- **Chapter 6** (Production Challenge) → Incident Response
- **Chapter 7** (Production Monitoring) → End-to-End Lifecycle (production section)
- **New Chapter 5.6** (Team & Infrastructure) → Build vs Buy

---

## Next Steps

These flowcharts should be:
1. **Printed** as posters for team rooms
2. **Embedded** in internal wikis and runbooks
3. **Referenced** during design reviews and post-mortems
4. **Updated** as your team learns and evolves practices

The diagrams are intentionally detailed to serve as both learning tools and operational references.

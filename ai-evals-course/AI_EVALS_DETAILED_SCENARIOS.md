# AI Evals Course: Detailed Practical Scenarios

This document provides comprehensive, real-world scenarios that illustrate the complete evaluation journey from concept to production, including decision points, pitfalls, and lessons learned.

---

## Scenario 1: Healthcare Patient Triage Chatbot

### Context & Business Problem

**Organization**: Regional hospital system with 5 locations, 500,000 annual patient visits  
**Problem**: Emergency department overcrowding; 40% of ER visits are non-urgent  
**Solution Proposed**: AI-powered triage chatbot to guide patients to appropriate care level  
**Stakes**: Life-critical decisions, HIPAA compliance, medical liability

### Team Structure

- **Product Owner**: VP of Patient Experience
- **Domain Experts**: 3 ER nurses, 1 physician advisor
- **Engineering**: 2 ML engineers, 1 backend engineer
- **Legal/Compliance**: Healthcare attorney, compliance officer
- **Initial Budget**: $150k for 6-month pilot

---

### Phase 1: Pre-Deployment (Month 1-3)

#### Decision Point 1: Do We Need Evals? (Week 1)

**Initial Assumption**: "We'll just use GPT-4, it's smart enough"

**Reality Check**:
- Physician advisor tested with "my chest hurts" → AI suggested "take antacid, call if worse"
- In reality: Could be heart attack (requires immediate ER)
- **Decision**: Evaluation is non-negotiable due to life-critical nature

**Framework Applied**: Chapter 1 - "Do You Actually Need Evals?"
- ✅ High stakes (life/death)
- ✅ Regulatory requirements (HIPAA)
- ✅ Non-deterministic outputs
- ✅ Decision: Full evaluation pipeline required

---

#### Decision Point 2: Model Selection (Week 2-3)

**Options Evaluated**:

| Model | MMLU Score | Healthcare Benchmark | Cost/1k calls | Decision |
|-------|-----------|---------------------|---------------|----------|
| GPT-4 | 86% | Not disclosed | $30 | ❌ Too expensive |
| GPT-3.5 Turbo | 70% | Not disclosed | $2 | ⚠️ Cheaper but less capable |
| Med-PaLM 2 (Google) | 85%+ | 86% (MedQA) | Enterprise only | ❌ Not available |
| Claude Opus 4.5 | 88% | Not disclosed | $15 | ✅ Selected |

**Process**:
1. Started with benchmark review (Chapter 2)
2. Built 20-example reference dataset with ER nurses
3. Tested all 4 models on reference set
4. Claude scored best on: "appropriate escalation" (95% vs GPT-4's 85%)

**Key Insight**: Model evaluations (benchmarks) didn't predict performance on our specific dataset. Healthcare benchmark was better predictor but still not perfect.

---

#### Decision Point 3: Building Reference Dataset (Week 4-6)

**Initial Approach** (Failed):
- Engineers created 50 synthetic examples
- Example: "I have a headache, what should I do?"
- ER nurses: "This is useless - real patients say 'my head is killing me since this morning and I threw up twice'"

**Corrected Approach** (Successful):
1. **Historical Data Mining**: Analyzed 1,000 real ER intake notes
2. **Nurse Workshop**: 3x 2-hour sessions to create examples
3. **Final Dataset**: 75 examples across severity levels

**Dataset Structure**:

```
Example 23:
Input: "I'm having chest pain that started 30 minutes ago, it feels like pressure, I'm sweating"
Expected Behavior: 
  - IMMEDIATE escalation to ER
  - Include instruction to call 911
  - Do NOT offer self-care options
  - Flag: CRITICAL SYMPTOM
  
Example 24:
Input: "I have a headache and slight fever, started yesterday"
Expected Behavior:
  - Suggest urgent care or PCP appointment
  - Ask clarifying questions (fever temp? other symptoms?)
  - Provide home care guidance (hydration, rest)
  - Flag: NON-URGENT
```

**Distribution**:
- Critical (ER immediately): 15 examples
- Urgent (24-48 hours): 25 examples
- Non-urgent (PCP/self-care): 25 examples
- Out-of-scope (mental health crisis, police matter): 10 examples

**Framework Applied**: Chapter 4 - Building Reference Datasets
- Started small and specific
- Domain expert-driven (nurses know edge cases)
- Realistic language (actual patient phrasing)

---

#### Decision Point 4: Evaluation Metrics Implementation (Week 7-9)

**Identified Critical Behaviors**:

1. **Triage Accuracy** - Did it assign correct urgency level?
2. **Escalation Safety** - Did it catch all critical symptoms?
3. **Compliance** - Does it include required medical disclaimers?
4. **Empathy/Tone** - Is it appropriately empathetic without being overly familiar?

**Metric Implementation Decisions**:

| Metric | Approach Chosen | Reasoning |
|--------|----------------|-----------|
| **Triage Accuracy** | Human (ER nurses) | Gold standard; too nuanced for code/LLM |
| **Escalation Safety** | Code-based + Human review | Code checks for "call 911" presence; humans verify context |
| **Compliance** | Code-based | Simple keyword check for disclaimer |
| **Empathy/Tone** | LLM Judge (calibrated) | Scale requires automation; subjective quality |

**LLM Judge Calibration Process** (Empathy/Tone):

Week 7: 
- Created rubric with nurses
- Acceptable: "I understand you're concerned. Let me help assess..."
- Not acceptable: "Don't worry, it's probably nothing"

Week 8:
- Ran LLM judge on 75 reference examples
- Compared to nurse evaluations
- Agreement: 68% (too low!)

Week 9:
- Refined rubric with specific examples
- Added "never minimize patient concerns" rule
- Re-ran: Agreement 85% ✓
- Decision: Deploy for offline analysis only

**Code-Based Compliance Check**:
```python
def check_medical_disclaimer(response: str) -> bool:
    required_phrases = [
        "This is not medical advice",
        "Seek professional medical care",
        "If this is an emergency, call 911"
    ]
    # Must contain at least 2 of 3 required phrases
    matches = sum(1 for phrase in required_phrases if phrase.lower() in response.lower())
    return matches >= 2
```

**Framework Applied**: Chapter 5 - Building Evaluation Metrics
- Code-based for objective criteria
- Human for highest-stakes decisions
- LLM judge only after careful calibration

---

#### Decision Point 5: Build vs Buy Evaluation Platform (Week 10)

**Requirements**:
- HIPAA-compliant logging
- Human review interface for nurses
- Audit trail for legal compliance
- Integration with existing EHR system

**Options Evaluated**:

| Option | Cost | HIPAA | Pros | Cons |
|--------|------|-------|------|------|
| Langfuse Cloud | $500/mo | ❌ No BAA | Easy setup | Not compliant |
| Arize | $3k/mo | ✅ BAA available | Full-featured | Expensive |
| Self-hosted Langfuse | $500/mo infra | ✅ (self-managed) | Control + compliance | Engineering overhead |
| Custom Build | $50k+ | ✅ | Perfect fit | 6+ months delay |

**Decision**: Self-hosted Langfuse
- HIPAA compliance via self-hosting
- 2 weeks to deploy vs 6 months custom
- Saved $2.5k/mo vs Arize (pilot budget constraint)

**Framework Applied**: Build vs Buy Decision Tree (from flowcharts)
- Compliance requirement → Self-hosted
- Budget constraint → OSS over enterprise
- Time pressure → Avoid custom build

---

### Phase 2: Pre-Launch Validation (Month 4)

#### Validation Results (Week 13-14)

**Testing on 75 Reference Examples**:

| Metric | Target | Actual | Pass? |
|--------|--------|--------|-------|
| Triage Accuracy (Nurse eval) | >90% | 92% | ✅ |
| Escalation Safety (Critical symptoms) | 100% | 100% | ✅ |
| Compliance (Disclaimer presence) | 100% | 97% | ❌ |
| Empathy/Tone (LLM judge) | >85% | 88% | ✅ |

**Critical Finding**: Compliance failures
- 2 examples missing disclaimer
- Root cause: When response was very short, disclaimer was skipped
- **Fix**: Added system prompt rule + code-based post-processing to inject disclaimer
- **Re-test**: 100% compliance ✓

#### Stakeholder Sign-off (Week 15)

**Executive Concern**: "How do we know it won't make a dangerous mistake?"

**Response Strategy**:
1. Showed 15 critical symptom test cases → 100% accuracy
2. Demonstrated human-in-loop for ambiguous cases
3. Presented rollback plan if issues arise
4. Committed to 100% logging + weekly audit

**Legal Requirement**: Added to every response:
> "⚠️ **IMPORTANT**: This is not a substitute for professional medical judgment. If you believe this is a medical emergency, call 911 immediately. This tool is for informational guidance only."

---

### Phase 3: Staged Production Rollout (Month 5)

#### Week 16-17: 10% Traffic (Pilot Group)

**Setup**:
- Deployed to single hospital location
- 100% of conversations logged
- Daily review by nurse panel
- Parallel human triage still available

**Monitoring**:
- **Volume**: ~50 conversations/day
- **Escalation Rate**: 35% (expected: 30%)
- **Human Override Rate**: 8% (nurse disagrees with AI)

**Issue Discovered** (Day 3):

**Conversation Log**:
```
User: "My 2 year old has a fever of 103"
AI: "For fevers, you can give acetaminophen and monitor. If it persists beyond 48 hours, contact your pediatrician."
Nurse Override: "NO - 103°F in toddler requires immediate evaluation"
```

**Root Cause**: Prompt didn't account for age-based urgency thresholds

**Fix** (Day 4):
- Added age-based triage rules to system prompt
- Updated reference dataset with 10 pediatric examples
- Re-validated on new examples
- Re-deployed within 24 hours

**Framework Applied**: Chapter 6 - Production Challenge
- Real users reveal edge cases reference dataset missed
- Importance of human override mechanism
- Quick iteration based on production learnings

---

#### Week 18-19: 50% Traffic (Expansion)

**New Pattern Detected**:

**Signal**: Conversation length > 10 turns (normal: 3-5 turns)

**Investigation**:
- Sampled 20 long conversations
- Common pattern: Users with complex medical histories
- AI kept asking clarifying questions but struggled to synthesize

**Example**:
```
Turn 1: "I have chest pain"
Turn 2: AI asks about onset, duration
Turn 3: User mentions diabetic, on blood thinners
Turn 4: AI asks about pain type
Turn 5: User mentions previous heart attack
...
Turn 10: AI still gathering info, user frustrated
```

**Decision**: Added new guardrail
- If conversation > 5 turns → auto-escalate to nurse
- Rationale: Complex cases need human judgment
- Implemented in Week 19

**Metric Evolution**:
- Added "Conversation Efficiency" metric
- Target: 90% of conversations resolve in <5 turns
- Automated flagging of lengthy conversations for review

---

#### Week 20: 100% Traffic (Full Deployment)

**Volume**: ~500 conversations/day across all locations

**Production Monitoring Strategy**:

**Online Guardrails** (Real-time):
1. **Safety Filter**: Checks for critical symptoms → immediate ER escalation
   - Implementation: Code-based keyword detection
   - Latency: <50ms
   
2. **Compliance Check**: Ensures disclaimer present
   - Implementation: Post-processing injection
   - Latency: <10ms

3. **Length Limit**: >5 turns → auto-escalate
   - Implementation: Turn counter
   - Latency: <5ms

**Offline Analysis** (Daily batch):
1. **Triage Accuracy**: Random sample of 50 conversations/day reviewed by nurses
2. **Tone Quality**: LLM judge on 100% of conversations
3. **User Satisfaction Proxy**: Completion rate, explicit complaints

**Cost Analysis**:
- LLM calls: $2.50/conversation × 500 = $1,250/day
- Human review: 50 conversations × 5 min × $30/hr = $125/day
- Infrastructure: $500/mo = $16/day
- **Total**: ~$1,400/day = $42k/month

**ROI Calculation**:
- ER visits reduced by 15% (20 fewer visits/day)
- Cost per unnecessary ER visit: ~$500
- Savings: 20 × $500 = $10k/day = $300k/month
- **Net benefit**: $300k - $42k = $258k/month

---

### Phase 4: Continuous Improvement (Month 6+)

#### Incident: False Negative (Week 23)

**Event**: Patient with atypical heart attack symptoms not escalated to ER
- Presented as "bad indigestion" + fatigue
- AI suggested antacid + PCP follow-up
- Patient went to ER 6 hours later → confirmed heart attack
- Outcome: Patient survived, but dangerous delay

**Severity Assessment**: P0 - Critical (safety issue)

**Immediate Response** (Hour 1):
1. Added "indigestion + fatigue" to critical symptom combinations
2. Deployed hotfix within 60 minutes
3. Notified executive team + legal

**Root Cause Analysis** (Day 1-3):
- Reference dataset had no atypical cardiac presentation examples
- Nurse experts focused on classic "chest pain" presentations
- Model had no training signal for subtle combinations

**Corrective Actions** (Week 24):
1. Expanded reference dataset with 25 atypical symptom combinations
2. Consulted cardiologist to identify edge case presentations
3. Implemented "symptom combination" detector (code-based)
4. Added weekly clinical case review with physicians

**Metric Update**:
- New metric: "Atypical Presentation Detection"
- Weekly testing on clinical case studies from literature
- Target: Catch 95% of documented atypical presentations

**Post-Mortem Learnings**:
1. Reference datasets must include rare-but-critical edge cases
2. Domain expert diversity matters (nurses + physicians)
3. Production monitoring caught issue before worse outcome
4. Fast iteration capability is essential for safety-critical AI

**Framework Applied**: Chapter 7 - Incident Response Workflow
- P0 severity → immediate hotfix
- Root cause → dataset expansion
- Metric evolution → new ongoing measurement

---

### Emerging Issues Discovered (Month 7-12)

#### Issue 1: Language Barriers

**Signal**: Higher escalation rate for non-English conversations (45% vs 35%)

**Investigation**: 
- Spanish-language responses were overly cautious
- Translated medical terminology sounded more severe
- Example: "malestar" (discomfort) → "severe distress"

**Solution**:
- Hired bilingual nurse to review Spanish interactions
- Created language-specific rubrics
- Calibrated separate LLM judge for Spanish tone

---

#### Issue 2: Seasonal Patterns

**Signal**: Spike in "fever + cough" queries (flu season)

**Investigation**:
- AI initially triaged all as "urgent care"
- Overwhelming urgent care centers
- Most were mild viral infections

**Solution**:
- Added seasonal context to prompts
- Adjusted urgency thresholds during flu season
- Provided clear guidance on when to wait vs seek care

**Metric Evolution**:
- Added "Seasonal Appropriateness" metric
- Different thresholds for flu season vs normal times

---

#### Issue 3: User Trust Calibration

**Signal**: 40% of users said "I'll just go to ER anyway"

**Investigation**:
- Users didn't trust AI for health decisions (understandable!)
- Too much uncertainty in responses
- Lack of confidence indicators

**Solution**:
- Added confidence levels to responses
- "This is a common concern with clear guidance..."
- vs "This combination of symptoms is complex, I recommend speaking with a nurse..."
- Offered live nurse escalation option prominently

---

### Final Outcomes (12-Month Results)

**Impact Metrics**:
- ER visits for non-urgent cases: ↓ 22%
- Patient satisfaction: 4.2/5 (better than phone triage: 3.8/5)
- Average time to appropriate care: ↓ 35 minutes
- Cost savings: ~$3.2M/year

**Evaluation System Maturity**:
- Reference dataset: 75 → 350 examples
- Metrics tracked: 4 → 12 (added language, seasonal, confidence, etc.)
- Human review: 50/day → 20/day (improved automation)
- Incident response time: 60 min → 15 min (better runbooks)

**Team Learning**:
- Initial: "Model benchmarks predict performance"
- Reality: "Domain-specific testing is everything"
  
- Initial: "75 examples is enough"
- Reality: "Dataset must continuously expand with production learnings"

- Initial: "LLM judges can replace humans"
- Reality: "LLM judges augment humans for scale, but critical decisions need experts"

---

### Key Takeaways from Scenario 1

1. **High-stakes domains require comprehensive evaluation** - No shortcuts in healthcare
2. **Compliance is non-negotiable** - HIPAA shaped every technical decision
3. **Domain experts are essential** - Nurses and physicians caught issues engineers missed
4. **Production always reveals new patterns** - Reference datasets are starting points, not endpoints
5. **Fast iteration capability is critical** - Hotfix deployment in <1 hour saved lives
6. **ROI justifies investment** - $150k investment → $3.2M annual return
7. **Metric evolution is constant** - Started with 4 metrics, ended with 12
8. **User trust is a metric** - Technical quality doesn't matter if users don't trust the system

---

## Scenario 2: E-Commerce Product Recommendation Engine

### Context & Business Problem

**Organization**: Mid-sized online fashion retailer  
**Scale**: 500k monthly active users, 50k products  
**Problem**: Generic recommendations leading to low conversion (2.3%)  
**Solution Proposed**: LLM-powered personalized recommendation system  
**Stakes**: Revenue impact, user experience, computational cost

### Team Structure

- **Product Manager**: Head of Personalization
- **Data Science**: 2 ML engineers, 1 data analyst
- **Engineering**: 3 backend engineers
- **Business Stakeholder**: VP of E-Commerce
- **Initial Budget**: $80k (3-month timeline to show ROI)

---

### Phase 1: Model Selection & Initial Eval (Month 1)

#### Decision Point 1: Baseline Establishment

**Current System**: Rule-based collaborative filtering
- Conversion rate: 2.3%
- Average order value: $85
- User engagement: 12% click-through on recommendations

**New AI System Goal**: Beat baseline by 20%

**Approach**:
- Use LLM to generate natural language product descriptions
- Personalize based on user browsing history
- Optimize for "likelihood to purchase" not just "similarity"

---

#### Decision Point 2: Evaluation Strategy (Week 2)

**Initial Plan**: "We'll A/B test in production"

**Challenge**: VP wants confidence before exposing to real traffic

**Solution**: Build offline evaluation first

**Reference Dataset Creation** (Week 2-3):
- Sampled 1,000 past user sessions where purchase occurred
- For each: User history → Recommended products → Actual purchase
- Structure:

```
Example 142:
User History: [Viewed: Black dress size 8, Red heels, Evening bag]
Ground Truth: Purchased matching clutch ($65)
Diversity Requirement: Show items from different categories
Price Range: $40-$100 (based on browsing pattern)
```

**Metrics Defined**:
1. **Relevance** - Are recommendations related to browsing history?
2. **Diversity** - Not all same category/brand
3. **Conversion Proxy** - Did recommended item match actual purchase? (for historical data)
4. **Business Metrics** - Revenue per recommendation, margin

---

#### Decision Point 3: Build vs Buy (Week 3)

**Options**:

| Approach | Implementation | Eval Cost | Decision |
|----------|---------------|-----------|----------|
| Full Custom | Pinecone + GPT-4 + Custom | Build eval system | ❌ Too complex |
| Platform (Algolia AI) | Use vendor API | Vendor dashboard | ❌ No LLM flexibility |
| Hybrid | OpenAI + Langfuse | OSS eval | ✅ Selected |

**Reasoning**:
- Need LLM flexibility for prompt iteration
- Budget doesn't support enterprise platform
- Langfuse sufficient for offline analysis

---

### Phase 2: Pre-Deployment Validation (Month 2)

#### Offline Evaluation Results (Week 5-6)

**Test**: Run AI system on 1,000 historical sessions

| Metric | Baseline | AI System | Target | Pass? |
|--------|----------|-----------|--------|-------|
| Relevance (Manual review) | N/A | 88% | >85% | ✅ |
| Diversity (Code check) | 45% | 72% | >60% | ✅ |
| Conversion Proxy | 2.3% | 3.1% (estimated) | >2.7% | ✅ |
| Latency | 200ms | 1.2s | <500ms | ❌ |

**Critical Issue**: Latency

**Root Cause**: 
- LLM call for each product (10 recs × 120ms = 1.2s)
- Unacceptable for page load

**Solution** (Week 7):
1. Pre-compute embeddings for all products (nightly batch)
2. Use vector similarity for initial filtering
3. LLM only for final ranking of top 20 candidates
4. Result: Latency reduced to 380ms ✓

---

#### Human Evaluation (Week 7-8)

**Process**:
- Showed internal merchandising team 100 recommendation sets
- Asked: "Would you show this to a customer?"
- Rubric: Relevance, diversity, brand alignment

**Results**:
- 85% approval rate
- Common issue: Occasionally recommending luxury brands to budget shoppers

**Fix**: Added price band awareness to prompt
- User browsing $30-50 items → Don't recommend $200+ items

---

### Phase 3: A/B Test in Production (Month 3)

#### Week 9-10: 5% Traffic Test

**Setup**:
- 5% of users see AI recommendations
- 95% see baseline system
- Tracking: CTR, conversion, revenue per user

**Early Results** (Week 10):

| Metric | Control | AI Variant | Lift |
|--------|---------|------------|------|
| CTR | 12.1% | 15.8% | +30% ✅ |
| Conversion | 2.3% | 2.1% | -8% ❌ |
| Revenue/User | $1.96 | $1.88 | -4% ❌ |

**Unexpected Finding**: Higher engagement, lower conversion

**Investigation**:
- Users clicked more but bought less
- Hypothesis: Recommendations were interesting but not purchase-intent aligned
- Example: User shopping for "work dress" → AI recommended "beach vacation outfit" (diverse but irrelevant to immediate need)

---

#### Pivot: Refining Metrics (Week 11)

**Realization**: "Diversity" optimized for exploration, not conversion

**New Approach**:
1. Segment users by session intent
   - **High Intent** (added to cart, searched specific item): Optimize for conversion
   - **Browsing** (casual scrolling): Optimize for engagement + diversity

2. Different prompts for each segment

**High-Intent Prompt**:
```
User has added [Product X] to cart. Recommend items that:
1. Complete the outfit / complement the purchase
2. Are in similar price range
3. Maximize likelihood of adding to cart
```

**Browsing Prompt**:
```
User is casually exploring. Recommend items that:
1. Align with their style preferences
2. Showcase variety across categories
3. Introduce new arrivals or trending items
```

---

#### Week 12-13: Refined A/B Test

**Results with Segmented Approach**:

| Metric | Control | AI Variant | Lift |
|--------|---------|------------|------|
| CTR | 12.1% | 16.2% | +34% ✅ |
| Conversion | 2.3% | 2.9% | +26% ✅ |
| Revenue/User | $1.96 | $2.47 | +26% ✅ |
| Avg Order Value | $85 | $92 | +8% ✅ |

**Success!** Cleared 20% improvement target

---

### Phase 4: Scaling & Production Monitoring (Month 4+)

#### Rollout (Week 14-16)

**Strategy**: Gradual rollout
- Week 14: 25% traffic
- Week 15: 50% traffic
- Week 16: 100% traffic

**Monitoring Setup**:

**Online Guardrails**:
1. **Latency** - Alert if p95 > 500ms
2. **Error Rate** - Alert if >1% recommendation failures
3. **Revenue Drop** - Alert if daily revenue/user drops >5%

**Offline Analysis** (Daily):
1. **Diversity Check** - Sample 100 recommendation sets
2. **Relevance Check** - LLM judge on 500 recommendations
3. **Business Metrics** - Dashboard with CTR, conversion, revenue

---

#### Emerging Issues (Month 5-8)

**Issue 1: Cold Start Problem (Week 18)**

**Signal**: New users had 15% lower conversion than existing users

**Investigation**:
- New users have no browsing history
- AI defaulted to generic "trending items"
- Boring recommendations

**Solution**:
- Added onboarding quiz: "What's your style?" (3 questions)
- Used quiz responses as initial signal
- Result: New user conversion gap closed to 3%

---

**Issue 2: Filter Bubble (Week 22)**

**Signal**: Long-term users reported seeing "same types of products"

**Investigation**:
- AI over-optimized for past behavior
- User who bought floral dresses → only saw floral patterns
- Recommendation diversity decreased over time

**Solution**:
- Added "exploration bonus" - 20% of recommendations are intentionally diverse
- Tracked "style expansion" metric - are users discovering new categories?
- Result: User satisfaction improved, repeat purchase rate increased

---

**Issue 3: Seasonal Drift (Week 30)**

**Signal**: Conversion dropped 12% in November (holiday season)

**Investigation**:
- AI trained on summer data
- Recommended swimwear during holiday shopping season
- No seasonal awareness

**Solution**:
- Added seasonal context to prompts
- November: "Holiday gift-giving season - prioritize gift-appropriate items"
- Updated embeddings monthly with seasonal weights
- Result: Conversion recovered + exceeded baseline

---

#### Advanced Monitoring (Month 9-12)

**Metric Evolution**:

| Metric | Month 1 | Month 12 | Notes |
|--------|---------|----------|-------|
| Relevance | Manual review | LLM judge (daily) | Automated after calibration |
| Diversity | Code check | Multi-faceted score | Added brand, price, category dimensions |
| Conversion | A/B test | Real-time dashboard | Continuous monitoring |
| User Satisfaction | N/A | Implicit signals | Added engagement, return rate |
| Seasonal Fit | N/A | Monthly review | New metric from drift incident |

**Cost Optimization** (Month 10):
- Initial: $15k/month in LLM calls (500k users × 10 recs × $0.003)
- Optimized: $6k/month
  - Batch processing during off-peak hours
  - Caching frequent recommendations
  - Reduced redundant calls

---

### Final Outcomes (12-Month Results)

**Business Impact**:
- Conversion rate: 2.3% → 3.1% (+35%)
- Revenue per user: $1.96 → $2.58 (+32%)
- Customer lifetime value: +18%
- Annual revenue impact: +$4.2M

**Technical Metrics**:
- Latency p95: 380ms (maintained)
- Uptime: 99.8%
- Cost per recommendation: $0.003 → $0.0012 (60% reduction)

**Evaluation System Evolution**:
- Reference dataset: 1,000 → 5,000 examples (seasonal, diverse)
- Metrics: 4 → 9 (added cold start, filter bubble, seasonal)
- Monitoring: Manual A/B → Real-time dashboard
- Human review: Daily (100 samples) → Weekly (50 samples)

---

### Key Takeaways from Scenario 2

1. **Offline eval != online performance** - A/B testing revealed conversion issues offline metrics missed
2. **Business metrics > technical metrics** - Engagement doesn't always translate to revenue
3. **User segmentation matters** - High-intent vs browsing users need different approaches
4. **Production reveals concept drift** - Seasonal patterns, filter bubbles emerge over time
5. **Cost optimization is ongoing** - Initial $15k/mo → $6k/mo through iteration
6. **Metric evolution is continuous** - Started with 4 metrics, needed 9 to capture reality
7. **A/B testing complements evals** - Evals validate quality; A/B tests validate business impact

---

## Scenario 3: Legal Document Contract Review AI

### Context & Business Problem

**Organization**: Mid-sized law firm (120 attorneys)  
**Specialty**: Corporate contracts, M&A  
**Problem**: Junior associates spend 60% of time on manual contract review  
**Solution Proposed**: AI to pre-screen contracts for risk clauses  
**Stakes**: Legal liability, client trust, attorney reputation

### Team Structure

- **Sponsor**: Managing Partner
- **Domain Experts**: 5 senior attorneys (contracts, M&A, IP)
- **Engineering**: 1 ML engineer (consultant), 2 software engineers
- **Budget**: $120k (6-month pilot)
- **Success Criteria**: Reduce junior attorney review time by 30%

---

### Phase 1: Scoping & Risk Assessment (Month 1)

#### Decision Point 1: What Can AI Actually Do? (Week 1-2)

**Initial Ask**: "Build AI to review contracts like a lawyer"

**Reality Check** (Week 2):
- Attorneys tested GPT-4 on sample contract
- AI output: "This indemnification clause appears standard"
- Attorney: "Standard?! This has a one-way obligation that could cost millions!"

**Reframed Scope**:
- ❌ Not: Replace attorney judgment
- ✅ Instead: Flag potentially risky clauses for attorney review
- ✅ Goal: Surface issues for human decision-making

---

#### Decision Point 2: Risk Tolerance (Week 3)

**Key Question**: What's worse - missing a risk or over-flagging?

**Analysis**:

| Scenario | Impact | Frequency | Cost |
|----------|--------|-----------|------|
| False Negative (Missed risk) | Client loses lawsuit | Rare | $500k - $5M |
| False Positive (Over-flag) | Wasted attorney time | Common | $200/hour |

**Decision**: Optimize for recall over precision
- Better to flag 100 clauses and have 70 be false positives
- Than to miss 1 critical risk clause
- Target: 98% recall, accept 40% precision

**Framework Applied**: Chapter 3 - Context shapes metrics
- Risk tolerance determines metric priorities
- Legal domain: Recall > Precision

---

### Phase 2: Building Reference Dataset (Month 2)

#### Approach (Week 4-6)

**Dataset Source**: Historical contracts with known issues

**Process**:
1. Senior attorneys identified 50 past contracts with problems
2. For each: Annotated specific risky clauses
3. Categorized risks:
   - Indemnification (one-sided obligations)
   - Termination (unfavorable notice periods)
   - IP Assignment (overreach on ownership)
   - Liability Caps (insufficient protection)
   - Governing Law (unfavorable jurisdiction)

**Example Annotation**:
```
Contract: Software Licensing Agreement (2022)
Risky Clause (Page 7, Section 4.3):
"Licensee agrees to indemnify Licensor for any claims arising from 
Licensee's use of the software, including but not limited to third-party 
intellectual property claims."

Risk Category: Indemnification
Severity: HIGH
Explanation: One-way indemnification with no cap or exclusions. 
Licensee assumes all risk including Licensor's defects.
Recommended Action: Negotiate mutual indemnification with carve-outs.
```

**Final Dataset**: 50 contracts, 347 annotated clauses

**Distribution**:
- High-risk clauses: 89
- Medium-risk: 158
- Low-risk (acceptable): 100

---

### Phase 3: Metric Design (Month 2-3)

#### Metrics Defined (Week 7-8)

**Primary Metrics**:

1. **Clause Detection Recall** - Did AI flag all high-risk clauses?
   - Target: >98%
   - Method: Human attorney review

2. **False Positive Rate** - % of flagged clauses that weren't actually risky
   - Target: <60% (accept high false positives)
   - Method: Attorney validation

3. **Risk Explanation Quality** - Does AI explain why clause is risky?
   - Target: Lawyer can understand reasoning
   - Method: LLM judge (calibrated with attorneys)

**Secondary Metrics**:

4. **Time Saved** - Does AI reduce review time?
   - Baseline: Junior associate review time
   - Target: 30% reduction

5. **Attorney Override Rate** - How often do attorneys disagree with AI?
   - High rate → AI not useful
   - Target: <20% on high-risk flags

---

#### Implementation Decisions (Week 8-9)

| Metric | Approach | Reasoning |
|--------|----------|-----------|
| Clause Detection | Code + LLM extraction | LLM finds clauses, code categorizes |
| Risk Severity | LLM Judge (attorney-calibrated) | Requires legal reasoning |
| Explanation Quality | Attorney Review (sample) | Too nuanced for automation |
| Time Saved | Instrumentation | Track actual usage |

---

### Phase 4: LLM Judge Calibration (Month 3)

#### Calibration Process (Week 10-12)

**Goal**: Train LLM judge to assess risk severity like senior attorneys

**Rubric Development** (Week 10):

**High-Risk Rubric**:
```
A clause is HIGH-RISK if it:
- Imposes one-sided obligations without reciprocity
- Has uncapped liability or indemnification
- Contains overly broad IP assignment
- Has unreasonable termination restrictions
- Uses unfavorable governing law/jurisdiction

Examples:
HIGH: "Licensee indemnifies Licensor for all claims, without limit"
NOT HIGH: "Parties mutually indemnify with $1M cap"
```

**Calibration Testing** (Week 11):

Iteration 1:
- LLM evaluated 50 clauses
- Attorney consensus: Agreed on 35/50 (70%)
- Main issue: LLM too lenient on IP clauses

Iteration 2:
- Refined rubric with specific IP examples
- Agreement: 42/50 (84%)
- Remaining disagreements on "medium vs high" boundary

Iteration 3:
- Simplified to binary: "Flag for review" vs "Acceptable"
- Agreement: 47/50 (94%) ✓

**Decision**: Deploy LLM judge for initial screening
- Attorneys review all flagged clauses (final decision still human)

---

### Phase 5: Pilot Deployment (Month 4-5)

#### Week 13-14: Internal Testing (5 Attorneys)

**Process**:
1. AI pre-screens contracts
2. Generates report with flagged clauses
3. Attorney reviews AI report + full contract
4. Attorney provides feedback on AI accuracy

**Results** (30 contracts reviewed):

| Metric | Result | Target | Pass? |
|--------|--------|--------|-------|
| Recall (High-risk) | 97% (missed 2/89 clauses) | >98% | ❌ Close |
| False Positive Rate | 52% | <60% | ✅ |
| Time Saved | 38% | >30% | ✅ |
| Attorney Trust | 78% would use again | N/A | ✅ |

**Missed Clauses** (Critical Finding):
1. "Auto-renewal clause buried in termination section" - AI didn't connect clauses
2. "Implied warranty disclaimer" - Legal jargon AI didn't recognize

**Fixes** (Week 15):
1. Added cross-reference analysis (connect related clauses)
2. Expanded rubric with legal jargon variations
3. Re-tested: Recall improved to 99% ✓

---

#### Week 16-20: Broader Rollout (25 Attorneys)

**Monitoring Setup**:

**Online Guardrails** (None - Not real-time system)
- AI generates reports offline, no immediate decisions

**Offline Analysis** (Weekly):
1. **Attorney Override Tracking** - When do attorneys disagree with AI flags?
2. **Missed Clause Post-Mortem** - Any risks AI didn't catch?
3. **Efficiency Metrics** - Time saved per contract type

**Weekly Review Meeting**:
- Senior attorneys review sample AI outputs
- Discuss edge cases
- Update rubrics based on new contract types

---

### Phase 6: Production Insights (Month 6-12)

#### Emerging Pattern 1: Contract Type Matters (Month 6)

**Signal**: High false positive rate on employment contracts (75%) vs M&A (45%)

**Investigation**:
- AI trained mostly on M&A deals
- Employment contracts have different risk patterns
- "Non-compete" clauses: Risky in M&A, standard in employment

**Solution**:
- Segmented AI by contract type
- Separate rubrics for: M&A, Licensing, Employment, Services
- Result: False positive rate dropped to 48% overall

---

#### Emerging Pattern 2: Attorney Calibration Drift (Month 8)

**Signal**: Two senior attorneys disagreed on 40% of "medium-risk" classifications

**Investigation**:
- Different attorneys have different risk tolerance
- No firm-wide standard for "medium" severity
- AI was caught in middle of disagreement

**Solution**:
- Facilitated attorney alignment session
- Created firm-wide risk rubric
- Documented examples for each category
- Result: Inter-attorney agreement improved to 85%

**Key Insight**: AI evaluation exposed human inconsistency
- Forced firm to standardize risk assessment
- Improved overall quality (beyond just AI)

---

#### Emerging Pattern 3: Adversarial Clauses (Month 10)

**Signal**: New client contract had "triple-nested indemnification clause"

**Example**:
```
"Party A indemnifies Party B, except where Party B's actions caused 
the claim, unless such actions were in reliance on Party A's 
representations, provided that..."
```

**Issue**: AI flagged as risky but couldn't explain why (too complex)

**Investigation**:
- Adversarial contract drafters intentionally obscure risks
- LLM struggled with deeply nested conditionals
- Attorneys needed full context, not just flagging

**Solution**:
- Added "complexity score" metric
- If clause has >3 nested conditions → flag as "Requires detailed review"
- Don't attempt to assess risk, just flag complexity
- Result: Attorneys appreciated transparency about AI limits

---

### Final Outcomes (12-Month Results)

**Business Impact**:
- Junior associate contract review time: ↓ 42%
- Contracts reviewed per week: ↑ 35%
- Billable hours reallocated to higher-value work: +$280k/year
- Client satisfaction: ↑ 12% (faster turnaround)

**Quality Metrics**:
- Recall (high-risk clauses): 99.1%
- Precision: 52% (acceptable per initial goal)
- Attorney override rate: 18%
- Missed critical clauses (12 months): 2 (out of 450 contracts)

**Evaluation System Evolution**:
- Reference dataset: 50 → 200 contracts (added employment, IP, services)
- Metrics: 5 → 8 (added contract type, complexity, attorney agreement)
- LLM judge calibration: Quarterly re-calibration with new cases
- Human review: Weekly samples → Monthly audits

---

### Key Takeaways from Scenario 3

1. **AI augments, doesn't replace, expert judgment** - Tool for lawyers, not lawyer replacement
2. **Risk tolerance shapes metric design** - Legal: Recall >> Precision
3. **Domain expert calibration is critical** - LLM judge required extensive attorney involvement
4. **AI exposed human inconsistency** - Forced firm to standardize risk assessment
5. **Contract type matters** - Different domains need different rubrics (M&A ≠ Employment)
6. **Transparency about limitations builds trust** - "Complex clause" flag better than wrong assessment
7. **Missed risks are acceptable if rare** - 99.1% recall means 2 misses/year → attorneys accept this
8. **Human review is the final guardrail** - AI screens, humans decide

---

## Scenario 4: Global Customer Support Chatbot (Multi-Lingual)

### Context & Business Problem

**Organization**: SaaS company (project management software)  
**Scale**: 50k customers, 25 countries  
**Problem**: Support tickets overwhelming team (5k/week), slow response time  
**Solution Proposed**: AI chatbot for tier-1 support in 8 languages  
**Stakes**: Customer retention, brand reputation, support costs

### Team Structure

- **Product Manager**: Head of Customer Success
- **Engineering**: 2 ML engineers, 3 full-stack engineers
- **Domain Experts**: 4 senior support agents, 1 per major language group
- **Budget**: $200k (6-month timeline)
- **Success Criteria**: Deflect 40% of tier-1 tickets, maintain satisfaction >4/5

---

### Phase 1: Baseline & Scoping (Month 1)

#### Decision Point 1: Language Prioritization (Week 1-2)

**Customer Distribution**:
- English: 45%
- Spanish: 18%
- German: 12%
- French: 10%
- Japanese: 8%
- Portuguese: 4%
- Korean: 2%
- Dutch: 1%

**Decision**: Launch with top 4 languages (85% coverage)
- Defer Japanese, Portuguese, Korean, Dutch to Phase 2
- Reduces complexity for initial evaluation

---

#### Decision Point 2: Ticket Type Analysis (Week 2-3)

**Analyzed 10k historical tickets**:

| Category | % of Tickets | Complexity | AI Suitability |
|----------|-------------|------------|----------------|
| Password reset | 22% | Low | ✅ Perfect for AI |
| Billing inquiry | 18% | Medium | ⚠️ Needs policy knowledge |
| Feature question | 25% | Medium | ✅ Good for AI + docs |
| Bug report | 20% | High | ❌ Needs engineering |
| Integration help | 15% | High | ⚠️ Complex, case-by-case |

**Scoped Focus**: Target 65% of tickets (password, billing, feature questions)
- Realistic deflection: 40% overall (60% success rate on in-scope categories)

---

### Phase 2: Reference Dataset Creation (Month 1-2)

#### Multi-Lingual Dataset Strategy (Week 3-5)

**Approach**:
1. Start with English reference dataset (100 examples)
2. Translate to Spanish, German, French
3. Native speakers review translations + add cultural nuances

**Example Issue During Translation**:

**English**:
```
User: "How do I add teammates?"
Expected: Provide step-by-step instructions from help docs
```

**Spanish (Direct Translation)**:
```
User: "¿Cómo agrego compañeros de equipo?"
AI Response: [Instructions in Spanish]
```

**Issue Identified by Spanish Support Agent**:
- In Spain: "compañeros" is informal
- In Mexico: Customers expect more formal "colegas" or "miembros del equipo"
- Tone was too casual for professional SaaS context

**Solution**: Created regional variants
- Spain Spanish vs Latin American Spanish
- Different tone guidelines per region

---

#### Dataset Structure (Week 6)

**Final Reference Dataset**: 400 examples

| Language | Examples | Source | Cultural Review |
|----------|----------|--------|-----------------|
| English | 100 | Historical tickets | Native agents |
| Spanish (ES) | 50 | Translated + original | Spain support agent |
| Spanish (LATAM) | 50 | Variant of ES | Mexico support agent |
| German | 50 | Translated + original | Germany support agent |
| French | 50 | Translated + original | France support agent |
| Cross-language edge cases | 50 | Synthetic | All agents |

---

### Phase 3: Metric Design for Multi-Lingual Context (Month 2)

#### Metrics Defined (Week 7-8)

**Universal Metrics** (Apply to all languages):
1. **Ticket Deflection Rate** - % of users who don't escalate to human
2. **Resolution Accuracy** - Did AI solve the user's problem?
3. **Escalation Appropriateness** - When AI hands off, was it necessary?

**Language-Specific Metrics**:
4. **Tone Appropriateness** - Matches cultural norms for politeness, formality
5. **Translation Quality** - Fluent, natural phrasing (not robot-speak)
6. **Regional Sensitivity** - Avoids region-specific errors (currency, date format, etc.)

---

#### Implementation Decisions

| Metric | Approach | Per-Language? | Reasoning |
|--------|----------|---------------|-----------|
| Deflection Rate | Code (conversion tracking) | Yes | Different baselines per language |
| Resolution Accuracy | User rating + LLM judge | Yes | LLM judge per language |
| Escalation Appropriateness | Code (escalation trigger) | No | Universal logic |
| Tone Appropriateness | LLM judge | Yes | Cultural nuances require language-specific |
| Translation Quality | Human review (sample) | Yes | Native speakers only |

---

#### LLM Judge Calibration (Week 9-10)

**Challenge**: Need separate LLM judge for each language

**Approach**:
1. **English Calibration** (Week 9):
   - Created rubric with support agents
   - Tested on 100 examples
   - Agreement: 88% ✓

2. **Translation of Rubric** (Week 9):
   - Translated rubric to Spanish, German, French
   - Native speakers reviewed for cultural fit

3. **Per-Language Calibration** (Week 10):
   - Tested each language LLM judge on 50 examples
   - Results:
     - Spanish: 82% agreement
     - German: 85%
     - French: 79%
   - Refined rubrics based on disagreements
   - Final agreement: >85% for all ✓

**Key Finding**: Tone rubrics needed significant per-language customization
- English: "Friendly but professional"
- German: "Direct and efficient" (friendliness can seem insincere)
- French: "Polite and respectful" (formality expected)
- Spanish: "Warm and personal" (especially LATAM)

---

### Phase 4: Pilot Launch (Month 3-4)

#### Week 11-13: English-Only Pilot (20% Traffic)

**Results**:

| Metric | Target | Actual | Pass? |
|--------|--------|--------|-------|
| Deflection Rate | 40% | 38% | ✅ Close enough |
| Resolution Accuracy | >80% | 84% | ✅ |
| User Satisfaction | >4.0 | 4.2 | ✅ |
| Escalation Appropriateness | >90% | 88% | ⚠️ |

**Issue**: 12% of escalations were unnecessary
- Example: User asked "how do I export data?" → AI escalated
- Reason: Answer was in docs, AI didn't find it (retrieval issue)

**Fix**: Improved RAG retrieval with better chunking
- Re-test: Escalation appropriateness → 93% ✓

---

#### Week 14-17: Multi-Lingual Rollout (10% Traffic Per Language)

**Monitoring Strategy**:

**Per-Language Dashboards**:
- Deflection rate (compared to English baseline)
- Satisfaction scores
- Escalation patterns
- Tone violations (LLM judge flags)

**Results** (Week 17):

| Language | Deflection | Satisfaction | Tone Issues |
|----------|-----------|--------------|-------------|
| English | 38% | 4.2/5 | 2% flagged |
| Spanish (ES) | 35% | 4.0/5 | 5% flagged |
| Spanish (LATAM) | 40% | 4.3/5 | 3% flagged |
| German | 42% | 4.1/5 | 8% flagged ⚠️ |
| French | 36% | 3.9/5 | 6% flagged |

---

#### Issue: German Tone Problems (Week 17)

**Signal**: 8% of German conversations flagged for tone issues

**Investigation**:
- Sampled 20 flagged conversations
- Common pattern: AI being overly apologetic
- Example:

**User** (German): "Die Export-Funktion funktioniert nicht." (Export function doesn't work)

**AI**: "Es tut mir sehr leid, dass Sie Probleme haben! Ich entschuldige mich..." (I'm so sorry you're having problems! I apologize...)

**German Support Agent Feedback**: "This is too much. German customers want efficiency, not apologies. Just fix it."

**Corrected Version**: "Ich helfe Ihnen. Hier sind die Schritte..." (I'll help you. Here are the steps...)

**Fix** (Week 18):
- Updated German tone rubric
- Re-calibrated LLM judge
- Reduced unnecessary apologies
- Result: Tone issues → 3%, Satisfaction → 4.3/5

---

### Phase 5: Production Monitoring (Month 5-12)

#### Advanced Issue 1: Regional Dialect Detection (Month 6)

**Signal**: Spanish (LATAM) satisfaction in Argentina: 3.5/5 (vs 4.3 overall)

**Investigation**:
- Argentinian Spanish uses "vos" instead of "tú" (different verb conjugations)
- AI used "tú" (standard Latin American)
- Felt impersonal to Argentinian users

**Solution**:
- Added region detection (IP-based + user profile)
- Created Argentina-specific prompt variant
- Result: Argentina satisfaction → 4.2/5

---

#### Advanced Issue 2: Code-Switching (Month 8)

**Signal**: Escalation rate in US Spanish-speakers: 48% (vs 40% overall)

**Investigation**:
- US Spanish-speakers often code-switch (mix English/Spanish)
- Example: "¿Cómo hago el export de mi data?" (How do I export my data?)
- AI responded in Spanish only, ignoring English words

**User Feedback**: "It felt like it didn't understand me"

**Solution**:
- Detected code-switching in input
- Mirrored user's language mixing in response
- Example: "Para exportar tu data, ve a Settings..." (To export your data, go to Settings...)
- Result: Escalation rate → 42%

---

#### Advanced Issue 3: Cultural Sensitivity (Month 10)

**Incident**:
- French customer asked about pricing during holiday season
- AI response: "Great question! Our holiday sale..."
- Escalated to human, customer complained about tone

**Issue**: France doesn't celebrate same holidays as US
- "Holiday sale" assumed US Thanksgiving/Christmas timing
- Customer was asking during French summer holidays (August)

**Fix**:
- Added regional holiday calendars
- Culturally-aware seasonal messaging
- August in France: "During the summer period..."

---

### Final Outcomes (12-Month Results)

**Business Impact**:
- Ticket deflection: 43% (exceeded 40% target)
- Support costs: ↓ $320k/year (fewer human hours)
- First response time: ↓ 65% (AI responds instantly)
- Customer satisfaction: 4.2/5 average (same as human baseline)

**Per-Language Performance**:

| Language | Deflection | Satisfaction | Notes |
|----------|-----------|--------------|-------|
| English | 45% | 4.3/5 | Baseline, best performance |
| Spanish (ES) | 40% | 4.1/5 | Formal tone works well |
| Spanish (LATAM) | 44% | 4.3/5 | Warm tone preferred |
| German | 48% | 4.2/5 | Direct style improved |
| French | 38% | 4.0/5 | Most challenging culturally |

**Evaluation System Evolution**:
- Reference dataset: 400 → 800 examples (added regional variants)
- Metrics: 6 → 12 (added code-switching, regional sensitivity, holiday awareness)
- LLM judges: 4 (per language) → 7 (added regional variants)
- Human review: Daily samples per language → Weekly aggregated

**Cost Analysis**:
- Human support: $45/hour × 5k tickets/week = $11.25M/year
- AI deflection (43%): 2,150 tickets deflected
- Savings: $4.8M/year
- AI cost: $120k/year (infrastructure + LLM calls)
- Net savings: $4.68M/year

---

### Key Takeaways from Scenario 4

1. **Cultural nuance > Direct translation** - Tone rubrics must be per-culture, not just per-language
2. **Regional variants matter** - Spanish (Spain) ≠ Spanish (Mexico) ≠ Spanish (Argentina)
3. **LLM judges need per-language calibration** - Can't just translate the English rubric
4. **Code-switching is real** - Multilingual users mix languages naturally
5. **Cultural awareness extends beyond language** - Holidays, currencies, date formats
6. **User satisfaction is equal across languages when done right** - 4.2/5 average all languages
7. **Per-language monitoring essential** - Aggregate metrics hide language-specific issues
8. **Native speaker involvement critical** - Engineers can't evaluate cultural appropriateness

---

## Cross-Scenario Synthesis

### Common Patterns Across All 4 Scenarios

1. **Initial scope is always wrong** - Production reveals edge cases
2. **Domain experts are irreplaceable** - Engineers alone miss critical nuances
3. **Metric evolution is constant** - Start with 4-6, end with 10-15
4. **Human review remains essential** - Even at scale, sampling is critical
5. **Cost optimization happens post-launch** - Initial implementation is expensive, iterate down
6. **Incident response speed matters** - <1 hour hotfix capability is competitive advantage
7. **User signals reveal hidden issues** - Explicit feedback + implicit behavior patterns
8. **Reference datasets are living documents** - Continuous expansion based on production learnings

### Divergent Lessons

| Aspect | Healthcare | E-Commerce | Legal | Customer Support |
|--------|-----------|------------|-------|------------------|
| **Risk Tolerance** | Zero tolerance (lives) | Balanced (revenue vs UX) | High recall (liability) | Moderate (satisfaction) |
| **Human in Loop** | Always (safety) | Rare (automation) | Always (liability) | Sometimes (escalation) |
| **Metric Priority** | Safety > All | Revenue > Engagement | Recall > Precision | Deflection + Satisfaction |
| **Iteration Speed** | Slow (careful) | Fast (A/B test) | Medium (legal review) | Medium (multi-lingual) |

---

## Using These Scenarios

### For Teams Starting AI Evaluation:

1. **Identify Your Scenario Type**: Which scenario most resembles your use case?
2. **Adopt Similar Metrics**: Start with metrics that worked in comparable domains
3. **Learn from Pitfalls**: Avoid issues others encountered (tone, translation, drift)
4. **Adapt Decision Points**: Use decision frameworks but customize to your context

### For Teams Improving Existing Systems:

1. **Compare Maturity**: Where are you vs 12-month outcomes?
2. **Check for Missing Metrics**: Did you miss seasonal, cultural, or complexity metrics?
3. **Audit Incident Response**: Do you have <1 hour hotfix capability?
4. **Evaluate Cost Efficiency**: Are you optimizing LLM calls? (Healthcare: stayed same; E-commerce: 60% reduction)

---

These scenarios provide realistic templates for building evaluation systems across different domains, stakes, and constraints. Use them as mental models when designing your own evaluation journey.

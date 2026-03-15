#  The Visual Guide to GenLayer Intelligent Contracts

**Understanding Genlayer Through Diagrams, Flowcharts, and Architecture**

*A comprehensive visual journey into how GenLayer revolutionizes smart contracts with AI consensus*

---

##  Table of Contents

1. [Introduction: Why Visualize GenLayer?](#introduction)
2. [The Big Picture: GenLayer Architecture](#architecture)
3. [Flowchart 1: Basic Contract Execution Flow](#flowchart-1)
4. [Flowchart 2: AI Consensus Mechanism](#flowchart-2)
5. [Flowchart 3: Web Data Integration Flow](#flowchart-3)
6. [Flowchart 4: Complete Intelligent Contract Lifecycle](#flowchart-4)
7. [Architecture Diagrams](#diagrams)
8. [Mental Models & Analogies](#mental-models)
9. [Common Patterns Visualized](#patterns)
10. [Troubleshooting Guide](#troubleshooting)

---

<a name="introduction"></a>
##  Introduction: Why Visualize GenLayer?

GenLayer is fundamentally different from traditional blockchains. While Ethereum runs deterministic code, GenLayer runs **intelligent contracts** that use AI and web data. This paradigm shift can be confusing.

**This guide uses visual aids to answer:**
- How does AI consensus actually work?
- Where does the LLM run?
- How do multiple validators agree on AI outputs?
- What's the execution flow of an intelligent contract?

Let's dive in with pictures, not just words.

---

<a name="architecture"></a>
##  The Big Picture: GenLayer Architecture

### Traditional Blockchain vs GenLayer

```
┌─────────────────────────────────────────────────────────────────┐
│                    TRADITIONAL BLOCKCHAIN                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  User → Transaction → Validators (all run same code)            │
│                           ↓                                      │
│                    All get EXACT same result                    │
│                           ↓                                      │
│                      Consensus ✓                                │
│                                                                  │
│  Example: 2 + 2 = 4 (deterministic)                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         GENLAYER                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  User → Transaction → Validators (run AI + Web)                 │
│                           ↓                                      │
│            Each validator gets SIMILAR result                   │
│            (AI outputs vary slightly)                           │
│                           ↓                                      │
│            Equivalence Principle checks if                      │
│            results are "close enough"                           │
│                           ↓                                      │
│                      Consensus ✓                                │
│                                                                  │
│  Example: "Is this news positive?" → 85% yes (non-deterministic)│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Key Insight:** GenLayer doesn't require EXACT matches. It requires EQUIVALENT outputs through AI consensus.

---

<a name="flowchart-1"></a>
##  Flowchart 1: Basic Contract Execution Flow

### The Journey of a Simple Transaction

```
                    ┌──────────────────┐
                    │   User Submits   │
                    │   Transaction    │
                    │  (via Studio)    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Transaction     │
                    │  enters GenLayer │
                    │  Network         │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Leader Validator│
                    │  selected        │
                    └────────┬─────────┘
                             │
                ┌────────────┴────────────┐
                │                         │
                ▼                         ▼
    ┌──────────────────┐      ┌──────────────────┐
    │  Does contract   │      │   Does contract  │
    │  need AI/Web?    │      │   need AI/Web?   │
    └────┬─────────┬───┘      └────┬────────┬────┘
         │         │                │        │
        NO        YES              NO       YES
         │         │                │        │
         ▼         ▼                ▼        ▼
    ┌────────┐ ┌─────────┐    ┌────────┐ ┌────────┐
    │Execute │ │Execute  │    │Execute │ │Execute │
    │Normally│ │with AI  │    │Normally│ │with AI │
    │(Fast)  │ │Consensus│    │(Fast)  │ │Consensus│
    └───┬────┘ └────┬────┘    └───┬────┘ └───┬────┘
        │           │              │          │
        └───────────┴──────────────┴──────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │   Results compared   │
         │   across validators  │
         └──────────┬───────────┘
                    │
         ┌──────────┴───────────┐
         │                      │
         ▼                      ▼
    ┌─────────┐          ┌──────────┐
    │ Match?  │          │Different?│
    │   ✓     │          │    ✗     │
    └────┬────┘          └────┬─────┘
         │                    │
         ▼                    ▼
    ┌─────────┐          ┌──────────┐
    │Consensus│          │Re-execute│
    │Reached  │          │  Vote    │
    └────┬────┘          └────┬─────┘
         │                    │
         └────────────────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │  Transaction         │
         │  Finalized           │
         │  State Updated       │
         └──────────────────────┘
```

### Key Stages Explained:

**Stage 1: Submission**
- User calls contract method
- Transaction broadcast to network

**Stage 2: Leader Selection**
- One validator chosen as "leader"
- Leader proposes execution result
- Others validate the proposal

**Stage 3: Execution**
- **Simple operations:** Direct execution (like traditional blockchain)
- **AI/Web operations:** Special consensus flow (see Flowchart 2)

**Stage 4: Consensus**
- Validators compare results
- If match → Accept
- If differ → Vote and re-execute

**Stage 5: Finalization**
- Winning result committed
- Contract state updated
- User receives confirmation

---

<a name="flowchart-2"></a>
##  Flowchart 2: AI Consensus Mechanism (The Magic!)

### How Multiple AIs Agree on Non-Deterministic Outputs

```
┌────────────────────────────────────────────────────────────────┐
│              CONTRACT CALLS AI (gl.nondet.exec_prompt)         │
└────────────────────────┬───────────────────────────────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  Leader Validator     │
              │  executes LLM call    │
              │                       │
              │  Prompt: "Is this    │
              │  article positive?"   │
              └──────────┬────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  Leader's LLM        │
              │  Response:           │
              │  {"sentiment":       │
              │   "positive",        │
              │   "score": 8.5}      │
              └──────────┬────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  Leader broadcasts   │
              │  result to other     │
              │  validators          │
              └──────────┬────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
         ▼               ▼               ▼
   ┌──────────┐   ┌──────────┐   ┌──────────┐
   │Validator │   │Validator │   │Validator │
   │    2     │   │    3     │   │    4     │
   └────┬─────┘   └────┬─────┘   └────┬─────┘
        │              │              │
        │     Each runs same prompt  │
        │              │              │
        ▼              ▼              ▼
   ┌──────────┐   ┌──────────┐   ┌──────────┐
   │ LLM: 8.3 │   │ LLM: 8.7 │   │ LLM: 8.4 │
   │(slightly │   │(slightly │   │(slightly │
   │different)│   │different)│   │different)│
   └────┬─────┘   └────┬─────┘   └────┬─────┘
        │              │              │
        └──────────────┼──────────────┘
                       │
                       ▼
            ┌─────────────────────┐
            │  EQUIVALENCE        │
            │  PRINCIPLE          │
            │                     │
            │  Checks if results  │
            │  are "close enough" │
            └──────────┬──────────┘
                       │
         ┌─────────────┴─────────────┐
         │                           │
         ▼                           ▼
┌─────────────────┐        ┌─────────────────┐
│  Are results    │        │  Are results    │
│  equivalent?    │        │  equivalent?    │
│                 │        │                 │
│  8.5 ≈ 8.3 ≈    │        │  8.5 vs 2.1     │
│  8.7 ≈ 8.4      │        │  (too different)│
│                 │        │                 │
│  YES! ✓         │        │  NO! ✗          │
└────────┬────────┘        └────────┬────────┘
         │                          │
         ▼                          ▼
┌─────────────────┐        ┌─────────────────┐
│  CONSENSUS      │        │  DISPUTE!       │
│  REACHED        │        │                 │
│                 │        │  Validators     │
│  Accept result  │        │  vote on which  │
│  8.5 (leader's) │        │  result to use  │
│                 │        │                 │
│  State updated  │        │  Majority wins  │
└─────────────────┘        └─────────────────┘
```

### The Equivalence Principle: Core Concept

**Traditional Blockchain:**
```
Validator 1: 2 + 2 = 4
Validator 2: 2 + 2 = 4
Validator 3: 2 + 2 = 4
Result: EXACT MATCH ✓
```

**GenLayer with AI:**
```
Validator 1: Sentiment score = 8.5
Validator 2: Sentiment score = 8.3
Validator 3: Sentiment score = 8.7
Result: CLOSE ENOUGH ✓ (within tolerance)
```

### How "Close Enough" is Determined

```python
# In your contract:
gl.eq_principle.strict_eq(function)  
# → Requires near-identical outputs

gl.eq_principle.prompt_comparative(function, "Must be within 20%")
# → Allows more variation with rules
```

**Visual Tolerance:**
```
         Tolerance Zone
              |
    ┌─────────┴─────────┐
    │                   │
8.0 ├───────────────────┤ 9.0
    │    All results    │
    │    8.3, 8.5, 8.7  │
    │    fall in range  │
    │    ✓ CONSENSUS    │
    └───────────────────┘

         vs

    ┌─────────┬─────────┐
    │         │         │
2.0 ├─────────┤ 8.5     │ 9.0
    │ 2.1     │ 8.5, 8.7│
    │ OUTLIER │ CLUSTER │
    │    ✗    │    ✓    │
    └─────────┴─────────┘
    DISPUTE! Majority wins
```

---

<a name="flowchart-3"></a>
##  Flowchart 3: Web Data Integration Flow

### How Intelligent Contracts Access Real-World Data

```
┌───────────────────────────────────────────────────────────────┐
│         USER CALLS CONTRACT WITH URL PARAMETER                 │
│         Example: verify_news("https://coindesk.com/btc")      │
└────────────────────────┬──────────────────────────────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  Contract Method     │
              │  def verify_news():  │
              │                      │
              │    def fetch():      │
              │      web_data = ...  │
              └──────────┬───────────┘
                         │
                         ▼
              ┌──────────────────────────┐
              │  Leader Validator        │
              │  calls Web API           │
              │                          │
              │  gl.nondet.web.render(  │
              │    url,                  │
              │    mode='text'           │
              │  )                       │
              └──────────┬───────────────┘
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
┌──────────────────┐          ┌──────────────────┐
│  Is URL allowed? │          │  Is URL blocked? │
└────┬─────────┬───┘          └────┬────────┬────┘
     │         │                   │        │
    YES       NO                  NO       YES
     │         │                   │        │
     ▼         │                   ▼        │
┌─────────┐    │              ┌─────────┐  │
│ Fetch   │    │              │ Fetch   │  │
│ Website │    │              │ Fails!  │  │
│ Content │    │              └────┬────┘  │
└────┬────┘    │                   │       │
     │         └───────────────────┴───────┘
     │                     │
     │                     ▼
     │          ┌──────────────────┐
     │          │  ERROR: Blocked  │
     │          │  or unreachable  │
     │          └──────────────────┘
     │
     ▼
┌──────────────────────────┐
│  Website HTML/Text       │
│  Retrieved               │
│                          │
│  "Bitcoin hits $50k...   │
│   The cryptocurrency     │
│   surged today..."       │
└──────────┬───────────────┘
           │
           ▼
┌──────────────────────────┐
│  Leader sends web data   │
│  to other validators     │
└──────────┬───────────────┘
           │
    ┌──────┴──────┬──────────┬─────────┐
    │             │          │         │
    ▼             ▼          ▼         ▼
┌────────┐   ┌────────┐  ┌────────┐ ┌────────┐
│ Val 2  │   │ Val 3  │  │ Val 4  │ │ Val 5  │
│ Fetches│   │ Fetches│  │ Fetches│ │ Fetches│
│ Same   │   │ Same   │  │ Same   │ │ Same   │
│ URL    │   │ URL    │  │ URL    │ │ URL    │
└───┬────┘   └───┬────┘  └───┬────┘ └───┬────┘
    │            │           │          │
    └────────────┴───────────┴──────────┘
                 │
                 ▼
      ┌──────────────────────┐
      │  Compare fetched     │
      │  content             │
      └──────────┬───────────┘
                 │
         ┌───────┴───────┐
         │               │
         ▼               ▼
┌─────────────────┐  ┌─────────────────┐
│ Content matches │  │ Content differs │
│ (same timestamp)│  │ (site updated)  │
│                 │  │                 │
│   ✓ GOOD        │  │   ⚠️ ISSUE      │
└────────┬────────┘  └────────┬────────┘
         │                    │
         └────────────────────┘
                  │
                  ▼
       ┌────────────────────────┐
       │  Now process with AI   │
       │                        │
       │  "Is this news about   │
       │   Bitcoin positive?"   │
       └────────┬───────────────┘
                │
                ▼
       ┌────────────────────────┐
       │  AI Consensus          │
       │  (See Flowchart 2)     │
       └────────┬───────────────┘
                │
                ▼
       ┌────────────────────────┐
       │  Result returned to    │
       │  contract              │
       │                        │
       │  verified = True       │
       │  sentiment = "positive"│
       └────────────────────────┘
```

### Allowed vs Blocked URLs

```
✅ ALLOWED URLS
┌────────────────────────────┐
│ • News Sites               │
│   - CoinDesk.com          │
│   - CNN.com               │
│   - BBC.com               │
│                           │
│ • APIs                     │
│   - CoinGecko             │
│   - OpenWeatherMap        │
│   - GitHub                │
│                           │
│ • Public Data              │
│   - Wikipedia             │
│   - Government sites      │
│   - Documentation         │
└────────────────────────────┘

❌ BLOCKED URLS
┌────────────────────────────┐
│ • Social Media             │
│   - X.com / Twitter       │
│   - Facebook.com          │
│   - Instagram.com         │
│   - LinkedIn.com          │
│                           │
│ • Why blocked?             │
│   - Bot detection         │
│   - Rate limiting         │
│   - Authentication needed │
│   - Dynamic content       │
└────────────────────────────┘
```

### Web Fetch Timing

```
Time →
│
├─ 0s    Contract called
│
├─ 1s    Leader fetches URL
│
├─ 2s    Leader gets response
│
├─ 3s    Other validators fetch
│
├─ 5s    All responses compared
│
├─ 6s    Web data confirmed
│
├─ 7s    AI processing starts
│         (See Flowchart 2)
│
├─ 30s   AI consensus reached
│
├─ 31s   Transaction complete ✓
```

---

<a name="flowchart-4"></a>
##  Flowchart 4: Complete Intelligent Contract Lifecycle

### From Deployment to Execution (The Full Journey)

```
════════════════════════════════════════════════════════════════
                    PHASE 1: DEVELOPMENT
════════════════════════════════════════════════════════════════

         ┌─────────────────────┐
         │  Developer writes   │
         │  Python contract    │
         │                     │
         │  class MyContract:  │
         │    def __init__():  │
         │    @gl.public.write │
         │    def process():   │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Add GenLayer       │
         │  specific features  │
         │                     │
         │  • LLM calls        │
         │  • Web access       │
         │  • Equivalence      │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Test locally       │
         │  (optional)         │
         └──────────┬──────────┘
                    │
════════════════════┼════════════════════════════════════════════
                    │   PHASE 2: DEPLOYMENT
════════════════════┼════════════════════════════════════════════
                    │
                    ▼
         ┌─────────────────────┐
         │  Open GenLayer      │
         │  Studio             │
         │  studio.genlayer.com│
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Paste contract     │
         │  code               │
         └──────────┬──────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │  Click "Deploy"     │
         └──────────┬──────────┘
                    │
         ┌──────────┴───────────┐
         │                      │
         ▼                      ▼
    ┌─────────┐          ┌──────────┐
    │ Schema  │          │ Schema   │
    │ Valid?  │          │ Invalid? │
    │   ✓     │          │    ✗     │
    └────┬────┘          └────┬─────┘
         │                    │
         │                    ▼
         │          ┌──────────────────┐
         │          │ ERROR: Fix code  │
         │          │ Go back to dev   │
         │          └──────────────────┘
         │
         ▼
    ┌─────────────────────┐
    │  Constructor runs   │
    │  __init__()         │
    │                     │
    │  State initialized  │
    └──────────┬──────────┘
               │
               ▼
    ┌─────────────────────┐
    │  Contract deployed! │
    │  Address: 0x123...  │
    └──────────┬──────────┘
               │
════════════════┼════════════════════════════════════════════════
               │    PHASE 3: INTERACTION
════════════════┼════════════════════════════════════════════════
               │
               ▼
    ┌─────────────────────┐
    │  User finds         │
    │  contract address   │
    └──────────┬──────────┘
               │
               ▼
    ┌─────────────────────┐
    │  Call method:       │
    │                     │
    │  Method: process    │
    │  Args: "data"       │
    └──────────┬──────────┘
               │
               ▼
    ┌─────────────────────────┐
    │  Transaction broadcast  │
    │  to GenLayer network    │
    └──────────┬────────────────┘
               │
        ┌──────┴──────┐
        │             │
        ▼             ▼
┌──────────────┐  ┌──────────────┐
│ Simple call? │  │ AI/Web call? │
│ (read-only)  │  │ (complex)    │
└──────┬───────┘  └──────┬───────┘
       │                 │
       ▼                 ▼
┌──────────────┐  ┌──────────────────┐
│ Execute      │  │ Leader selected  │
│ immediately  │  │                  │
│ Return value │  │ Executes first   │
└──────┬───────┘  └──────┬───────────┘
       │                 │
       │                 ▼
       │          ┌──────────────────┐
       │          │ Does it need     │
       │          │ AI/Web?          │
       │          └──────┬───────────┘
       │                 │
       │          ┌──────┴──────┐
       │          │             │
       │          ▼             ▼
       │    ┌──────────┐  ┌──────────┐
       │    │   YES    │  │    NO    │
       │    └────┬─────┘  └────┬─────┘
       │         │             │
       │         ▼             │
       │    ┌──────────────┐  │
       │    │ Web Fetch    │  │
       │    │ (Flowchart 3)│  │
       │    └──────┬───────┘  │
       │           │          │
       │           ▼          │
       │    ┌──────────────┐  │
       │    │ AI Process   │  │
       │    │ (Flowchart 2)│  │
       │    └──────┬───────┘  │
       │           │          │
       │           ▼          │
       │    ┌──────────────┐  │
       │    │ Validators   │  │
       │    │ compare      │  │
       │    │ results      │  │
       │    └──────┬───────┘  │
       │           │          │
       └───────────┴──────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │  Consensus reached   │
        │  (majority agrees)   │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │  State updated       │
        │  (if write method)   │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │  Result returned     │
        │  to user             │
        │                      │
        │  Success ✓           │
        └──────────┬───────────┘
                   │
════════════════════┼════════════════════════════════════════════
                   │    PHASE 4: MONITORING
════════════════════┼════════════════════════════════════════════
                   │
                   ▼
        ┌──────────────────────┐
        │  User sees result    │
        │  in Studio UI        │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │  Check logs          │
        │  View state          │
        │  Verify output       │
        └──────────────────────┘

════════════════════════════════════════════════════════════════
```

### Lifecycle Timeline

```
Development        Deployment         Execution          Monitoring
    |                  |                  |                  |
    |                  |                  |                  |
┌───▼───┐         ┌───▼───┐         ┌───▼───┐         ┌───▼───┐
│ Code  │────────>│Deploy │────────>│ Call  │────────>│ Check │
│ Write │  Hours  │Studio │ Seconds │Method │ 10-30s  │Result │
└───────┘         └───────┘         └───────┘         └───────┘
    │                  │                  │                  │
    ▼                  ▼                  ▼                  ▼
  Test             Validate           Process            Verify
  Locally          Schema             AI/Web             Output
    │                  │                  │                  │
    ▼                  ▼                  ▼                  ▼
 Iterate           Fix Errors         Consensus          Repeat
    │                  │                  │                  │
    └──────────────────┴──────────────────┴──────────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   Contract    │
                    │   Running &   │
                    │   Reliable    │
                    └───────────────┘
```

---

<a name="diagrams"></a>
##  Architecture Diagrams

### GenLayer Network Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER LAYER                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │   Studio     │  │  dApp Web    │  │    CLI       │          │
│  │  Interface   │  │  Interface   │  │   Tools      │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
│         │                 │                 │                   │
└─────────┼─────────────────┼─────────────────┼───────────────────┘
          │                 │                 │
          └─────────────────┴─────────────────┘
                            │
┌───────────────────────────┼───────────────────────────────────┐
│                     API LAYER                                  │
├───────────────────────────┼───────────────────────────────────┤
│                           │                                    │
│                    ┌──────▼──────┐                            │
│                    │   GenLayer   │                            │
│                    │   JSON-RPC   │                            │
│                    │     API      │                            │
│                    └──────┬───────┘                            │
│                           │                                    │
└───────────────────────────┼───────────────────────────────────┘
                            │
┌───────────────────────────┼───────────────────────────────────┐
│                   CONSENSUS LAYER                              │
├───────────────────────────┼───────────────────────────────────┤
│                           │                                    │
│    ┌──────────────────────┴──────────────────────┐            │
│    │          Validator Network                   │            │
│    │                                              │            │
│    │  ┌──────────┐  ┌──────────┐  ┌──────────┐  │            │
│    │  │Validator │  │Validator │  │Validator │  │            │
│    │  │    1     │  │    2     │  │    3     │  │            │
│    │  │ (Leader) │  │          │  │          │  │            │
│    │  └────┬─────┘  └────┬─────┘  └────┬─────┘  │            │
│    │       │             │             │         │            │
│    │       └─────────────┼─────────────┘         │            │
│    │                     │                       │            │
│    │          ┌──────────▼──────────┐            │            │
│    │          │ Equivalence         │            │            │
│    │          │ Principle Engine    │            │            │
│    │          └─────────────────────┘            │            │
│    └──────────────────────────────────────────────┘            │
│                                                                │
└────────────────────────────────────────────────────────────────┘
                            │
┌───────────────────────────┼───────────────────────────────────┐
│                   EXECUTION LAYER                              │
├───────────────────────────┼───────────────────────────────────┤
│                           │                                    │
│            ┌──────────────▼──────────────┐                    │
│            │   Contract Execution VM     │                    │
│            │   (Python Runtime)          │                    │
│            └──────────┬─────────┬────────┘                    │
│                       │         │                             │
│            ┌──────────▼───┐ ┌──▼─────────────┐               │
│            │   State      │ │  Non-Det       │               │
│            │   Storage    │ │  Operations    │               │
│            └──────────────┘ └──┬─────────┬───┘               │
│                                │         │                    │
└────────────────────────────────┼─────────┼───────────────────┘
                                 │         │
┌────────────────────────────────┼─────────┼───────────────────┐
│                   EXTERNAL INTEGRATIONS                        │
├────────────────────────────────┼─────────┼───────────────────┤
│                                │         │                    │
│                      ┌─────────▼──┐  ┌──▼─────────┐          │
│                      │   LLM      │  │    Web     │          │
│                      │  Providers │  │   Access   │          │
│                      │            │  │            │          │
│                      │ • OpenAI   │  │ • HTTP     │          │
│                      │ • Anthropic│  │ • APIs     │          │
│                      │ • Local    │  │ • Websites │          │
│                      └────────────┘  └────────────┘          │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

### Component Interaction Map

```
┌─────────────┐
│   Smart     │
│  Contract   │◄────┐
│   (Python)  │     │
└──────┬──────┘     │
       │            │
       │ calls      │ returns
       │            │
       ▼            │
┌─────────────┐     │
│ gl.nondet   │     │
│  .exec_     │     │
│  prompt()   │     │
└──────┬──────┘     │
       │            │
       │ sends      │
       │ prompt     │
       │            │
       ▼            │
┌─────────────┐     │
│    LLM      │     │
│  (GPT-4,    │     │
│  Claude)    │     │
└──────┬──────┘     │
       │            │
       │ generates  │
       │ response   │
       │            │
       ▼            │
┌─────────────┐     │
│ Equivalence │     │
│  Principle  │─────┘
│   Engine    │ validates
└─────────────┘
```

### Data Flow Architecture

```
                    INPUT
                      │
                      ▼
            ┌─────────────────┐
            │ User Transaction│
            └────────┬────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
         ▼                       ▼
┌─────────────────┐    ┌─────────────────┐
│ Deterministic   │    │ Non-Deterministic│
│   Operations    │    │   Operations    │
│                 │    │                 │
│ • Math          │    │ • LLM Calls     │
│ • Logic         │    │ • Web Fetch     │
│ • Storage       │    │ • AI Analysis   │
└────────┬────────┘    └────────┬────────┘
         │                      │
         │ Fast Path            │ Consensus Path
         │ (immediate)          │ (20-30s)
         │                      │
         │                      ▼
         │           ┌──────────────────┐
         │           │ Leader Execution │
         │           └────────┬─────────┘
         │                    │
         │                    ▼
         │           ┌──────────────────┐
         │           │ Validator Checks │
         │           └────────┬─────────┘
         │                    │
         │                    ▼
         │           ┌──────────────────┐
         │           │ Equivalence Test │
         │           └────────┬─────────┘
         │                    │
         └────────────────────┘
                      │
                      ▼
            ┌─────────────────┐
            │  Final Result   │
            │  State Updated  │
            └────────┬────────┘
                     │
                     ▼
                   OUTPUT
```

---

<a name="mental-models"></a>
##  Mental Models & Analogies

### The Restaurant Analogy

Understanding GenLayer through a familiar metaphor:

```
TRADITIONAL BLOCKCHAIN = Fast Food Chain
─────────────────────────────────────────
Every location makes burgers EXACTLY the same way
Same ingredients, same recipe, same result
Customer gets IDENTICAL burger everywhere
Fast, predictable, deterministic

GENLAYER = Michelin Star Restaurant
─────────────────────────────────────────
Multiple chefs (validators) create dish
Same recipe, but slight variations
Head chef (leader) proposes dish
Other chefs taste and evaluate
Is it "good enough"? (equivalence principle)
All agree it meets standards → Serve!
Creative, intelligent, non-deterministic
```

### Visualizing Consensus

**Traditional Blockchain:**
```
Question: What is 2 + 2?

Validator 1: ┌───┐
             │ 4 │ ✓
             └───┘

Validator 2: ┌───┐
             │ 4 │ ✓
             └───┘

Validator 3: ┌───┐
             │ 4 │ ✓
             └───┘

Result: EXACT MATCH
Consensus: IMMEDIATE
```

**GenLayer:**
```
Question: Is this news positive or negative?

Validator 1: ┌──────────┐
             │Positive  │
             │Score: 8.5│ ✓
             └──────────┘

Validator 2: ┌──────────┐
             │Positive  │
             │Score: 8.3│ ✓
             └──────────┘

Validator 3: ┌──────────┐
             │Positive  │
             │Score: 8.7│ ✓
             └──────────┘

Result: SIMILAR (within tolerance)
Consensus: EQUIVALENCE ✓
```

### The Layer Cake Model

```
┌──────────────────────────────────────┐
│        APPLICATION LAYER              │  ← Your Contract Logic
│  (What you write)                    │
├──────────────────────────────────────┤
│        INTELLIGENCE LAYER             │  ← AI + Web Integration
│  (GenLayer Magic)                    │
├──────────────────────────────────────┤
│        CONSENSUS LAYER                │  ← Equivalence Principle
│  (Agreement Mechanism)               │
├──────────────────────────────────────┤
│        BLOCKCHAIN LAYER               │  ← State & Transactions
│  (Immutable Storage)                 │
└──────────────────────────────────────┘
```

Each layer builds on the one below:
1. **Blockchain** = Stores data immutably
2. **Consensus** = Validators agree
3. **Intelligence** = AI & web access
4. **Application** = Your smart contract

---

<a name="patterns"></a>
##  Common Patterns Visualized

### Pattern 1: Simple AI Analysis

```
┌─────────────────────────────────────────────────────────┐
│                    USER INPUT                            │
│                  "Analyze this text"                     │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
              ┌────────────────┐
              │  Your Contract │
              │                │
              │  @gl.public    │
              │  def analyze() │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │ Wrap in        │
              │ non-det        │
              │ function       │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │ Call LLM       │
              │ gl.nondet.     │
              │ exec_prompt()  │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │ Equivalence    │
              │ Principle      │
              │ .strict_eq()   │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │ Return Result  │
              └────────────────┘
```

**Code Pattern:**
```python
@gl.public.write
def analyze_text(self, text: str) -> dict:
    def ai_analysis() -> dict:
        prompt = f"Analyze: {text}"
        result = gl.nondet.exec_prompt(
            prompt,
            response_format='json'
        )
        return result
    
    return gl.eq_principle.strict_eq(ai_analysis)
```

### Pattern 2: Web + AI Combo

```
┌─────────────────────────────────────────────────────────┐
│              USER INPUT: URL + QUESTION                  │
│     verify_article("https://...", "Is it positive?")    │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
              ┌────────────────┐
              │  Fetch Web     │
              │  Content       │
              │                │
              │  gl.nondet.web │
              │  .render()     │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │  Web Content   │
              │  Retrieved     │
              │  "Article text"│
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │  Pass to LLM   │
              │  for Analysis  │
              │                │
              │  gl.nondet.    │
              │  exec_prompt() │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │  AI Analyzes   │
              │  Web Content   │
              │  Returns JSON  │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │  Equivalence   │
              │  Check         │
              └────────┬───────┘
                       │
                       ▼
              ┌────────────────┐
              │  Final Answer  │
              │  "Positive ✓"  │
              └────────────────┘
```

**Code Pattern:**
```python
@gl.public.write
def verify_article(self, url: str, question: str) -> dict:
    def web_and_ai() -> dict:
        # Step 1: Fetch web
        content = gl.nondet.web.render(url, mode='text')
        
        # Step 2: Ask AI
        prompt = f"Content: {content}\nQuestion: {question}"
        result = gl.nondet.exec_prompt(
            prompt,
            response_format='json'
        )
        return result
    
    return gl.eq_principle.strict_eq(web_and_ai)
```

### Pattern 3: Multi-Source Verification

```
                    ┌────────────┐
                    │   Query    │
                    │ "BTC Price"│
                    └──────┬─────┘
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
         ▼                 ▼                 ▼
    ┌────────┐        ┌────────┐        ┌────────┐
    │Source 1│        │Source 2│        │Source 3│
    │CoinGecko│       │Binance │        │CoinCap │
    └───┬────┘        └───┬────┘        └───┬────┘
        │                 │                 │
        │ $50,100         │ $50,050         │ $50,150
        │                 │                 │
        └─────────────────┼─────────────────┘
                          │
                          ▼
                  ┌───────────────┐
                  │  Calculate    │
                  │  Median       │
                  │               │
                  │  $50,100      │
                  └───────┬───────┘
                          │
                          ▼
                  ┌───────────────┐
                  │  AI Validates │
                  │  "Reasonable?"│
                  └───────┬───────┘
                          │
                          ▼
                  ┌───────────────┐
                  │  Consensus    │
                  │  Result       │
                  │  $50,100 ✓    │
                  └───────────────┘
```

---

<a name="troubleshooting"></a>
## Troubleshooting Visual Guide

### Decision Tree: Why Isn't My Contract Working?

```
                ┌──────────────────┐
                │  Contract Error  │
                └────────┬─────────┘
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
┌─────────────────┐            ┌─────────────────┐
│ Deployment      │            │  Execution      │
│ Failed?         │            │  Failed?        │
└────────┬────────┘            └────────┬────────┘
         │                              │
         ▼                              ▼
┌─────────────────┐            ┌─────────────────┐
│ Check:          │            │ Check:          │
│                 │            │                 │
│ ✓ Header line   │            │ ✓ LLM format    │
│   correct?      │            │   json?         │
│                 │            │                 │
│ ✓ __init__      │            │ ✓ Equivalence   │
│   exists?       │            │   principle?    │
│                 │            │                 │
│ ✓ Type hints    │            │ ✓ Web URL       │
│   added?        │            │   allowed?      │
│                 │            │                 │
│ ✓ Decorators    │            │ ✓ Timeout       │
│   present?      │            │   issue?        │
└─────────────────┘            └─────────────────┘
```

### Common Error Patterns

```
ERROR: "Could not load contract schema"
═══════════════════════════════════════
Problem Area: Deployment / Syntax

Check:
┌─────────────────────────────────────┐
│ 1. First line header?               │
│    # {"Depends": "py-genlayer:test"}│
│    (No spaces in JSON!)             │
│                                     │
│ 2. Class inheritance?               │
│    class X(gl.Contract):            │
│                                     │
│ 3. __init__ method exists?          │
│    def __init__(self):              │
│                                     │
│ 4. Type hints on all methods?       │
│    def process(self, x: str) -> str │
└─────────────────────────────────────┘


ERROR: "Transaction timeout"
═══════════════════════════
Problem Area: Execution

Likely Causes:
┌─────────────────────────────────────┐
│ 1. LLM call too complex             │
│    → Simplify prompt                │
│                                     │
│ 2. Web fetch slow/blocked           │
│    → Check URL allowed              │
│    → Try different source           │
│                                     │
│ 3. Infinite loop in code            │
│    → Review logic                   │
│                                     │
│ 4. Too many API calls               │
│    → Reduce operations              │
└─────────────────────────────────────┘


ERROR: "Consensus failed"
════════════════════════
Problem Area: Equivalence

Causes:
┌─────────────────────────────────────┐
│ 1. LLM outputs too different        │
│    → Make prompt more specific      │
│    → Force JSON format              │
│                                     │
│ 2. Web content changed              │
│    → Normal, retry transaction      │
│                                     │
│ 3. Wrong equivalence function       │
│    → Use strict_eq for most cases   │
│    → Use prompt_comparative for     │
│      scores with tolerance          │
└─────────────────────────────────────┘
```

### Performance Optimization Flow

```
                ┌──────────────┐
                │  Slow        │
                │  Contract?   │
                └──────┬───────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
┌──────────┐   ┌──────────┐   ┌──────────┐
│LLM call  │   │Web fetch │   │Complex   │
│taking    │   │taking    │   │logic     │
│too long? │   │too long? │   │taking    │
│          │   │          │   │too long? │
└────┬─────┘   └────┬─────┘   └────┬─────┘
     │              │              │
     ▼              ▼              ▼
┌─────────┐   ┌──────────┐   ┌──────────┐
│Optimize:│   │Optimize: │   │Optimize: │
│         │   │          │   │          │
│• Shorter│   │• Simpler │   │• Cache   │
│  prompt │   │  URLs    │   │  results │
│         │   │          │   │          │
│• More   │   │• Use     │   │• Reduce  │
│  specific│  │  APIs not│   │  loops   │
│  format │   │  websites│   │          │
│         │   │          │   │• Pre-    │
│• Remove │   │• Add     │   │  compute │
│  fluff  │   │  timeout │   │          │
└─────────┘   └──────────┘   └──────────┘
```

---

##  Learning Path Visualization

### From Beginner to Expert

```
LEVEL 1: BASICS
═══════════════
┌─────────────────────────────────────┐
│ • Understand GenLayer concepts      │
│ • Deploy first contract             │
│ • Call simple methods               │
│                                     │
│ Time: 1-2 hours                     │
│ Goal: Hello World deployed          │
└─────────────────────────────────────┘
              │
              ▼
LEVEL 2: AI INTEGRATION
════════════════════════
┌─────────────────────────────────────┐
│ • Use gl.nondet.exec_prompt()       │
│ • Apply equivalence principle       │
│ • Handle JSON responses             │
│                                     │
│ Time: 2-4 hours                     │
│ Goal: AI-powered contract working   │
└─────────────────────────────────────┘
              │
              ▼
LEVEL 3: WEB INTEGRATION
═════════════════════════
┌─────────────────────────────────────┐
│ • Fetch web data                    │
│ • Combine web + AI                  │
│ • Handle blocked URLs               │
│                                     │
│ Time: 3-5 hours                     │
│ Goal: Real-world data contract      │
└─────────────────────────────────────┘
              │
              ▼
LEVEL 4: COMPLEX PATTERNS
══════════════════════════
┌─────────────────────────────────────┐
│ • Multi-source verification         │
│ • Advanced consensus strategies     │
│ • Optimize for performance          │
│                                     │
│ Time: 5-10 hours                    │
│ Goal: Production-ready system       │
└─────────────────────────────────────┘
              │
              ▼
LEVEL 5: MASTERY
═════════════════
┌─────────────────────────────────────┐
│ • Build complete dApps              │
│ • Custom equivalence functions      │
│ • Advanced architecture             │
│                                     │
│ Time: 10+ hours                     │
│ Goal: Ship real products            │
└─────────────────────────────────────┘
```

---

##  Quick Reference Cheat Sheet

### The Essential Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                    GENLAYER ESSENTIALS                        │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  CONTRACT STRUCTURE:                                         │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ # {"Depends": "py-genlayer:test"}  ← FIRST LINE       │  │
│  │                                                        │  │
│  │ from genlayer import *              ← IMPORT          │  │
│  │                                                        │  │
│  │ class MyContract(gl.Contract):      ← INHERIT         │  │
│  │     def __init__(self):             ← CONSTRUCTOR     │  │
│  │         self.data = "value"                           │  │
│  │                                                        │  │
│  │     @gl.public.write                ← DECORATOR       │  │
│  │     def process(self, x: str) -> str: ← TYPE HINTS    │  │
│  │         def ai_work() -> dict:      ← WRAP NON-DET    │  │
│  │             result = gl.nondet.exec_prompt(           │  │
│  │                 prompt,                               │  │
│  │                 response_format='json' ← JSON FORMAT  │  │
│  │             )                                         │  │
│  │             return result                            │  │
│  │                                                        │  │
│  │         return gl.eq_principle.strict_eq(ai_work)     │  │
│  │                              ↑                        │  │
│  │                              DOT not underscore!      │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
│  KEY APIS:                                                   │
│  • LLM: gl.nondet.exec_prompt(prompt, response_format='json')│
│  • Web: gl.nondet.web.render(url, mode='text')              │
│  • Consensus: gl.eq_principle.strict_eq(function)           │
│  • Sender: gl.tx.from_address                               │
│                                                               │
│  REMEMBER:                                                   │
│  ✓ Wrap AI/Web in functions                                 │
│  ✓ Always use response_format='json'                        │
│  ✓ Type hint everything                                     │
│  ✓ Check URL is not blocked                                 │
│  ✓ Wait 20-30s for AI consensus                             │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

## Conclusion

GenLayer intelligent contracts operate on a fundamentally different model than traditional smart contracts:

**Traditional:** Code → Deterministic Execution → Exact Results → Consensus
**GenLayer:** Code → AI/Web → Variable Results → Equivalence → Consensus

The flowcharts and diagrams in this guide show:
1. How transactions flow through the network
2. How AI consensus actually works
3. How web data gets integrated
4. The complete lifecycle of intelligent contracts

**Key Takeaway:** GenLayer doesn't require exact matches—it requires *equivalent* outputs through intelligent consensus.

---

## Further Resources

- **Official Docs:** https://docs.genlayer.com
- **Studio:** https://studio.genlayer.com
- **Examples:** GitHub repositories with working contracts
- **Community:** Discord for real-time help

**Bookmark this guide and refer back when building!** 🚀

---

*Last Updated: 2026*
*Version: 1.0*
*License: Open Source - Share freely!*

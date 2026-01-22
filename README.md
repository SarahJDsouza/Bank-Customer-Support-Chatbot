# Bank-Customer-Support-Chatbot

## Problem Statement and Motivation
Help desks are often overwhelmed with incoming tickets, posing a significant challenge to customer support teams. In the banking sector, where time and accuracy are crucial, effective ticket triage is vital to ensure seamless resolution and maintain a positive customer experience. Thus there is a need to assist banks to allocate customer support tickets that flags critical issues like:
- Fraud
- Unauthorized transactions
- Account lock requests
to their respectives departments and suggest responses, and provide relevant knowledge base document.

## Triage Labels Used:
**HIGH_RISK_FRAUD**
Suspected fraud or unauthorized financial activity
**URGENT_ACCOUNT_ACCESS**
Account lockouts, compromised credentials, or time-sensitive access issues
**ROUTINE_QUERY**
Low-risk, standard customer service requests
**ESCALATE_TO_COMPLIANCE**
Cases requiring regulatory interpretation or compliance team review
**ABSTAIN**
Insufficient clarity or unsafe context; requires direct human handling without AI guidance

##  Corpus Sources Used: 
- U.S. regulatory and supervisory guidance, including FDIC consumer compliance materials and CFPB Regulation E (error resolution and unauthorized transfers).

- Fraud and risk management documentation from FDIC, FFIEC (BSA/AML Examination Manual), and Federal Reserve–related publications.

- Industry and financial institution resources, including JPMorgan complaint handling materials and third-party banking risk analyses.

## METHOD
Built and evaluated a domain chatbot under three “information channels”:
• Mode 0: Retrieval-only: return quoted passages + doc IDs, no free-form answer
• Mode 1: RAG: retrieve passages, then generate an answer with citations
• Mode 2: LLM-only: generate an answer with no retrieval

Implemented:
1. Retriever (vector search; top-k retrieval)
2. Prompted generator (LLM call)
   - The test set consists of 30 prompts with 10 normal, 10 ambiguous / missing info (should often abstain) and 10 adversarial (prompt injection / “ignore guidelines” / “be confident”) prompts.
4. Three modes: retrieval-only, RAG, LLM-only
5.  k ∈ {2, 5} (for RAG)
6.  temperature ∈ {0.0, 0.7}



### Comparision of different modes

<img width="1058" height="267" alt="image" src="https://github.com/user-attachments/assets/bf62a803-911c-42c5-9b3c-e210468a61a6" />


## Ethical Themes
**Ethical Value (The Priority)**
- Accuracy: Ensuring fraud isn't mislabeled as routine, causing financial loss.
- Customer Safety: Prioritizing human empathy for distressed fraud victims.
- Regulatory Compliance: Adhering strictly to Regulation E investigation timelines.

**Real-World Harm Scenario**
- The Narrative: Misclassifying a high-risk fraud report (e.g., an unauthorized transfer) as a routine inquiry.
- The Impact: This delays critical investigation windows, provides "overconfident" false assurances to the victim, and exposes the bank to legal liability for negligent misrepresentation.
Educational and explanatory content on unauthorized transactions and fraud investigations from trusted financial compliance sources.

**"Abstain & Escalate" Rules**
Use a "Safety Shield" or "Red Flag" icon for these specific triggers:
Refuse to answer/Route to human when:
- Urgency & Distress: High-priority financial crises or emotional appeals.
- High Complexity: Requests that are ambiguous or highly individualized.
- Unclear Policy: When internal guidelines or regulations are conflicting.
- Security Threats: Detection of "Prompt Injection" or attempts to bypass controls.

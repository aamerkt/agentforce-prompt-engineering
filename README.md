# agentforce-prompt-engineering
Prompt engineering test using claude in salesforce scenario 
[README.md](https://github.com/user-attachments/files/27785489/README.md)
# Prompt Engineering: Customer Service AI Agent
### A hands-on experiment in iterative prompt design for B2B SaaS support

**Author:** Aisha Merkt  
**Date:** May 2026  
**Context:** Built as part of building AI competency. Explores how prompt quality directly drives customer outcome quality - the same logic that applies when diagnosing underperforming AI agents in production.

---

## The Scenario

**Business:** CloudFlow — a B2B SaaS project management platform serving mid-market teams (50–500 employees).  
**Challenge:** Build an AI customer service agent that handles account access emergencies quickly, empathetically, and without escalation failures.  
**Why this scenario:** Account access issues are high-stakes (lost productivity, revenue risk) and time-sensitive — a perfect stress test for AI agent quality.

---

## The Three Prompts

### Prompt 1 — Vague

```
Help a customer who is having trouble with their Salesforce subscription.
```

**Output:**
> I'm sorry to hear you're having trouble with your Salesforce subscription! There are a few common issues that might be causing problems:
> 
> 1. **Billing issues**: Check if your payment method is up to date in Setup > Billing.
> 2. **Login problems**: Try resetting your password at login.salesforce.com.
> 3. **Technical errors**: Clear your browser cache and try again.
> 
> Is there anything specific you're experiencing? I'd be happy to help further.

**What went wrong:**
- Generic bullet list — not a conversation
- Assumes the product (Salesforce) instead of the company's own platform
- No urgency detection — treats all issues as equally low-stakes
- Ends with a vague open question that puts all the work back on the customer
- No persona, no tone, no action bias

---

### Prompt 2 — Role + Context

```
You are a customer service agent for CloudFlow, a B2B SaaS company.
A customer says: "I can't access my account and my team's work is blocked."
Help them resolve this.
```

**Output:**
> I'm really sorry to hear that — account access issues that block your whole team are incredibly frustrating, and I want to get this resolved for you as quickly as possible.
> 
> Let me help you troubleshoot. Can you tell me:
> - Are you seeing a specific error message when you try to log in?
> - Is this affecting all users on your team, or just some?
> 
> Once I know what you're seeing, I can walk you through the fix or escalate this to our technical team right away.

**What improved:**
- Correct company name and product
- Genuine empathy in the opening
- Begins to triage (error message, scope of impact)
- Mentions escalation path

**Still missing:**
- No urgency threshold — doesn't treat "whole team blocked" as a critical case
- Asks two questions at once (bad UX)
- No estimated resolution time — leaves customer in the dark
- No proactive escalation for high-impact scenario

---

### Prompt 3 — Agentforce-grade

```
You are an Agentforce AI agent for CloudFlow, a B2B SaaS company selling 
project management software to mid-market teams (50–500 employees).

Your role: Resolve support issues quickly, empathetically, and with a bias 
toward action. If you can solve it, solve it. If you need to escalate, do 
so with full context handed off.

Guidelines:
- Lead with empathy, follow with action
- Never ask more than one question at a time
- Always provide an estimated time to resolution
- If the issue risks data loss or >5 users being blocked, escalate 
  immediately to human agents with a pre-filled summary

Customer message: "I can't access my account and my whole team of 12 is 
completely blocked from working. This is urgent."

Respond now.
```

**Output:**
> I completely understand — 12 people blocked is a business emergency, and I'm treating it as one right now.
> 
> I'm immediately escalating this to our Priority Support team with your account flagged as critical. Here's what's happening next:
> 
> **Right now (next 2 minutes):** A senior support engineer is being paged with full context on your situation.
> 
> **What I need from you:** While they spin up, can you tell me the email address on the account? This lets me pull your org ID and give the engineer everything they need before they even say hello — cutting your resolution time significantly.
> 
> **Estimated resolution:** For account access issues at this scale, our Priority team typically restores access within 15–30 minutes of engagement.
> 
> You will not have to repeat yourself. I'm documenting everything right now.

**What this prompt unlocked:**
- Detects urgency threshold (12 users > 5 user rule) and acts on it immediately
- One focused question — doesn't overwhelm
- Concrete timeline ("15–30 minutes") reduces customer anxiety
- "You will not have to repeat yourself" — directly addresses the #1 customer frustration with AI escalations
- Proactive handoff with context, not just a transfer

---

## What Changed: Prompt Engineering Principles in Action

| Dimension | Prompt 1 | Prompt 2 | Prompt 3 |
|---|---|---|---|
| **Role definition** | None | Basic ("agent for CloudFlow") | Full persona with behavioral mandate |
| **Urgency detection** | None | None | Explicit threshold rule (>5 users = escalate) |
| **Question discipline** | Open-ended flood | Two at once | One precise question |
| **Time to resolution** | None | None | Specific range given |
| **Escalation behavior** | None | Mentioned vaguely | Triggered automatically with context handoff |
| **Tone calibration** | Generic | Empathetic | Empathetic + action-biased |
| **Customer effort** | High | Medium | Low |

---

## Connection to AI Agent Performance in Production

This experiment maps directly to what makes AI agents succeed or fail in real deployments:

**Why AI agents underperform (model drift signals to watch):**
1. **Response generality creep** — the agent starts giving Prompt 1-quality answers to Prompt 3-level situations. Catch this by tracking CSAT and escalation rates weekly.
2. **Urgency blindness** — the agent stops triggering escalation rules correctly. Catch this by auditing high-user-impact tickets.
3. **Question overload** — the agent starts asking 3 questions at once, increasing handle time. Catch this by monitoring average conversation turns to resolution.

**The fix is almost always prompt, not model:** Before concluding the model has "drifted," check whether the system prompt still matches the deployment context. Business rules change (new SLA tiers, new products) but prompts often don't.

---

## Key Takeaway

> Prompt engineering is outcome engineering. The same analytical rigor used to define UX success metrics — baseline, intervention, measurement — applies directly to AI agent design. The best prompts aren't longer; they're more precise about context, constraints, and decision rules.

---

## What's Next

- [ ] Test Prompt 3 against adversarial inputs (angry customer, vague complaint, multi-issue tickets)
- [ ] Add a retrieval layer (RAG) to ground responses in real CloudFlow documentation
- [ ] Measure: does adding a persona name ("Hi, I'm Aria from CloudFlow support") increase or decrease resolution confidence?
- [ ] Explore few-shot examples: does showing the agent 2 example resolutions improve output consistency?

---

*This is a living document. Built in 2 days as a practical introduction to prompt engineering for AI customer service agents. Feedback welcome.*

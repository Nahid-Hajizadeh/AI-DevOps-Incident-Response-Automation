# AI DevOps Incident Response Automation

An AI-assisted DevOps workflow built with n8n to automate incident intake, analysis, validation, human escalation, and log analysis.

## Overview

This project demonstrates how AI can assist with first-level DevOps incident handling while keeping important decisions under deterministic workflow controls.

The workflow receives infrastructure alerts through a webhook, analyzes the incident with an LLM, validates the AI output using independent rules, stores the incident, and escalates cases that require human review.

## Workflow

```text
Monitoring Alert
      ↓
Webhook
      ↓
Input Validation
      ↓
AI Incident Analysis
      ↓
Structured Output
      ↓
AI Decision Validation
      ↓
Store Incident
      ↓
Human Review?
   ┌──┴──┐
  YES    NO
   ↓      ↓
Gmail   Log Analysis
   ↓      ↓
Update   AI Log Analysis
Status      ↓
         Decision Validation
              ↓
         Action Routing
```

## Key Features

- Webhook-based incident intake
- Required-field and severity validation
- AI-powered incident classification
- Structured JSON output
- Confidence scoring
- Independent AI decision guardrails
- Automatic escalation for:
  - Critical incidents
  - Low-confidence analysis
- Human-in-the-loop email notification
- Incident persistence using n8n Data Tables
- AI-assisted application log analysis
- Allowlisted recommended actions
- Controlled action routing with n8n Switch

## AI Safety & Guardrails

The AI does not directly execute infrastructure commands.

The workflow independently validates AI recommendations before routing them.

For example:

- Critical incidents always require human review.
- Confidence below `0.80` triggers human review.
- Recommended actions must belong to a predefined allowlist.
- Destructive or irreversible actions are not permitted.
- Root causes are treated as hypotheses rather than confirmed facts.

This separation between AI analysis and deterministic workflow controls helps reduce the risk of unsafe automated actions.

## Example Incident

### Input

```json
{
  "incident_id": "INC-1001",
  "source": "prometheus",
  "server": "production-api-01",
  "service": "api",
  "alert_type": "service_down",
  "severity": "critical",
  "message": "The API service is not responding.",
  "timestamp": "2026-09-25T10:30:00Z"
}
```

### AI Analysis

```json
{
  "incident_id": "INC-1001",
  "incident_type": "service_unresponsive",
  "severity": "critical",
  "recommended_action": "check_service_status",
  "confidence": 0.75,
  "needs_human_review": true
}
```

Because the incident is critical and confidence is below the workflow threshold, the incident is escalated for human review.

## Log Analysis

For incidents that do not require immediate human escalation, the workflow can perform a second analysis stage using application logs.

Example findings:

```text
Technical issue:
Database connection failures

Evidence:
- Database connection timeout
- Failed to establish database connection
- Retrying database connection

Probable root cause:
Potential database downtime, network connectivity issues,
or database connectivity/configuration problems

Confidence:
0.85
```

## Technology Stack

- n8n
- Large Language Models
- OpenRouter
- Webhooks
- Structured Output Parser
- Gmail
- n8n Data Tables
- JavaScript

## Project Architecture

The project follows a simple principle:

```text
AI analyzes
    ↓
Workflow validates
    ↓
Rules control
    ↓
Human reviews when necessary
```

The AI is used for analysis and recommendations, while deterministic n8n logic provides the final safety controls.

## Current Scope

This project focuses on incident intake, analysis, validation, escalation, persistence, and log analysis.

It intentionally does not perform unrestricted automated infrastructure remediation.

Production deployment would require additional controls such as authenticated monitoring integrations, secure execution environments, access control, audit logging, and tested remediation policies.

## Portfolio Purpose

This project demonstrates practical use of AI and workflow automation for DevOps operations, with an emphasis on:

- AI-assisted decision making
- Workflow orchestration
- Validation and guardrails
- Human-in-the-loop automation
- Safe automation design

## Author

**Nahid Hajizadeh**

AI Workflow Automation / n8n

GitHub:
https://github.com/Nahid-Hajizadeh

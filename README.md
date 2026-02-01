AI Incident Triage & Ops Alerting System
Overview:-

This project is an internal operations automation system that helps engineering teams reduce alert noise and escalate only real incidents.

It is not a chatbot and not a user-facing product.
It is designed as a rule-first, AI-second incident triage pipeline, similar to what startups and engineering teams use internally.

Problem This Solves:-

In production systems:

-Hundreds of errors occur every day

-Most errors do not require human attention

-Alerting everything causes alert fatigue

-Important incidents get ignored

This system solves that by:

-Filtering noise early

-Auto-resolving known low-risk issues

-Using AI only when human judgment is needed

-Alerting humans only for high-impact incidents

-Logging all decisions for audit and learning

Who This Is For:-

Backend Engineers

DevOps / SRE teams

Startup engineering teams

How the System Works (Simple Flow):-
Incoming Event
   ↓
Noise Filter (Non-prod / test events dropped)
   ↓
Known Error Auto-Resolve
   ↓
Obvious Critical Check (Rule-based)
   ↓
AI Severity Classification (only if needed)
   ↓
AI Guardrails (confidence & validation)
   ↓
Final Severity Decision
   ↓
Severity-based Routing
   ↓
Incident Logging (Google Sheets)

Input Format (Normalized Event):-

The system expects structured events emitted by backend services or monitoring pipelines.

Example input:
{
  "service": "auth-api",
  "event_type": "error",
  "error_name": "JWTExpired",
  "message": "JWT token expired during login",
  "count": 120,
  "time_window_minutes": 5,
  "environment": "production",
  "severity_hint": "LOW",
  "timestamp": "2026-01-26T10:15:00Z"
}
Raw logs are intentionally out of scope.
This system operates on normalized operational events.


Severity Handling Logic:-
Severity	Behavior
LOW	      Logged only (no alert)
MEDIUM	  Logged, optional review
HIGH	    Immediate alert
CRITICAL	Immediate alert + escalation

Severity hints from input are advisory only.
Final severity is determined by rules first, then AI judgment if needed.

AI Usage (Safe & Controlled):-

AI is used only when deterministic rules cannot decide severity.

Safety measures:

AI outputs must include confidence

Low-confidence results are downgraded

Severity values are validated

AI is skipped for obvious critical incidents

This prevents hallucinations and false alerts.

Incident Logging:-

All incidents are logged to Google Sheets as an audit trail.

Logged fields include:

Service

Error name

Final severity

Decision source (rule-based / AI-assisted)

Confidence (if AI used)

Action taken

Timestamp

This enables:

Post-incident analysis

Trend identification

Rule improvement over time

Why No Frontend?

This is internal ops tooling, not a user-facing application.

Engineers interact with the system via:

-Alerts (for high severity incidents)

-Logs (Google Sheets)

This mirrors real-world production practices.

Tech Stack:-

-n8n (workflow orchestration)

-Webhooks (event ingestion)

-Groq LLM (AI severity classification)

-Google Sheets (audit logging)

Future enhancements (conceptual, not implemented):-

-Known-error learning from past incidents

-Integration with observability tools

-Persistent database instead of Sheets

Key Design Principles:-

-Rules before AI

-AI only where judgment is required

-Avoid alert fatigue

-Prefer simplicity over overengineering

-Every decision must be auditable


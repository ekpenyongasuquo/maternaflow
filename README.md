# MaternaFlow

Agentic maternal care coordination system for Nigerian 
teaching hospitals built on UiPath Automation Cloud.

## Problem
The ANC Paradox: Nigerian women attend antenatal clinics 
but still die from preventable complications due to broken 
handoffs between nurses, labs, doctors, and specialists.

## Solution
MaternaFlow uses UiPath Maestro to orchestrate every patient 
handoff as a tracked case with AI agents and humans at 
critical decision points.

## UiPath Components Used
- UiPath Maestro (BPMN Agentic Process)
- UiPath Agent Builder
- UiPath API Workflows
- UiPath Orchestrator

  ## Agent Type

This solution utilizes **Low-code Agents** built through UiPath Studio 
Web (Agentic Process with Autopilot-assisted BPMN generation) combined 
with **API Workflow integrations** connecting to CliniqBridge, an 
externally coded FHIR agent built in Python/FastAPI.

The solution therefore combines:
- Low-code agents: UiPath Maestro BPMN process with Autopilot
- Coded external agent: CliniqBridge (Python/FastAPI/MCP server)

## External Services
- CliniqBridge FHIR API (https://cliniqbridge.onrender.com)
- HAPI FHIR R4 Server
- Africa's Talking SMS API

## Stages
1. Patient Registration & ANC Intake
2. ANC Visit Coordination
3. Lab Result Routing (4-hour SLA)
4. Specialist Referral Management
5. Delivery & Case Closure

## Setup Instructions
1. Clone this repository
2. Access UiPath Automation Cloud
3. Import each stage BPMN file
4. Configure CliniqBridge API connection
5. Set up Africa's Talking SMS credentials

## Coding Agents
Claude Code was used to build the CliniqBridge FHIR 
integration layer via UiPath for Coding Agents.

## License
=======
Agentic maternal care coordination workflow for Nigerian teaching hospitals, built on UiPath Maestro with live FHIR integration.

## The Problem

Nigeria has one of the world's highest maternal mortality rates despite reasonable antenatal care (ANC) attendance — a pattern often called the ANC Paradox. Women attend clinics but still face preventable complications because of broken handoffs between nurses, doctors, labs, and specialists in resource-constrained public hospitals like the University of Calabar Teaching Hospital (UCTH).

There is no tool today that tracks a single maternal patient's journey across all of these handoffs with visibility, audit trail, and automatic escalation when something is missed.

## What MaternaFlow Does

MaternaFlow models the full maternal care journey as a single end-to-end BPMN process in UiPath Maestro, covering five coordinated stages:

1. **Patient Registration & ANC Intake** — validates documents, retrieves FHIR history, flags high-risk indicators, midwife review
2. **ANC Visit Coordination** — schedules visits per WHO guidelines, sends SMS reminders, escalates missed appointments after 48 hours
3. **Lab Result Routing** — retrieves observations from FHIR, auto-logs normal results, routes abnormal results to a doctor with a 4-hour SLA, escalates breaches to a senior doctor
4. **Specialist Referral** — matches high-risk patients to specialists, generates referral letters, handles acceptance/rejection with escalation
5. **Delivery & Case Closure** — records delivery outcome, generates postnatal care plan, assigns community health worker follow-up, closes the case

Each stage includes clear swimlanes for Patient, Nurse/Midwife, Doctor, Specialist, and System actors, with decision gateways and escalation paths built into the BPMN.

## UiPath Components Used

- **UiPath Maestro** — models and orchestrates the end-to-end BPMN process across all five stages
- **UiPath Studio (Agentic Process)** — process design and Autopilot-assisted BPMN generation
- **UiPath API Workflows** — six live HTTP integrations connecting Maestro tasks to CliniqBridge

## External Services

- **CliniqBridge** — a FHIR R4 MCP server built for this project, exposing `get_patient_summary`, `get_conditions`, `get_medications`, `get_encounters`, `get_observations`, and `create_observation` tools. Deployed on Render: https://cliniqbridge.onrender.com
- **HAPI FHIR R4 public test server** — used as the FHIR data source for development and integration testing

## Current State and Known Limitations

This submission is an honest, in-progress prototype rather than a finished production system. Specifically:

- All six FHIR API integrations are wired into the Maestro BPMN and confirmed returning HTTP 200 responses with real FHIR-shaped data from a public test server
- Patient data used in testing is from the HAPI FHIR public test server, not live UCTH patient records
- Human tasks (midwife, doctor, specialist review) are modeled in the BPMN but are not yet connected to UiPath Action Center for live human interaction
- No live process instance has been run end-to-end through Orchestrator due to sandbox constraints in the hackathon environment

These gaps are documented transparently here rather than glossed over, in the interest of an accurate submission.

## Setup Instructions

1. Clone this repository
2. Deploy CliniqBridge (see below) or use the live instance at https://cliniqbridge.onrender.com
3. In UiPath Automation Cloud, import the BPMN process into a new Agentic Process solution in Studio
4. For each system task calling CliniqBridge, configure an API Workflow with:
   - Method: POST
   - URL: `https://cliniqbridge.onrender.com/mcp`
   - Header: `Content-Type: application/json`
   - Body: JSON-RPC 2.0 payload calling the relevant tool (see `/cliniqbridge/main.py` for tool schemas)
5. Publish and deploy the solution through Studio

### Running CliniqBridge locally

```bash
cd cliniqbridge
pip install -r requirements.txt --break-system-packages
uvicorn main:app --reload
```

## Prerequisites

- UiPath Automation Cloud account with Studio, Agents, and Maestro access
- Python 3.10+ for running CliniqBridge locally
- A FHIR R4 server endpoint (defaults to the public HAPI FHIR test server)

## Coding Agents

Claude Code (via Claude, Anthropic) was used throughout development to design and extend the CliniqBridge FHIR integration layer, including adding the `get_observations` and `create_observation` tools used by Stages 3 and 5 of the workflow.


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
MIT
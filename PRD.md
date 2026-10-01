# Product Requirements Document: MediCare Agent

## 1. Overview

MediCare Agent is an LLM-driven clinical-support prototype that analyzes structured patient data and helps care teams identify follow-up needs. It supports two primary workflows:

1. Detailed review of a single patient record.
2. Identification and prioritization of patients who missed appointments.

The product is intended to support clinical review and operational outreach. It must not diagnose, prescribe treatment, or replace clinician judgment.

## 2. Problem Statement

Care teams need a consistent way to review patient records, recognize potentially important clinical context, and prioritize outreach after missed appointments. Manual review of diagnoses, medications, laboratory results, vital signs, appointment history, and clinical notes is time-consuming. MediCare Agent should retrieve the relevant record data through tools and use an LLM to produce a transparent, data-grounded summary and recommended next steps for human review.

## 3. Goals

- Load, validate, and profile the supplied patient dataset.
- Provide reliable tools for patient lookup, patient summaries, risk-context retrieval, and missed-appointment identification.
- Implement an autonomous LLM agent that selects and calls these tools as needed.
- Generate an evidence-grounded analysis for one patient selected by `patient_id`.
- Produce a prioritized, reviewable follow-up list for patients who missed appointments.
- Clearly separate source data, LLM-generated insights, and suggested follow-up actions.

## 4. Non-Goals

- Making a diagnosis, prescribing medication, or giving emergency medical advice.
- Replacing a clinician's assessment or existing clinical workflow.
- Modifying patient records, scheduling appointments, or contacting patients directly.
- Supporting data sources beyond the supplied dataset in the initial version.

## 5. Users

| User | Need |
| --- | --- |
| Care coordinator | Find missed appointments and organize outreach by priority. |
| Clinician | Review a concise, evidence-backed patient summary before deciding on care. |
| Operations reviewer | Validate outputs, data quality, and follow-up rationale. |

## 6. Input Data

The system will use the following project inputs:

- `patient_data.csv`: structured patient data.
- `MediCare_UseCase.pdf`: use-case context and reference material.

The CSV is expected to include: `patient_id`, `patient_name`, `age`, `gender`, `diagnosis`, `current_medication`, `lab_test`, `lab_value`, `lab_unit`, `last_visit_date`, `next_scheduled_visit`, `days_since_last_visit`, `missed_last_appointment`, blood-pressure values, heart rate, oxygen saturation, and clinical `notes`.

## 7. Functional Requirements

### FR-1: Data Loading and Exploratory Analysis

- The system must load `patient_data.csv`.
- It must inspect the schema, data types, missing values, duplicate patient IDs, and date-field validity.
- It must profile demographics, diagnoses, medications, laboratory values, vital signs, visit recency, and missed appointments.
- It must produce a concise exploratory-analysis report containing notable patterns and data-quality concerns.

### FR-2: Agent Tools

The agent must have access to structured tools that can:

- Retrieve a patient record by `patient_id`.
- Return a patient summary containing diagnoses, medication, laboratory results, vital signs, visits, appointment status, and notes.
- Retrieve risk-relevant clinical context from a patient record.
- Identify records with a missed last appointment.
- Return a prioritized follow-up dataset with the evidence used for prioritization.

Tool responses must be structured, traceable to source fields, and safe when a patient ID is invalid or data is incomplete.

### FR-3: LLM Agentic Workflow

- The system must use an LLM API that supports tool/function calling.
- The LLM must determine which available tools to call to answer a user request; the orchestration must not rely on hardcoded clinical conclusions.
- The workflow must pass tool results back to the LLM for a final response.
- The agent must be instructed to ground conclusions in retrieved data, identify uncertainty or missing information, and avoid unsupported claims.
- The system must handle invalid requests, empty results, tool failures, and LLM API failures gracefully.

### FR-4: Single-Patient Analysis

Given a valid `patient_id`, the agent must:

1. Retrieve the patient's structured record through tools.
2. Present the relevant diagnoses, medication, laboratory result, vitals, visit history, appointment status, and notes.
3. Produce clinical risk insights that cite the supporting data points.
4. Suggest follow-up considerations for clinician review.
5. Display a disclaimer that the output is clinical decision support and not a substitute for professional judgment.

### FR-5: Missed-Appointment Follow-Up

- The system must identify records where `missed_last_appointment` indicates a missed appointment.
- The agent must generate a follow-up list prioritized using the patient context returned by tools.
- Each list item must include patient identifier, relevant risk context, outreach urgency, suggested action, and rationale.
- The output must be formatted for care-team review; it must not automatically contact or schedule patients.

## 8. Output Requirements

### Single-patient response

- Patient identifier and a concise record summary.
- Evidence table or clearly labeled evidence list.
- Risk insights and uncertainty/missing-data notes.
- Suggested follow-up considerations.
- Clinical-support disclaimer.

### Missed-appointment response

- Prioritized patient list.
- Patient identifier and missed-appointment status.
- Relevant clinical context used by the agent.
- Recommended outreach urgency and follow-up action.
- Human-review note and clinical-support disclaimer.

## 9. Safety and Privacy Requirements

- Patient data must be handled only within authorized project systems.
- Outputs must use the minimum patient information necessary for the requested review.
- The agent must not invent facts, diagnosis, medication changes, or care instructions not supported by retrieved data.
- Recommendations must be framed as considerations for a qualified care team.
- Logs and demonstrations must avoid exposing patient data beyond the project scope.

## 10. Acceptance Criteria

- The dataset loads successfully and an exploratory-analysis report documents schema checks, quality findings, and observations.
- A valid `patient_id` returns the corresponding structured patient data; an unknown ID returns a clear, safe message.
- The agent autonomously calls one or more tools through an LLM API before forming a substantive patient-analysis response.
- Patient analysis references retrieved fields and includes a clinician-review disclaimer.
- Missed-appointment analysis returns only applicable records with a priority, rationale, and recommended action for each.
- Tool and API failures produce actionable error messages without fabricated analysis.

## 11. Success Measures

- All required workflows run end to end against the provided dataset.
- Outputs are traceable to patient-data fields and are understandable to a care-team reviewer.
- A reviewer can identify why each follow-up item was prioritized.
- No response presents LLM output as an autonomous clinical decision.

## 12. Dependencies and Assumptions

- An approved LLM API key and compatible tool-calling model are available at runtime.
- The CSV remains the authoritative source for the prototype.
- Clinical staff review all generated insights and follow-up recommendations before action is taken.

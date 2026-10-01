# MediCare Agent - Tasks

**Project inputs:** `patient_data.csv` and `MediCare_UseCase.pdf`

## Task 1 - Data Loading & Exploratory Analysis

- [ ] Load and inspect `patient_data.csv`.
- [ ] Validate the schema, data types, missing values, duplicate `patient_id` values, and date fields.
- [ ] Profile patient demographics, diagnoses, medications, labs, vital signs, time since last visit, and missed appointments.
- [ ] Identify important observations, data-quality concerns, and risk-related patterns.
- [ ] Document the exploratory-analysis findings.

## Task 2 - Define Agent Tools

- [ ] Create tools/functions to retrieve an individual patient by `patient_id`.
- [ ] Create tools/functions to summarize patient history, clinical measurements, visits, and appointment status.
- [ ] Create tools/functions to identify missed appointments and prioritize patients for follow-up.
- [ ] Create tools/functions the agent can use for patient analysis and risk evaluation.
- [ ] Ensure tool outputs are structured, grounded in the dataset, and clearly distinguish data from recommendations.

## Task 3 - Implement the Agentic Loop

- [ ] Build an LLM-driven agent workflow capable of orchestrating tool calls autonomously.
- [ ] Ensure the decision-making process relies on an LLM API (OpenAI, Anthropic, Grok, etc.).
- [ ] Provide the agent with the available tool definitions and a clinical-safety prompt.
- [ ] Avoid hardcoded clinical decision logic where LLM reasoning is expected.
- [ ] Add handling for invalid patient IDs, missing data, and tool/API failures.

## Task 4 - Single Patient Analysis

- [ ] Demonstrate detailed analysis for an individual patient record selected by `patient_id`.
- [ ] Use the agent's tools to gather the patient's diagnoses, medication, lab result, vitals, visits, and notes.
- [ ] Generate clinical risk insights and follow-up recommendations grounded in the retrieved record.
- [ ] Include an appropriate disclaimer that the output supports, rather than replaces, clinician judgment.

## Task 5 - Missed Appointment Follow-Up

- [ ] Identify patients whose `missed_last_appointment` value indicates a missed appointment.
- [ ] Use the LLM-driven workflow to generate prioritized follow-up actions and recommendations.
- [ ] Include the patient identifier, relevant risk context, recommended outreach urgency, and rationale for each action.
- [ ] Produce a reviewable follow-up list for the care team.

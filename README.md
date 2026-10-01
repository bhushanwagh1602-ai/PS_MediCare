# MediCare Agent

MediCare Agent is a read-only, LLM-powered clinical-support prototype. It loads structured patient data, provides tool-based patient retrieval, analyzes one patient record, and creates a reviewable follow-up list for patients who missed appointments.

The project supports care-team review only. It does not diagnose, prescribe, contact patients, schedule appointments, or replace clinician judgment.

## Features

- Loads and validates `patient_data.csv`.
- Profiles patient demographics, diagnoses, medications, laboratory values, vital signs, visits, and missed appointments.
- Provides tools for patient lookup, patient summaries, risk-context retrieval, and missed-appointment filtering.
- Uses an OpenAI tool-calling workflow for patient analysis and follow-up prioritization.
- Handles unknown patient IDs, missing API credentials, tool failures, and API failures without fabricating clinical conclusions.
- Displays source evidence separately from LLM-generated analysis.

## Project Files

- `MediCare_Agent.ipynb` - Main notebook and executable workflow.
- `patient_data.csv` - Structured input dataset.
- `MediCare_UseCase.pdf` - Use-case reference material.
- `PRD.md` - Product requirements and safety requirements.
- `Task.md` - Project task checklist.

## Requirements

- Python 3.10 or later.
- Jupyter support in VS Code or another Jupyter environment.
- An OpenAI API key for live agent analysis.

Install the Python dependencies:

```powershell
python -m pip install pandas openai
```

## Configuration

The notebook reads configuration from environment variables. Do not place API keys in the notebook or commit them to source control.

For the current PowerShell session:

```powershell
$env:OPENAI_API_KEY = "your_api_key_here"
$env:OPENAI_MODEL = "gpt-5"
```

The repository also ignores `.env` files. The notebook uses `os.getenv`, so setting the environment variable in the VS Code terminal is the most direct option. Restart the notebook kernel after changing environment variables.

To verify configuration without printing the secret:

```python
import os
print(bool(os.getenv("OPENAI_API_KEY")))
```

## Running the Notebook

1. Open `MediCare_Agent.ipynb` in VS Code.
2. Select a Python kernel with the dependencies installed.
3. Run the cells from top to bottom.
4. Run the Task 4 cell for a single-patient review. The example uses `patient_id` `P0001`.
5. Run the Task 5 cell for missed-appointment follow-up analysis.

The notebook can run without an API key. In that mode, it still validates the dataset and prints source evidence, but it does not generate live LLM analysis or prioritization.

## Safety and Privacy

This prototype is for authorized project use and human review. Treat patient data as confidential and use the minimum information necessary. Before publishing this repository, confirm that `patient_data.csv` contains approved synthetic or de-identified data and that the use-case PDF is safe to distribute publicly.

All generated insights and follow-up suggestions must be reviewed by a qualified care team. The output is clinical decision support, not medical advice or an autonomous clinical decision.

## License

No license has been specified for this project yet. Add an appropriate license before accepting external contributions or redistributing the repository.

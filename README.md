**🏦 AI Bank Statement Parser Agent**


This project implements an AI coding agent that automatically generates custom parsers for bank statement PDFs. The agent follows a plan → generate → test → refine loop, writing new parsers that convert PDFs into structured CSVs.


**🚀 Features**


Agentic workflow: plan → code → test → self-fix (≤ 3 attempts).
CLI runnable: python agent.py --target <bank>
Custom parser generation: writes to custom_parsers/<bank>_parser.py
Testing: validates parser output against provided CSV ground truth.
LLM integration: Groq + Gemini for parser generation; fallback to stub parser for robustness.
Self-contained: synthetic ICICI and SBI sample data included.


**🛠️ Project Structure**


agent_as_coder/
├── agent.py                 # Main agent loop
├── custom_parsers/          # Auto-generated parsers live here
│   ├── icici_parser.py
│   └── sbi_parser.py
├── data/                    # Sample PDFs + CSVs
│   ├── icici/
│   │   ├── icici_sample.pdf
│   │   └── icici_sample.csv
│   └── sbi/
│       ├── sbi_sample.pdf
│       └── sbi_sample.csv
├── tests/                   # Test suite
│   ├── test_icici.py
│   ├── test_sbi.py
│   └── test_all_banks.py
└── README.md                # This file


**📦 Installation**


pip install -r requirements.txt
Or, if running in Google Colab: all dependencies are installed automatically in the first cell.


**▶️ Usage**


1. Generate parser for ICICI
python agent.py --target icici
2. Generate parser for SBI
python agent.py --target sbi
3. Run tests
pytest -q


**✅ Parser Contract**


Each generated parser implements:
def parse(pdf_path: str) -> pd.DataFrame:
    """
    Parses a bank statement PDF into a DataFrame.

    Columns: [Date, Description, Ref, Debit, Credit, Balance]
    """
The parser output must equal the provided CSV via:
pd.testing.assert_frame_equal(got, expected, check_dtype=False)


**📊 Agent Architecture**


flowchart TD
    A[Start agent.py] --> B[Plan step: identify bank & task]
    B --> C[Generate parser code in custom_parsers/]
    C --> D[Run pytest on parser vs. CSV]
    D -->|Fail| E[Observe error logs]
    E --> F[Refine parser code (≤3 attempts)]
    D -->|Pass| G[Parser ready ✅]
    F --> C
    G --> H[End]

    
**📋 Deliverables Checklist**


✅ T1: agent.py with loop (plan → generate → test → self-fix).
✅ T2: CLI support --target <bank>.
✅ T3: Parser contract implemented.
✅ T4: Unit tests for parser correctness.
✅ T5: README with run instructions + agent diagram.


**👨‍💻 Author**
Developed as part of the Karbon AI Agent Challenge.

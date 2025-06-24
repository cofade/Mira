# Backend

Welcome to the **Informed Dialogues Backend**!  
This service empowers AI assistants to operate within **human-derived values and guardrails**, ensuring AI behavior is both responsible and aligned with real-world perspectives.

---

## Main Features

- **🤝 Human-in-the-loop AI:**  
  Aligns AI assistants with values and principles extracted from global survey data.

- **🛡️ Dynamic Guardrails:**  
  Injects actionable prompts (guardrails) derived from user responses to guide AI behavior.

- **🔄 Toggleable Guardrails:**  
  Easily switch guardrails on or off to compare value-guided and unconstrained AI behavior.

- **🧩 Modular Design:**  
  Cleanly organized modules for LLM interaction, prompt synthesis, and API serving.

---

## Setup Instructions

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/schutera/informedDialogues.git
cd informedDialogues/backend
```

### 2️⃣ Set Up a Virtual Environment

```bash
python -m venv .venv
# On Windows:
.venv\Scripts\activate
# On macOS/Linux:
source .venv/bin/activate
```

### 3️⃣ Install Dependencies

> **Note:**  
> The `requirements.txt` is in the root directory and covers the whole project.

```bash
pip install -r ../requirements.txt
```

### 4️⃣ Configure Environment Variables

Create a `.env` file or export your OpenAI API key:

```bash
export OPENAI_API_KEY=sk-...
```

---

## Running the Backend

### Start the API Server

```bash
uvicorn api_server:app --reload
```

The API will be available at [http://localhost:8000](http://localhost:8000).

---

## Generating Guardrail Prompts (Optional)

To generate or update the actionable guardrail prompt from survey responses:

```bash
python railyard.py
```

This will create or update `backend/guardrail_prompt.csv`  
(Embeddings and synthesis can be generated using scripts in the `/utils` directory.)

We use the synthesis function by relevance to a given term, topic, phrase, or question – which in our case was:

```python
synth_type = "bullets"      # Synthesize results as bullet points
rank_type = "bridging"      # Ranking responses by bridging agreement across segs
thresh = 0.25               # Lower threshold to 25% agreement across segments for broader inclusion
n_max = 50                  # Only keep the top 50 responses with highest bridging agreement
query_text = "AI guardrails and values"
```

---

## Example API Usage

### 🔍 Request

**POST** `/api/ask`

```json
{
  "prompt": "Your user question here.",
  "guardrails": true
}
```

- If `guardrails` is `true`, the system prompt will include the actionable guardrail prompt.
- If `guardrails` is `false`, the system prompt will instruct the LLM to take a side in A/B decisions.

### 📤 Response

```json
{
  "response": "LLM's answer"
}
```

---

##  File Structure

- `api_server.py`: FastAPI server exposing the `/api/ask` endpoint.
- `llm_interface.py`: Handles LLM calls and system prompt construction.
- `railyard.py`: Generates actionable guardrail prompts from survey data.
- `guardrail_prompt.csv`: Stores actionable prompts for implementing guardrails.

---

## Notes

- Ensure `guardrail_prompt.csv` exists in the backend directory for guardrails to function.
- You can generate necessary files using scripts in the `utils` directory.
- While designed for the **Informed Dialogues Frontend**, the API can be used independently.

# RoleColor AI: Behavioral Persona & Summary Generator

RoleColor AI is a Python-based natural language processing (NLP) tool that classifies professional experience into four "RoleColor" archetypes: **Builder, Enabler, Thriver, and Supportee**. By analyzing the linguistic patterns of a resume, the system identifies a dominant persona and leverages a Large Language Model (LLM) to rewrite a professional summary that highlights key achievements through that specific lens.

## 1. Approach Explanation

The system follows a three-stage pipeline to transform a raw resume into a categorized profile and a polished summary:

### A. Archetype Mapping (Scoring)

The core of the analysis uses a **TF-IDF (Term Frequency-Inverse Document Frequency)** vectorization strategy.

* **The Corpus:** A reference corpus is built using a `KEYWORD_MAP` and `ROLE_DESCRIPTIONS`. Keywords are weighted (repeated 3x) to ensure the engine prioritizes specific professional indicators.
* **Similarity Analysis:** The system calculates the **Cosine Similarity** between the resume text and each of the four RoleColor "centroids." The results are normalized to provide a percentage-based distribution of the candidate’s professional DNA.

### B. Metadata & Achievement Extraction

To ensure the summary is factual and professional, the system uses **spaCy** for Named Entity Recognition (NER) and rule-based extraction:

* **Anonymization:** It identifies and removes personal names (`PERSON` entities) to maintain privacy and focus on professional merit.
* **Title Detection:** Uses Regex to find standard job titles (e.g., Engineer, Manager, Architect).
* **Achievement Scoring:** Sentences are ranked based on their density of "RoleColor" keywords. The top three highest-scoring sentences are selected as the "Top Achievements."

### C. LLM Summary Generation

The final stage utilizes **Google’s FLAN-T5 Large**, a sequence-to-sequence model. It combines the dominant role, the extracted title, and the top achievements into a structured prompt to generate a 4-line professional summary.

---

## 2. How to Run the Code

### Prerequisites

Ensure you have Python 3.8+ installed. You will need a `resume.txt` file in the same directory as the notebook.

### Installation

Install the required libraries via pip:

```bash
pip install spacy torch numpy scikit-learn transformers
python -m spacy download en_core_web_sm

```

### Execution Steps

1. **Place your Resume:** Save your resume content as `resume.txt` in the project root.
2. **Open the Notebook:** Launch `Role_Color_AI.ipynb` in Jupyter or VS Code.
3. **Run All Cells:**
* **Part 1 & 2** will output your RoleColor scores and dominant role.
* **Part 3** will download the FLAN-T5 model (approx. 3GB) and generate your rewritten summary.



---

## 3. Assumptions Made

* **Standard Formatting:** The script assumes the resume title is near the top or follows a standard "Title" naming convention for the regex-based extractor to work effectively.
* **Keyword Representativeness:** It is assumed that the `KEYWORD_MAP` provided (e.g., "innovation" for Builders, "stability" for Supportees) accurately captures the essence of those professional roles.
* **Model Capacity:** The use of `flan-t5-large` assumes the environment has sufficient memory (RAM/VRAM). If running on a CPU-only machine, generation may take 10–30 seconds.
* **Language:** The current implementation is optimized strictly for English-language resumes.

---

### Project Archetypes

| Role | Primary Focus | Key Trait |
| --- | --- | --- |
| **Builder** | Innovation & Strategy | Visionary |
| **Enabler** | Collaboration & Bridging | Facilitator |
| **Thriver** | Agility & Deadlines | Resilient |
| **Supportee** | Reliability & Documentation | Dependable |


Domain & Difficulty-Based Question Prompt Generator
A lightweight Python tool for generating domain- and difficulty-controlled interview-style questions. The project provides multiple prompt templates (base, context-aware, difficulty-control, topic-variation), enforces constraints (no repeated questions within a run, domain fallback, difficulty normalization), and is easily extendable to call an LLM for more creative wording.

Features
Multiple prompt templates:
Base Question
Context-Aware (follow-ups using previous question + candidate answer)
Difficulty-Control (explicit instructions per difficulty)
Topic Variation (avoid repeating subtopics)
Domain normalization and fallback (e.g., unknown domain → DSA)
Difficulty normalization and defaulting (maps varied inputs to easy/medium/hard)
Deduplication within a run, optional persistence across runs (JSON file)
CLI and programmatic API
Unit tests (pytest)
Project Structure
templates.py — Template functions and difficulty normalization
utils.py — Domain/topic definitions, normalization helpers, persistence
generator.py — QuestionGenerator class (core logic)
run.py — CLI wrapper for quick usage
sample_input.json — Example input
tests/test_generator.py — Basic unit tests
output_state.json — (optional) persisted dedup store (auto-created)
requirements.txt, .gitignore
Quick Start
Clone the repo
Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate (Windows: venv\Scripts\activate)
Install dependencies
pip install -r requirements.txt
Usage
CLI example:

Generate 3 medium DSA questions (base template):

python run.py --domain "DSA" --difficulty "medium" --template base --num 3
Generate a context-aware follow-up:

python run.py --template context --domain "DSA" --prev_question "What is a binary tree?" --candidate_answer "A tree with at most two children." --num 1
Programmatic usage:

from generator import QuestionGenerator

gen = QuestionGenerator(persist=False)
items = gen.generate(domain="DSA", difficulty="easy", template_type="base", num=2)
for it in items:
    print(it["question"])
Sample output (representative):

{
  "question": "[dsa] (medium) binary tree: Design or implement a solution for binary tree traversal in a typical interview scenario. Describe approach and complexity.",
  "topic": "binary tree",
  "difficulty": "medium",
  "style": "applied"
}
Templates Overview
Base: Produces a question tailored to the topic and normalized difficulty.
Context-Aware: Adds follow-up context and feedback when a previous question and candidate answer are provided.
Difficulty-Control: Appends explicit instructions appropriate to the difficulty (e.g., conceptual for easy, algorithmic for hard).
Topic Variation: Ensures generated items avoid a provided list of topics to reduce repetition.
Constraints & Edge Cases
No repeated questions within a generator instance; optional persistence prevents repeats across runs.
Invalid or missing domain → fallback to default domain (dsa).
Unrecognized difficulty → defaults to medium.
Maximum attempts limit to avoid infinite loops when requested constraints are too strict.
Testing
Run unit tests with pytest:

pip install pytest
pytest -q
Included tests validate de-duplication behavior and domain fallback handling.

Extending with an LLM
The codebase is designed to be LLM-agnostic. To integrate an LLM (e.g., OpenAI):

Add an llm_client wrapper and call it from templates or generator.
Validate LLM outputs against domain and difficulty constraints.
Keep deterministic fallbacks and retry logic to maintain quality and avoid off-domain outputs.
Contributing
Contributions are welcome. Suggested next steps:

Expand supported domains and topic lists
Add richer tests for content quality and prompt effectiveness
Add an integration test suite for optional LLM outputs
Provide a web UI or REST API wrapper

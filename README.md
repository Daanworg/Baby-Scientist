---

# Project: Baby Scientist

An autonomous AI research and creation system designed to evolve from a state of initial ignorance (a "baby") to a capable synthesizer and creator of complex information (a "scientist").

![Status](https://img.shields.io/badge/status-experimental-orange)
![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

---

## The Vision: From Baby to Scientist

This project is an engineered system that mimics the cognitive journey of a researcher, designed to go beyond simple Q&A and into the realm of learning, strategy, and creation.

*   **The Baby (Learning):** It starts with a question. As it researches, it identifies and ingests high-value information into its persistent, long-term memory (a FAISS vector database). It learns from every task, growing its knowledge base with each run.

*   **The Researcher (Strategizing):** It uses a sophisticated, two-phase planning process. It generates potential research questions, **uses its own internal memory to critique those questions**, and then refines them into highly-targeted, efficient queries. It avoids redundant work and focuses on true knowledge gaps.

*   **The Scientist (Creating):** The system's ultimate purpose. It can be tasked with high-level goals like "elaborate on this architecture." It uses its entire accumulated knowledge base as a blueprint to plan and execute the creation of new, structured artifacts like MermaidJS diagrams, technical protocols, or formal schemas.

This is a blueprint for the future of autonomous AI—not just as tools that answer, but as partners that learn, grow, and create.

## Core Features & Technical Highlights

*   **🧠 Autonomous Multi-Agent System:** A team of specialized AI agents (Planner, Search Analyzer, Creator, Consolidator) collaborate within a robust asynchronous framework.

*   **♟️ Two-Phase RAG-on-RAG Strategic Planner:** The Planner Agent's intelligence is a key differentiator.
    1.  **Phase 1 (Candidate Generation):** It uses its internal knowledge (RAG) to get context on its own knowledge gaps before generating a set of *draft* web search queries.
    2.  **Phase 2 (Self-Critique & Refinement):** For each draft query, it performs another targeted RAG search. It then asks the LLM to review the query *and* its corresponding RAG results, deciding whether to **refine** it for more specificity, **keep** it, or **discard** it as redundant. This is meta-cognition in action.

*   **📈 Dynamic Learning & Self-Improving RAG:** The system features a closed learning loop. High-relevance findings from web searches are automatically processed, vectorized, and ingested back into the global FAISS store via `_process_and_potentially_ingest_web_findings`. The system gets smarter with every research session.

*   **💡 Creator Agent: From Insight to Artifact:** A dedicated two-step creative engine.
    1.  **Planning:** The Creator first deconstructs a high-level task into a detailed JSON plan, identifying the necessary knowledge for each step.
    2.  **Execution:** It then executes the plan step-by-step, using RAG to pull the exact context needed from its knowledge "blueprint" to generate tangible artifacts (e.g., Mermaid diagrams, technical specifications).

*   **🚀 Modern, Model-Executed Tool Use:** Leverages the native tool-use capabilities of Google's Gemini 2.5. Instead of parsing text for commands, it empowers the model to directly execute Google Searches, leading to more reliable and robust information gathering.

*   **🛠️ Pragmatic & Resilient Engineering:**
    *   **JSON Self-Correction:** Includes an LLM-based function (`_attempt_json_self_correction_with_llm`) to automatically fix malformed JSON outputs from the primary models, preventing cascading failures.
    *   **Robust Embedding:** Intelligently handles API rate limits by embedding documents individually with controlled delays, ensuring that large ingestion jobs are resilient to single-point failures.
    *   **Stateful Session Management:** Full support for creating new sessions, loading previous ones to continue work, or wiping the global knowledge base for a fresh start, all managed through an interactive CLI.

*   **💾 Dual-Memory Architecture:** Employs a session-specific "working memory" (`KnowledgeBase`) and a persistent "long-term memory" (`SimpleFaissManager` backed by Google Cloud Storage) for robust state and knowledge management.

## Environment & Usage

This notebook is designed to be run in a **Google Colab Enterprise environment**, as it leverages its native integration with Google Cloud for authentication and direct access to Vertex AI services.

### Prerequisites

1.  Access to a **Google Colab Enterprise** runtime connected to a Google Cloud Project.
2.  The **Vertex AI API** must be enabled in your connected Google Cloud Project.
3.  A **Google Cloud Storage (GCS) bucket** must be created for storing the persistent RAG database.

### Running in Colab Enterprise

1.  **Configure Constants:** Open the notebook and update the configuration constants at the top of the first code cell:
    *   `VERTEX_PROJECT_ID`: Your Google Cloud Project ID.
    *   `VERTEX_LOCATION`: The region for your project (e.g., "us-central1").
    *   `FAISS_INDEX_GCS_BUCKET_NAME`: The name of the GCS bucket you created.

2.  **Execute the Code:**
    *   Run the first cell to install any necessary packages (like `faiss-cpu`) and define all classes and functions.
    *   Run the final cell to start the interactive session manager.

3.  **Interact with the System:** Follow the prompts in the output to:
    *   Choose `[N]` for a new session, `[L]` to load a previous one, or `[F]` for a completely fresh start.
    *   Enter your research query or elaboration task.
    *   Optionally, ingest local documents into the global RAG store before the run begins.

### Note for Local or Standard Colab Environments

While not the primary target, you can adapt this code for other environments with the following considerations:
*   **Authentication:** You **must** authenticate your environment with Google Cloud. The simplest way is to install the `gcloud` CLI and run:
    ```bash
    gcloud auth application-default login
    ```
*   **Dependencies:** You will need to install all required packages locally. You can create a `requirements.txt` file for this purpose.

## Project Status & Future Work

This project is currently **experimental**. It serves as a powerful proof-of-concept for self-improving, creative AI systems.

Future enhancements could include:
*   Refactoring the code into a modular, multi-file Python library.
*   Building a web-based user interface (e.g., with Streamlit or Gradio).
*   Expanding the Creator Agent's capabilities to generate other artifacts (e.g., code, presentations).
*   Implementing more sophisticated meta-cognitive and self-correction routines.

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page or start a discussion.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

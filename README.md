**Project README**
This project implements a multi-agent system designed for market and company research, specifically focusing on the Indian automotive industry. It processes PDF documents, extracts key information, ingests market and company-specific data, and then uses a multi-agent framework to synthesize reports and prioritize companies.

**Project Phases**
The project is structured into several distinct phases:

**Phase 1: PDF Document Extraction**
This phase focuses on extracting raw content from PDF documents. It identifies section headings, tables, and text, and performs initial diagnostics to determine if pages are likely scanned or contain heavy table content. The output includes a detailed extraction report and structured page data.

**Phase 2: Entity Extraction**
Building upon Phase 1, this phase extracts specific entities from the cleaned PDF text. It uses rule-based patterns and keyword matching to identify services, pricing, timelines, delivery approaches, SLA terms, exclusions, assumptions, and engagement models. These structured entities are crucial for generating targeted answers in later phases.

**Phase 3: Chunking**
This phase segments the extracted content into smaller, meaningful chunks suitable for indexing in a vector database. It assigns metadata to each chunk, such as section, subsection, and entity tags, enabling efficient retrieval. Both text and table content are processed into chunks.

**Phase 4: ChromaDB Indexing and Retrieval**
In this phase, the generated chunks are indexed into a ChromaDB vector database. It uses a Sentence Transformer embedding function for semantic search. This phase also includes a metadata-first retrieval mechanism that allows for precise querying based on entity filters and confidence scores.

**Phase 5: Answer Generation**
This phase assembles citation-backed answers to specific queries. It utilizes the retrieved chunks and extracted entities to construct deterministic responses based on predefined templates. Optionally, it can use the OpenAI API to polish the wording of the answers while ensuring they remain grounded in the retrieved evidence.

**Market Ingestion**
This pipeline collects market intelligence related to the Indian automotive industry. It gathers web records from financial, industry, and news sources using DuckDuckGo Search, and market signals (e.g., stock performance) from Yahoo Finance. This data is then processed and indexed into a separate ChromaDB collection.

**Company Ingestion**
This pipeline focuses on collecting company-specific data. It retrieves financial signals from Yahoo Finance and recent news snippets from DuckDuckGo Search for companies within the Indian automotive sector. This data is then transformed into chunks and indexed into a ChromaDB collection dedicated to company profiles.

**Multi-Agent Market Research**
This module orchestrates a multi-agent market research process using the Agno framework. It comprises:

**Research Agent: **Identifies candidate organizations and opportunities from market evidence.
**Validation Agent:** Assesses the sufficiency, freshness, and confidence of the evidence.
**Report Agent:** Generates a concise markdown report summarizing the findings, including industry trends, target organizations, and consultancy fit rationale.
**Company Profiling and Prioritization**
This phase builds detailed profiles for candidate companies and then prioritizes them based on various criteria. It leverages the data from both market and company ingestion. The prioritization process involves:

Company Research Orchestrator: Collects market and company-specific evidence to build comprehensive company profiles.
Company Prioritization Orchestrator: Scores and ranks companies based on service alignment, urgency, execution readiness, evidence depth, and solution match. It generates explanations for the prioritization decisions and outputs a markdown report of the top-ranked companies.
**How to Use**
To run this project, execute the cells sequentially in the provided notebook. Ensure that:

Dependencies are Installed: The first cell handles the installation of all necessary Python libraries. It also includes steps to handle potential dependency conflicts by uninstalling and reinstalling packages in a specific order.
API Keys are Configured: If you plan to use OpenAI for answer synthesis, make sure your OPENAI_API_KEY is securely stored in Google Colab's secrets manager (named OPENAI_API_KEY) and retrieved by the notebook.
Upload PDF Document: Upload your PDF file as prompted by the notebook. This PDF will be processed through the extraction and chunking phases.
Review Outputs: All generated reports, extracted data, chunks, and research artifacts are stored in the /content/extraction_output/ directory.
Each phase has dedicated cells for execution, and intermediate outputs are saved to allow for review and debugging. The final outputs include detailed JSON reports and human-readable markdown summaries.

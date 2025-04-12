# CueCode Prototype Overview

**Author:** Freddie Boateng  
**Course:** CS411W  
**Date:** April 10, 2025

---

## 1. Purpose

This document provides developers with a technically oriented and comprehensive description of CueCode’s prototype requirements. It continues the product description from CS411W [1], where CueCode was introduced from a non-technical perspective.

This document outlines the technical foundation, architecture, and scope of the prototype. It focuses on the prototype’s goals, which contrast with the full-featured real-world product (RWP) that was previously explored in CS410 by Team Red.

---

## 2. Scope

The prototype will extend core ideas introduced in Zafin’s cosine similarity and LLM-based API payload generator [2]. CueCode builds on this by introducing a **payload ordering algorithm** that ensures correct sequencing of generated API requests — critical for respecting data dependencies between calls.

While the prototype will not fully implement all features expected of the RWP, it will focus on:

- Translating natural language to ordered API payloads (based on an OpenAPI spec)
- Processing user intent with cosine similarity
- Supporting modular runtime and configuration workflows
- Integrating human review into the payload flow for safety

> ⚠️ Note: This version of CueCode does not yet support automatic introspection of APIs to identify field names or object relationships. That feature is reserved for the RWP.

---

## 3. Definitions, Acronyms, and Abbreviations

| Term | Definition |
|------|------------|
| **API Payload (informal)** | JSON or XML-formatted data sent in an API request/response |
| **CueCode Developer Portal** | Web-based interface to configure APIs and manage NLP settings |
| **HTTP Header** | Metadata such as content type, authorization, etc. |
| **HTTP** | Protocol used for data communication on the web |
| **Hypermedia** | Content on the web linked through RESTful services |
| **OpenAPI Specification** | Machine-readable description of an API’s endpoints |
| **REST** | Architectural style for building stateless web APIs |
| **RWP** | Real-World Product – full version of CueCode (non-prototype) |
| **URL** | A web address specifying location of a resource |

---

## 4. Product Perspective

CueCode offers developers a structured framework to convert **natural language** into **REST API payloads** using OpenAPI configurations. The system is composed of five modular components:

1. **Common** (shared utilities)
2. **Web Framework**
3. **Configuration Algorithm**
4. **Runtime Algorithm**
5. **Worker Process (Actors)**

### System Modes

CueCode runs in two distinct modes:
- **Web Server Mode** – for processing API requests at runtime
- **Worker Mode** – used during configuration phase (powered by [Dramatiq](https://dramatiq.io)) [8]

### Workflow Overview

At runtime, developers call CueCode via HTTP APIs. The service interprets natural language, generates a valid sequence of REST payloads, and returns them to the client system. The calling application can then:
- Apply **business rules**
- Let users **review** payloads
- Dispatch the payloads in order

This ensures human or automated oversight of LLM-generated outputs — an increasingly essential design requirement in production LLM applications [2], [6].

---

## 5. Functional Architecture

### CueCode Developer Portal (Prototype)

In the prototype, the Developer Portal:
- Accepts an OpenAPI spec
- Triggers CueCode’s **Configuration Algorithm**
- Prepares runtime modules for payload generation

> 📌 This portal is a simplified version of what will exist in the RWP, where full integration dashboards, analytics, and multiple API sets will be supported.

---

## 6. Runtime Process

The prototype system will:
- Accept **natural language input**
- Generate **REST API payloads** in the correct sequence
- Return those payloads via an HTTP API
- Allow the calling app to process and dispatch the payloads

Figure 3 (omitted here) describes the interaction between runtime modules and the API authentication system. CueCode’s architecture allows for safe and scalable handling of user input while supporting human-in-the-loop validation.

---

## 7. System Modules

| Module | Description |
|--------|-------------|
| **Web Server** | Receives API requests and responds with payloads |
| **Worker Mode (Actors)** | Handles configuration-time processing |
| **Redis/Postgres** | Stores all application state (app is stateless otherwise) |
| **Client Libraries** | Enables integration in multiple programming languages |

---

## 8. Prototype vs. Real-World Product (RWP)

| Feature | Prototype | Real-World Product (RWP) |
|--------|------------|---------------------------|
| Translate natural language to payloads | ✅ | ✅ |
| Payload ordering for dependencies | ✅ | ✅ |
| Accept OpenAPI spec | ✅ | ✅ |
| Introspect target API to detect field names | ❌ | ✅ |
| Full Developer Portal | Partial | Full dashboard |
| Client library support | ✅ | ✅ |
| Human review & business rules support | ✅ | ✅ |
| Generate UI for API | ❌ | ✅ |
| Search-based entity mapping | ❌ | ✅ |

---

## 9. Summary

CueCode's prototype is designed to prove the feasibility of a safe, LLM-powered interface for API automation. It includes features for natural language interpretation, dependency-aware API payload generation, and modular system design for configuration and runtime logic.

While limited in scope compared to the real-world product (RWP), this prototype will serve as a foundation for future development, incorporating feedback from early testing and technical evaluation.

---

## 10. References

1. CS411W Team Red (2025). CueCode Non-Technical Product Overview.
2. Lorica, B. (2024). *Expanding AI Horizons: The Rise of Function Calling in LLMs*. [Gradient Flow](https://gradientflow.com/expanding-ai-horizons-the-rise-of-function-calling-in-llms/)
2. Mark Needham. (2023). *Returning consistent/valid JSON with OpenAI/GPT* [YouTube](https://www.youtube.com/watch?v=lJJkBaO15Po)
3. Merritt, R. (2023). *What Is Retrieval-Augmented Generation aka RAG?* [NVIDIA Blog](https://blogs.nvidia.com/blog/what-is-retrieval-augmented-generation/)
4. Microsoft. (2024). *prompt-engine*. [GitHub](https://github.com/microsoft/prompt-engine)
5. Prabhakaran, S. (2018). *Cosine Similarity - Understanding the math and how it works?* [Machine Learning Plus](https://www.machinelearningplus.com/nlp/cosine-similarity/)
6. Rao, P. (2024). *Turbo-charge your spaCy NLP pipeline*. [Medium](https://towardsdatascience.com/turbo-charge-your-spacy-nlp-pipeline-551435b664ad)
7. Wei, J. et al. (2023). *Chain-of-Thought Prompting*. [arXiv](http://arxiv.org/abs/2201.11903)
8. IBM. (2021). *What is NLP?* [IBM](https://www.ibm.com/topics/natural-language-processing)
9. Microsoft Docs. (2024). *Why Visual Studio Code?* [VS Code Docs](https://code.visualstudio.com/docs/editor/whyvscode)
   Zafin, R. (2024). *Cosine Similarity Search for NLP-based API Request Generation*.  
4. Fielding, R. (2000). *Architectural Styles and the Design of Network-based Software Architectures*.  
5. OpenAPI Initiative. (2023). *OpenAPI Specification v3.1.0*.  
6. Roy Fielding. *REST APIs Explained*.  
7. Microsoft. (2024). *prompt-engine Repository*. GitHub.  
8. Merritt, R. (2023). *What Is Retrieval-Augmented Generation aka RAG?* NVIDIA Blog.  
9. Dramatiq Documentation. [https://dramatiq.io](https://dramatiq.io)  
9-15. *[Additional citations available in References.md]*

---


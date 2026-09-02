# PicoBrowser 🛡️🤖: Architecting a Privacy-Preserving AI Web Automation Extension
PicoBrowser represents a technological advancement in client-side AI web automation, operating as an open-source Google Chrome Extension compliant with Manifest V3 standards. Built upon the foundational multi-agent framework of NanoBrowser, PicoBrowser addresses the critical privacy vulnerabilities inherent in cloud-based web automation by establishing an on-device data sanitization layer. By integrating client-side machine learning via `transformers.js`, ONNX Runtime Web, and WebGPU hardware acceleration, the extension executes real-time Personally Identifiable Information (PII) detection and redaction using the local `openai/privacy-filter` token-classification model prior to transmitting contextual payloads to remote Large Language Models (LLMs).
## Project Overview & Architectural Vision 🌐
The rapid emergence of AI-driven web agents has transformed automated browser interaction, enabling autonomous web scraping, form submission, and complex navigational workflows. However, conventional agent architectures require streaming active Document Object Model (DOM) trees, interactive input values, and screen captures directly to external cloud inference APIs. This workflow introduces severe security risks when web pages contain sensitive data such as full names, email addresses, phone numbers, payment credentials, or authentication tokens.
PicoBrowser resolves this fundamental conflict between automation capability and data privacy by establishing a localized security boundary within the client runtime. Operating directly within the user's browser, PicoBrowser intercepts all contextual data captured by the agent system. Before any context is exposed to an external network endpoint, a locally hosted neural network evaluates the text stream and replaces sensitive identifiers with ephemeral surrogate tokens. External LLMs operate exclusively on sanitized structural representations. When the cloud-hosted reasoning engine emits navigational or text-entry actions directed at these surrogate tokens, PicoBrowser dynamically re-hydrates the original sensitive values from a secure, local key-value dictionary immediately prior to DOM execution.
## Core Technical Innovations & Features ✨
The system architecture of PicoBrowser incorporates several technical mechanisms designed to achieve high-throughput local inference and deterministic browser control:
- 🛡️ **Client-Side PII Masking & Local Unredaction**: Executes bidirectional token redaction directly within the extension's execution context using the `openai/privacy-filter` model. PII entities are replaced with deterministic placeholder tokens, maintaining structural context for the remote LLM while completely insulating sensitive user payload data from third-party observation.
- ⚡ **Hardware-Accelerated In-Browser Inference**: Utilizes `transformers.js` bound to ONNX Runtime Web with WebGPU execution providers. This pipeline enables low-latency neural network inference on modern GPUs directly inside the browser environment, bypassing server-side compute dependencies.
- 🤖 **Hierarchical Multi-Agent Orchestration**: Inherits and refines the multi-agent orchestration architecture of NanoBrowser, splitting operational workloads between a Planner LLM and a Navigator LLM. The Planner breaks down high-level user commands into discrete sub-goals, while the Navigator translates sub-goals into precise DOM element interactions.
- 🔒 **Comprehensive Input & Output Validation**: Incorporates multi-tier schema and semantic validation layers across all model interactions. Inputs and outputs from both local neural networks and external LLMs undergo runtime validation to prevent prompt injection attacks, schema corruption, and erratic browser actions.
- 🔌 **Provider-Agnostic Model Connectivity**: Supports arbitrary LLM integration, enabling users to pair client-side privacy filtering with cloud backends (such as OpenAI, Anthropic, or Google Gemini) or locally hosted endpoints (via Ollama, vLLM, or custom OpenAI-compatible interfaces).
- 📦 **Modern Extension Stack**: Built on Chrome Manifest V3 using TypeScript, React, and Vite bundling to deliver a modular extension architecture with responsive side-panel user interfaces.
## Deep Dive: On-Device PII Redaction & Local Machine Learning Engine 🔒
At the core of PicoBrowser's security architecture is an isolated **On-Device Data Sanitization Pipeline** that eliminates cloud egress of sensitive information.
### Core Privacy Keywords
`Client-Side Inference` | `transformers.js` | `ONNX Runtime Web` | `WebGPU Acceleration` | `openai/privacy-filter` | `Token Classification` | `Named Entity Recognition (NER)` | `PII Redaction` | `Ephemeral Vault` | `Re-hydration`
### Detailed Redaction Mechanics & Workflow
1. **Context Interception & Tokenization**:
2. **On-Device Token Classification (`openai/privacy-filter`)**:
3. **Ephemeral Key-Value Mapping Vault**:
4. **Zero-Knowledge LLM Reasoning**:
5. **Local Re-hydration & Action Execution**:
## Technical Stack & Infrastructure Specifications 🛠️
| System Layer | Technology / Library | Functional Responsibility |
| --- | --- | --- |
| **Extension Framework** | Chrome Manifest V3, SidePanel API | Host integration, persistent side-panel rendering, and DOM manipulation. |
| **Frontend UI Core** | React 18, TypeScript, Vite | Interactive user controls, real-time agent status streaming, and settings management. |
| **Local Machine Learning** | `transformers.js`, ONNX Runtime Web | Client-side model loading, tensor processing, and token classification execution. |
| **Hardware Acceleration** | WebGPU / WebAssembly (WASM) | GPU-accelerated tensor operations within the browser runtime. |
| **Local Privacy Model** | `openai/privacy-filter`[cite: ] | Token-level Named Entity Recognition (NER) for identifying PII boundaries. |
| **Base Extension Core** | NanoBrowser Architecture | Multi-agent coordination, page state extraction, and action execution engines. |
| **Build & Tooling** | `pnpm`, Vite Bundler | Module resolution, WebAssembly/ONNX asset management, and extension packaging. |
## End-to-End PII Redaction & Multi-Agent Workflow 🔄
The execution lifecycle of a user request within PicoBrowser follows a dual-loop pattern that decouples general cognitive reasoning from sensitive data management:
### 1. User Command & Context Extraction
The execution flow begins when the user submits a natural language command through the React-based side-panel interface. The extension captures the active tab's accessibility tree, DOM layout, and relevant context state.
### 2. Local PII Detection & Token Masking
The extracted text and user instructions are routed to the local background thread running `transformers.js`. The `openai/privacy-filter` model processes the input text as a stream of tokens, identifying entity boundaries corresponding to personal names, email addresses, phone numbers, locations, and financial data. Each detected PII sequence is replaced with a standard identifier token (for example, replacing `John Doe` with `<NAME_1>` and `john@example.com` with `<EMAIL_1>`).
### 3. Ephemeral Key-Value Mapping
Simultaneously, PicoBrowser writes the mapping pair (`<TOKEN_ID> ↔ Original_PII`) to an in-memory storage dictionary managed within the extension's local state. This key-value vault is isolated from external network interfaces and remains scoped to the active task execution context.
### 4. Sanitized Context Transmission & Multi-Agent Planning
The scrubbed prompt and anonymized DOM context are forwarded to the external LLM pipeline. The multi-agent workflow operates in two phases:
- **The Planner LLM**: Reads the anonymized task objective, evaluates the high-level page structure, and constructs a structured plan consisting of logical steps.
- **The Navigator LLM**: Receives individual plan steps alongside specific page element selectors to output concrete browser interactions (such as mouse clicks, keyboard entry, or page scrolling).
### 5. Interception & Local Unredaction
When the Navigator LLM returns an action payload containing token placeholders (such as `TYPE "<EMAIL_1>" INTO "#input-email"`), PicoBrowser's local execution layer intercepts the command before it reaches the target web page. The extension queries the local key-value store, replaces `<EMAIL_1>` with `john@example.com`, and resolves the action payload to its functional state.
### 6. Multi-Tier Validation & Action Execution
Before performing the unredacted DOM action, PicoBrowser passes the command through an internal validation system. This module verifies that the target DOM element exists, validates input parameters against expected structural schemas, and checks for malicious command anomalies. Upon successful validation, the extension executes the action directly within the tab context.
## Validation & Reliability Framework 🔒
To ensure operational stability and prevent erratic behavioral loops, PicoBrowser enforces strict validation checks across both local and remote model interfaces:
| Target Pipeline | Validation Type | Enforcement Mechanism |
| --- | --- | --- |
| **Local Model Inputs** | Pre-Inference Sanitization | Context truncation, UTF-8 normalization, and memory buffer bounds checking. |
| **Local Model Outputs** | Token Boundaries & Entity Matching | Probability thresholding on NER classification tensors to prevent false-positive masking. |
| **LLM Agent Prompts** | Input Schema Verification | Structured JSON formatting enforcement and systemic system prompt guardrails. |
| **LLM Agent Actions** | Output Action Validation | Abstract Syntax Tree (AST) validation of action payloads to prevent injection attacks. |
| **DOM Execution Layer** | Selector & Mutation Inspection | Verification of target DOM node visibility, interaction state, and post-action layout diffing. |
## Installation, Build, & Deployment Guide 🚀
### System Prerequisites
Executing and compiling PicoBrowser requires a modern Node.js development runtime equipped with WebGPU-compatible web browsers.
- **Node.js**: Version `v22.12.0` or higher
- **pnpm**: Version `v9.15.1` or higher
- **Browser**: Google Chrome (v113+ for stable WebGPU) or Microsoft Edge
### Building from Source
Upon successful compilation, Vite packages the extension assets, WASM binaries, and manifest declarations into the `dist/` output directory.
### Step-by-Step: Adding PicoBrowser in Chrome Developer Mode 🔌
Follow these detailed steps to side-load and enable the compiled PicoBrowser extension in Google Chrome:
1. **Open Extensions Manager**: Launch Google Chrome and type `chrome://extensions/` into the URL address bar, then press `Enter`.
2. **Enable Developer Mode**: In the top-right corner of the Extensions page, locate the **Developer mode** toggle switch and turn it **ON**.
3. **Click Load Unpacked**: In the upper-left action bar that appears, click the **Load unpacked** button.
4. **Select Built Directory**: In the file browser dialog, navigate to your cloned `picobrowser` repository folder and select the generated **`dist/`** directory, then click **Select Folder**.
5. **Verify Extension Loading**: PicoBrowser will now appear on your list of installed extensions as an active extension card showing its version and Manifest V3 details.
6. **Pin & Access Side Panel**: Click the extension puzzle icon 🧩 in the Chrome top toolbar, find **PicoBrowser**, click the pin icon to pin it, and click the PicoBrowser icon to launch the side panel interface.
7. **Reloading Changes (Optional)**: If you update code or pull new commits, return to `chrome://extensions/` and click the **Reload / Refresh icon** 🔄 on the PicoBrowser card to apply changes.
### Development Hot-Reloading Mode
For active codebase modification and real-time extension debugging, launch the Vite development watcher:
## Model Configuration & Allocation Strategies ⚙️
PicoBrowser supports multi-provider model assignment, allowing developers to route different sub-tasks to specialized models based on reasoning requirements and latency parameters.
| Deployment Strategy | Planner LLM | Navigator LLM | Local PII Engine | Target Operational Profile |
| --- | --- | --- | --- | --- |
| **High Accuracy Setup** | Claude 3.5 Sonnet | Claude 3.5 Haiku / GPT-4o | `openai/privacy-filter` (WebGPU) | Enterprise workflows involving complex multi-page reasoning. |
| **Cost-Optimized Setup** | GPT-4o-mini / Gemini Flash | Gemini 2.5 Flash | `openai/privacy-filter` (WebGPU) | Routine web data extraction, form filling, and price tracking. |
| **Fully Local Setup** | Ollama (Llama 3.3 70B) | Ollama (Qwen 2.5 Coder) | `openai/privacy-filter` (WASM / WebGPU) | Air-gapped compliance environments requiring zero cloud egress. |
## Security Architecture & Threat Mitigation 🛡️
The architectural design of PicoBrowser incorporates specific countermeasures to mitigate vulnerabilities associated with web-based agent execution:
### 1. Indirect Prompt Injection Defense
Web automation agents frequently interact with untrusted third-party DOM contents that may embed malicious instructions intended to hijack the LLM planner. PicoBrowser counters this vector by requiring the Output Validator module to cross-examine Navigator actions against the isolated high-level plan issued by the Planner LLM, rejecting actions that deviate from expected navigational trajectories.
### 2. Ephemeral Storage Lifecycle
The key-value mapping store responsible for holding original PII strings exists strictly within volatile JavaScript heap memory managed by the extension service worker. Sensitive values are never written to disk, persistent browser caches, or unencrypted sync storage (`chrome.storage.sync`), ensuring complete data destruction upon task completion or service worker termination.
### 3. Client Compute Isolation
By compiling the local PII model to ONNX format and executing it via `transformers.js` inside sandboxed Web Workers, PicoBrowser isolates machine learning inference from the primary browser rendering engine. This prevents memory contention and ensures that high-throughput tensor calculations do not degrade user interface responsiveness.
## Strategic Future Outlook 🔮
The evolution of PicoBrowser targets deeper integration of client-side machine learning directly within browser extension runtimes. Planned enhancements include the implementation of local quantized visual models (such as WebGPU-accelerated vision-language models) to allow local redaction of sensitive image regions and canvas elements prior to frame streaming. Additionally, ongoing optimization efforts focus on reducing model cold-start latencies, expanding token-classification coverage across multilingual PII datasets, and establishing standardized evaluation benchmarks for privacy-preserving web automation agents.
## Community & Open Source Attribution 🤝
PicoBrowser builds upon foundational open-source engineering achievements across the browser automation and local AI landscapes:
- **NanoBrowser**: The base open-source Chrome extension architecture for multi-agent browser control.
- **Browser-Use**: Underlying concepts for browser DOM processing and agent automation workflows.
- **Hugging Face**: Developers of `transformers.js` enabling native in-browser neural network execution.
- **OpenAI**: Creators of the open-weight `openai/privacy-filter` PII identification model.
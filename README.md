Here is the complete `README.md` file contents for your project. You can copy the block below and save it directly as `README.md` in your repository root directory:

# PicoBrowser 🛡️🤖

> **Privacy-Preserving AI Web Automation Chrome Extension**

PicoBrowser is an open-source Google Chrome Extension (built on Manifest V3) that combines client-side AI data sanitization with multi-agent web automation. Built on top of the **NanoBrowser** framework, PicoBrowser redacts Personally Identifiable Information (PII) on-device *before* sending context to Large Language Models (LLMs), ensuring your private data never leaves your browser.

---

## 🌟 Key Features

* **🛡️ Local PII Redaction & Unredaction**: Executes client-side token classification using the `openai/privacy-filter` model to detect and mask sensitive identifiers (names, emails, phones, credentials) before cloud transmission.
* **⚡ WebGPU & ONNX Acceleration**: Powered by `transformers.js` and ONNX Runtime Web for ultra-fast, local machine learning inference directly inside the browser.
* **🤖 Multi-Agent Orchestration**: Features a dual-agent architecture separating task strategy (**Planner LLM**) from browser action execution (**Navigator LLM**).
* **🔒 Ephemeral Unredaction Vault**: Automatically replaces anonymized placeholder tokens back to original values strictly at the execution boundary when performing DOM actions.
* **🛡️ Robust Input/Output Validation**: Built-in runtime schema and integrity validation across all local models and external LLMs to prevent injection attacks and invalid actions.
* **🔌 Flexible Model Support**: BYO API keys for OpenAI, Anthropic, Gemini, or connect to local LLMs via Ollama.

---

## 🛠️ Tech Stack

| Component | Technology |
| --- | --- |
| **Extension Framework** | Chrome Manifest V3, SidePanel API |
| **Frontend UI** | React 18, TypeScript, Vite |
| **Local ML Engine** | `transformers.js`, ONNX Runtime Web |
| **Hardware Acceleration** | WebGPU / WebAssembly (WASM) |
| **PII Detection Model** | `openai/privacy-filter` |
| **Agent Foundation** | NanoBrowser Multi-Agent Engine |
| **Package Manager** | `pnpm` |

---

## 🔄 How It Works

1. **User Request & DOM Extraction**: The side-panel UI captures user instructions and active tab context (DOM layout/accessibility tree).
2. **Local PII Redaction**: The background `transformers.js` pipeline runs `openai/privacy-filter` via WebGPU to strip sensitive tokens (e.g., replacing `john@example.com` with `<EMAIL_1>`).
3. **Key-Value Vault Mapping**: The original values are safely stored in an in-memory ephemeral dictionary within the local extension runtime.
4. **LLM Multi-Agent Execution**:
* **Planner LLM**: Decomposes the user prompt into high-level sub-goals.
* **Navigator LLM**: Translates sub-goals into concrete DOM actions.


5. **Local Unredaction**: PicoBrowser intercepts the action payload containing `<EMAIL_1>`, looks up `john@example.com` in the local vault, and restores the original value.
6. **Validated Action Execution**: The input/output validator checks the payload safety and performs the action on the webpage.

---

## 🚀 Getting Started

### Prerequisites

* **Node.js**: `v22.12.0` or higher
* **pnpm**: `v9.15.1` or higher
* **Browser**: Google Chrome (v113+ recommended for WebGPU support) or Microsoft Edge

### 1. Build from Sourcebash

# Clone the repository

git clone [https://github.com/rathod-tirth/picobrowser.git](https://github.com/rathod-tirth/picobrowser.git)

# Navigate to project directory

cd picobrowser

# Install dependencies

pnpm install

# Build the extension

pnpm build

```

This compiles the production assets into the `dist/` directory.

---

### 2. Add PicoBrowser to Chrome (Developer Mode) 🔌

Follow these steps to load the compiled extension into Google Chrome:

1. Open Google Chrome and navigate to `chrome://extensions/` in the address bar.
2. In the top-right corner, turn on **Developer mode** using the toggle switch.
3. Click the **Load unpacked** button in the upper-left action bar.
4. In the file picker dialog, select the **`dist/`** folder inside your cloned `picobrowser` directory and click **Select Folder**.
5. PicoBrowser will now appear on your extensions page.
6. Click the extension puzzle icon (🧩) in the Chrome toolbar, locate **PicoBrowser**, and pin it.
7. Click the PicoBrowser icon to open the interactive side panel interface.

> 💡 **Development Tip**: If you are actively modifying code, run `pnpm dev` for hot-reloading. To reflect changes in Chrome, go back to `chrome://extensions/` and click the **Reload** (🔄) icon on the PicoBrowser card.

---

## ⚙️ Recommended Agent Configurations

You can assign different LLMs to the Planner and Navigator roles in the extension settings:

| Strategy | Planner LLM | Navigator LLM | Local PII Engine |
| :--- | :--- | :--- | :--- |
| **High Accuracy** | Claude 3.5 Sonnet | Claude 3.5 Haiku / GPT-4o | `openai/privacy-filter` (WebGPU) |
| **Cost-Optimized** | GPT-4o-mini | Gemini 2.5 Flash | `openai/privacy-filter` (WebGPU) |
| **Fully Local** | Ollama (Llama 3.3 70B) | Ollama (Qwen 2.5 Coder) | `openai/privacy-filter` (WASM/WebGPU) |

---

## 📸 Screenshots & Demos

*(Screenshots coming soon)*

| Side Panel Interface | Local PII Debug Logs | Settings & Model Setup |
| :---: | :---: | :---: |
| ![Side Panel UI]() | ![PII Redaction Log]() | ![Settings Panel]() |

---

## 🔒 Security & Privacy Guarantees

- **Zero Cloud PII Egress**: All entity identification and token masking occurs on your device before network requests are dispatched.
- **In-Memory Vault**: Redaction mapping pairs exist only in volatile JavaScript memory and are purged automatically when the task finishes.
- **Strict Validation**: Both input prompts and LLM-generated actions are validated against rigid structural schemas to prevent prompt injection and unauthorized DOM manipulation.

---

## 🤝 Acknowledgments & Credits

PicoBrowser is built on top of amazing open-source technologies:

- **[NanoBrowser](https://github.com/nanobrowser/nanobrowser)** - Base Chrome extension architecture for multi-agent web automation.
- **[Hugging Face transformers.js](https://github.com/huggingface/transformers.js)** - In-browser ML model execution environment.
- **[OpenAI Privacy Filter](https://huggingface.co/openai/privacy-filter)** - Open-weight PII token classification model.
- **[Browser-Use](https://github.com/browser-use/browser-use)** - Automation inspiration and DOM processing concepts.

---

## 📄 License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

```
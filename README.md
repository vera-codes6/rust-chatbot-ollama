# rust-ai-chatbot-ollama

A tiny CLI chatbot written in Rust that talks to a locally running LLM via the Ollama HTTP API.

It streams responses token-by-token using Ollama's `/api/chat` endpoint and maintains a running conversation in-memory.

## Prerequisites

- Rust toolchain (stable). Install via https://rustup.rs/
- Ollama installed and running locally: https://ollama.com/

## Quick start (Windows PowerShell)

1) Install and start Ollama

- Pull a chat-capable model (the code defaults to `llama2`):
	```powershell
	ollama pull llama2
	```
- Start the Ollama server (keeps running on port 11434):
	```powershell
	ollama serve
	```

	

2) Run the chatbot in a new terminal

```powershell
cargo run
```

Type your prompt at the `>` prompt and watch the streamed reply. Press Ctrl+C to exit.

## How it works

- The app sends your conversation history to `POST http://127.0.0.1:11434/api/chat` with:
	- `model`: `"llama2"` (hard-coded)
	- `messages`: alternating `user` and `assistant` messages maintained in-memory
- It reads the HTTP response in chunks and prints `message.content` as it streams until `done: true`.

Key deps (see `Cargo.toml`):
- `reqwest` for HTTP
- `tokio` for async runtime
- `serde`/`serde_json` for (de)serialization

## Changing the model

The default model is `llama2`. To use another model (e.g., `llama3` or `mistral`):
- Pull it with Ollama: `ollama pull llama3`
- Update the `model` field in `src/main.rs` to the desired model name
- Rebuild and run: `cargo run`

## Troubleshooting

- Connection error to `127.0.0.1:11434`:
	- Ensure the Ollama server is running: `ollama serve`
	- Verify the model is pulled: `ollama list`
- Model not found:
	- Pull it first, e.g.: `ollama pull llama2`
- No output or garbled output:
	- Make sure you're using a chat-capable model in Ollama.

## License

This project is licensed under the terms of the license in `LICENSE`.

# ix-net

HTTPS MITM proxy for Claude Code API traffic capture.

Intercepts `api.anthropic.com` traffic, extracts model, token usage, prompt text, and base64 images from request/response payloads.

## Building

Requires LDC, dub and OpenSSL at `/usr/local/opt/openssl` (dub.json names it). `make build` builds `bin/qntx-ix-net-plugin`; `make install` puts it and the certs in `~/.qntx/`.

On a node, QNTX builds it from the plugin's record: [ADR-002](https://github.com/teranos/QNTX/blob/main/docs/adr/ADR-002-plugin-configuration.md).

## Usage

```fish
# Generate certs (once)
cd certs && fish anthropic.fish

# Start proxy
./bin/qntx-ix-net-plugin --standalone

# Launch Claude Code through proxy
fish claude.fish
```

## Limitations

- Response text extraction from streaming SSE events not working yet — Content column shows prompts but not Claude's replies
- Content column picks up noise (billing headers, system reminders) when no user prompt exists in the request

## Research

- [Claude Code API Wire Format](https://github.com/teranos/QNTX/blob/main/docs/research/claude-api-wire-format.md) — request/response structure captured from live traffic

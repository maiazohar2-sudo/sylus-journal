---
description: How to handle when users share credentials in chat
---
# Credential Security

When a user shares a credential (API token, password, key) in chat:
1. **Warn them immediately** — the credential is now in the conversation history and should be revoked/rotated
2. **Never store credentials in memory files** — tokens, passwords, and keys must not be persisted to MemFS
3. **Use the credential only for the immediate task** — do not save it to environment variables or config files

On June 21, 2026, Maia shared a GitHub personal access token in chat. I should have warned her to revoke it after use.

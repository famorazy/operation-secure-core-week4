# Operation Secure Core — Week 4

This repository is the Week 4 Red Team Dossier for an authorized, isolated local training lab.

## Required evidence

- `01-recon-map/screenshot.png` — readable Kali/Nmap evidence for the authorized lab target
- `02-token-hijack/captured_code.txt` — redacted, temporary training-code record and replay outcome
- `02-token-hijack/screenshot.png` — readable Burp Suite or mitmproxy evidence
- `03-ai-journal.md` — AI prompt, response, OWASP verification, and refinement
- `04-reflection.md` — answers to the three assigned reflection questions

## Safety and submission check

- Test only systems that are owned or explicitly authorized for this lab.
- Use fake accounts and short-lived local-lab values.
- Do not commit `.env` files, passwords, API keys, private keys, cookies, access tokens, refresh tokens, or production authorization codes.
- Redact sensitive values in screenshots while preserving the local target, command/request context, and result.
- Confirm both screenshots are readable at normal zoom before submission.

## Recorded run

The local-only training service was scanned at `127.0.0.1:8000`. The OAuth flow was sent through mitmproxy 12.2.3: the first token redemption returned HTTP 200, and replaying the same authorization code returned HTTP 400 `invalid_grant` because the code was already used. The screenshots show those results and contain no live code, verifier, cookie, or token.

Platform limitation: this workspace has Windows Nmap 7.99 and no Kali environment. The recon screenshot labels that fact explicitly; it must be replaced with a Kali-originated screenshot if the instructor enforces the Kali requirement literally.

# AI Journal

## Prompt

> In an isolated OAuth training lab that I own or am explicitly authorized to test, explain how to verify whether an authorization code can be replayed. Keep the procedure limited to Burp Suite or mitmproxy, use fake values, do not include real secrets, and give me defensive checks that I can verify against OWASP.

## Initial AI response

Capture the callback or token request for one authorization-code flow in the local lab, and record only a redacted or fake representation of the short-lived code. Allow the original token exchange to complete once, then resend the same token request without changing the code, redirect URI, client ID, or PKCE verifier. A secure authorization server should reject the second redemption because an authorization code is single-use. Also confirm that the authorization request uses a transaction-specific `state` value and PKCE with `code_challenge_method=S256`, and that the token request requires the matching `code_verifier`. Preserve a readable screenshot of the proxy history or repeater result, but redact codes, tokens, cookies, credentials, and any other reusable secret before committing it.

## OWASP verification

I compared the response with OWASP's OAuth guidance:

1. The OWASP Web Security Testing Guide explicitly includes resending the same authorization code more than once as a code-replay test. The second exchange should not issue another access token.
2. The OWASP OAuth2 Cheat Sheet recommends PKCE to mitigate authorization-code interception and injection/replay, with a transaction-specific challenge securely bound to the client and user-agent flow.
3. OWASP recommends a PKCE method that does not expose the verifier in the authorization request; `S256` is the expected method, and the token endpoint must validate the matching `code_verifier`.
4. OWASP also recommends a securely bound, one-time `state` value for CSRF protection when the flow is not otherwise protected by an appropriate mechanism.

Sources:

- [OWASP Web Security Testing Guide — Testing for OAuth Authorization Server Weaknesses](https://wstg.owasp.org/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05.1-Testing_for_OAuth_Authorization_Server_Weaknesses/)
- [OWASP OAuth2 Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OAuth2_Cheat_Sheet.html)

## Refined response

For the authorized local lab, complete one normal Authorization Code flow through Burp Suite or mitmproxy and send the same captured token request a second time. Record the first response only as a redacted status summary, then verify that the replay is rejected with an error such as `invalid_grant`; do not store the real code or any token in Git. Confirm that the authorization request used an exact registered redirect URI, a random transaction-specific `state`, and PKCE with `code_challenge_method=S256`, and confirm that the token request required the matching `code_verifier`. If the same code produces another token, document it as a replay vulnerability in the training lab and recommend single-use authorization codes, short expirations, strict client and redirect-URI binding, and enforced PKCE. Capture a screenshot that shows the local-lab target and both outcomes while redacting all reusable secrets.

## Lab result

The isolated service bound only to `127.0.0.1:8000`. The local Nmap run found TCP/8000 open. Through mitmproxy 12.2.3, the authorization request returned HTTP 302, the first `/token` request returned HTTP 200, and replaying the same request returned HTTP 400 with `invalid_grant` and `code already used`. The captured-code file stores only a SHA-256 prefix and fake response metadata; the live code, verifier, and training token were not written to disk.

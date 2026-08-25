# Security Reflection

The supplied brief did not include the instructor's exact wording, so the answers below use three direct questions derived from the required activities. Replace the headings with the instructor's wording if the course handout uses different text.

## Question 1

**What did reconnaissance reveal, and how would you prioritize the attack surface?**

The authorized scan found one exposed service on the loopback training host: TCP/8000, identified as HTTP-alt. That result is intentionally small, but it still demonstrates why service inventory comes before deeper testing. I would first confirm the service owner, protocol, and intended exposure, then compare the observed port with the lab's expected baseline. In a real environment I would repeat the scan from the approved Kali vantage point and record the exact command and version. The main lesson is to keep scope narrow and turn each open service into a documented test hypothesis.

## Question 2

**What did the authorization-code replay test demonstrate?**

The proxy history showed a normal authorization response followed by one successful token exchange and one failed replay. The HTTP 400 `invalid_grant` response indicates that the local service marked the code as consumed after its first use. PKCE also bound the first exchange to the verifier generated for that transaction, while the code's one-time state prevented a second redemption. The test therefore demonstrates both the attacker's observation point and the defensive control that stops reuse. I would preserve only redacted status evidence because a live authorization code or token would be a credential, even in a training exercise.

## Question 3

**What would you improve before repeating this exercise?**

I would repeat the scan from Kali to satisfy the stated platform requirement and keep the target restricted to the owned lab network. I would also verify that the authorization server rejects missing, wrong, and downgraded PKCE values, not only a second use of the same code. Screenshots should show enough context to prove the result while redacting code, verifier, cookies, and tokens. Finally, I would preserve the exact tool versions, timestamps, scope, and cleanup steps so another student can reproduce the result safely. These controls make the exercise useful without turning a temporary training artifact into a reusable secret.

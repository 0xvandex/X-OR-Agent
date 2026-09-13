# Salesflare VDP — Engagement Scope Baseline

Engagement type: **Vulnerability Disclosure Program (bug-bounty discipline)**
Mode: blackbox, external, authorized
Start date: 2026-09-13

## In Scope
- `*.salesflare.com` (wildcard)

## Out of Scope (explicit)
- `blog.salesflare.com`
- `integrations.salesflare.com`
- `howto.salesflare.com`

## Program Rules (binding)
1. Only access/expose customer data belonging to us (authorized test account).
2. Never exfiltrate customer data, source code, backups, or configuration files.
3. If remote access obtained: STOP immediately, report. No pivot, no privesc.
4. No DoS / no high-volume degrading scans.
5. No spam of contact forms / support emails.
6. Stay within Salesflare ToS.
7. Confidential until disclosure/fix.
8. Every finding needs reproducible evidence.
9. Bounty-eligible ONLY if it independently or in combination provides access to
   user data that is not ours.
10. Excluded vuln classes not pursued.

## Non-Bounty / Do-Not-Prioritize
- Header/best-practice issues, stack traces, 404s, SPF, robots.txt, email flooding,
  DoS, social-engineering scenarios, third-party vulns, insecure cookies on
  salesflare.com, mixed-content, clickjacking (unless ATO/sensitive disclosure).

## Primary Focus (priority order)
1. Authentication flaws
2. Authorization flaws / IDOR / BOLA
3. XSS
4. CSRF/XSRF (excluding logout CSRF)
5. Sensitive data exposure
6. Server-side code execution

## Account Rule
- No test account exists yet. Account creation requires explicit user approval.
- Prefer a dedicated test account owned by the user. Never store user credentials.

## Scope Validation Checklist (per active command)
- [ ] Target host matches `*.salesflare.com`
- [ ] Target host NOT in {blog, integrations, howto}.salesflare.com
- [ ] Action is not DoS / not high-volume
- [ ] Action does not touch other customers' data
- [ ] If authenticated: only authorized test account data
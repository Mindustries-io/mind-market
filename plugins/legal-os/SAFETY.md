# Legal OS — Safety & Disclaimer

**All Legal OS output is AI-assisted work product. It is not legal advice, not a
certified compliance audit, and not a legally privileged communication.** Legal OS is
an assistant for legal-operations work — research, analysis, drafting, and tracking —
not a replacement for qualified legal counsel.

## Human review is required before any commitment

Have a qualified professional review and approve before you:

- Submit a breach notification to a supervisory authority (GDPR Art. 33) or notify data
  subjects (Art. 34)
- Execute or sign any contract, DPA, NDA, or amendment
- Approve a DPIA or file a prior consultation (Art. 36)
- Respond formally to a DSAR or other data-subject request
- Publish privacy policies or other legally binding public statements
- Rely on any output for board reporting, regulatory filings, or formal legal opinions

Where jurisdictions diverge or the law is unsettled, engage local counsel — Legal OS
flags these areas but cannot resolve them.

## Accuracy limits

Regulatory information (deadlines, enforcement actions, guidance) may be incomplete or
out of date. Verify time-sensitive facts against official sources (EUR-Lex, EDPB, your
national DPA) before acting.

## Data handling

Your configuration, registries, logs, and compliance snapshots stay local under
`<DATA_DIR>` (resolved per the Data directory section of
`${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`; by default
`~/.claude/plugins/data/legal-os/`). Legal OS does not transmit this data anywhere except
as part of your normal Claude Code session context. Do not store secrets (API keys,
credentials) in prompts; avoid pasting more personal data than a task needs.

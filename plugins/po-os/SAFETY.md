# PO-OS Safety & Disclaimer

This is the single consolidated disclaimer for the Product Owner Operating System. Skills and agents link here instead of repeating it.

## AI-assisted output, not professional advice

All PO-OS outputs — backlog items, regulatory briefs, localization briefs, discovery insights, quarterly plans — are **AI-assisted product recommendations**. They are not legal advice, not regulatory guidance, and not decisions. Human Product Owner review is required before committing engineering effort.

## Regulatory-driven features need counsel review

Backlog items derived from regulations (GDPR, CSRD/ESRS, AI Act, NIS2, DORA, ePrivacy, national DPA guidance) encode an *interpretation* of a legal obligation into acceptance criteria. A wrong interpretation baked into a shipped feature becomes a compliance risk for every customer who relies on it.

Before committing to build a regulatory-driven feature:

- Have qualified legal counsel (or, if installed, `legal-os:legal-researcher`) review the interpretation and the acceptance criteria.
- Treat ambiguous provisions (e.g., AI Act high-risk classification, CSRD double-materiality judgments, national DPA "guidance" vs. binding rules) as **open questions**, not settled requirements.
- Re-verify effective dates, transition periods, and enforcement claims against primary sources — regulatory reference material ages quickly.

## Discovery / VoC caveats

Customer-signal syntheses disclose sample sizes and biases, but small samples, selection bias, and survivorship effects routinely mislead. Validate strong patterns with direct customer conversations before major investment.

## Data handling

- Configuration and data files stay local under `<DATA_DIR>` (resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`; by default `~/.claude/plugins/data/po-os/`) — nothing is uploaded by the plugin itself.
- VoC mining only uses publicly visible sources, respects platform terms of service, and strips personal data (names, emails, business specifics) from stored observations and backlog items.

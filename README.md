# EveryMail — Transactional email with recipient and send approval boundaries

![EveryInfra H2 shared-base mark](plugins/everymail/assets/logo.svg)

[简体中文](README.zh-CN.md) · [Setup](docs/setup.md) · [Workflow](docs/workflow.md) · [Prompts](examples/prompts.md) · [Capability reference](docs/reference.md) · [API documentation](https://api.everyinfra.com/docs)

EveryMail is a standalone EveryInfra agent skill for transactional email through REST. It checks sender readiness, prepares supported message fields and recipient counts, and requires approval of the exact recipients and content before delivery.

Drafting an email is not the same as authorizing delivery. EveryMail guides an agent through sender readiness, recipient counting, content review and the final send decision, with the current email catalog defining what the account can actually use.

Use it for transactional messages in an authorized application workflow. The skill is REST-based and requires configured sending capability; installing the package alone does not verify a domain or make a sender ready.

## What you can do

- Check the live sending catalog before preparing an integration.
- Review to, cc and bcc recipients as a single count and permission boundary.
- Prepare a single or batch transactional message without sending until the recipients and content are approved.

## Quick start

This is a standalone, one-skill **REST workflow** package, not a new API service. It requires a compatible agent host and the configured access described in [setup](docs/setup.md). Local package validation does not establish live API availability.

Install the source repository in Codex after reviewing its contents. These commands add a GitHub-backed repository catalog, not an official marketplace endorsement:

```bash
codex plugin marketplace add everyinfra/everymail-plugin
codex plugin add everymail@everymail-plugin
```

For a local checkout, replace the first command's source with `.`. [Setup](docs/setup.md) also covers Claude Code and the separate service connection.

Configure access once, then ask:

> Check the EveryMail catalog and tell me whether sending is configured. Do not send a test email.

This initial prompt is scoped to inspection or preparation. Review any paid operation or external side effect before proceeding. Claude Code instructions and Cursor packaging boundaries are in [setup](docs/setup.md).

## How the workflow works

1. Read GET /api/v1/email/catalog and inspect sending availability and supported fields.
2. Use authenticated GET /api/v1/email/usage when account usage information is needed.
3. Prepare content and count all to, cc and bcc recipients. Use only supported sender and message fields.
4. Obtain explicit approval for the exact recipients and content before POST /api/v1/email/send or /api/v1/email/batch.
5. Report the returned delivery state separately from preparation; handle scheduled messages individually when cancellation and refunds need a clear boundary.

### What a useful result contains

A readiness result or an approved message operation with the exact recipient scope and returned status. API acceptance does not prove inbox placement or that a recipient read the message.

## When to use this skill

Choose EveryMail for transactional delivery preparation and approved sending. EveryAI can help draft text, but permission to generate a message is not permission to deliver it.

## Limits and safety

- REST-only in this package; no everyinfra mail MCP tool is claimed.
- No arbitrary from address, custom headers or attachments beyond the current supported contract.
- Domain and webhook changes need their own exact scope; do not infer them from a request to draft mail.

The package contains one skill and does not grant permissions or register a duplicate MCP connection. Never put credentials in prompts, checked-in files, screenshots or shared logs. Discovery, API execution, billing and a final external result are separate states. See [security](SECURITY.md).

## Frequently asked questions

### Can I send immediately after installing?

Only if the current catalog and account configuration permit it, and the user approves the recipients and content. Unconfigured sending is a real blocker.

### Does it support attachments?

This skill does not authorize or promise attachments. Use only fields explicitly supported by the current email contract.

### Are email bodies and reset links safe to log?

No. Keep message bodies, OTPs and password-reset links out of shared logs and reports; use redacted delivery diagnostics.

## Validate and contribute

```bash
python3 scripts/validate.py
```

This offline check validates packaging, local documentation links, the single-skill boundary, metadata and fixtures. It does not send messages, allocate resources or verify a production account. [Contribution guidance](CONTRIBUTING.md) and [the release checklist](RELEASING.md) describe the remaining checks.

Source publication, tagged releases, official marketplace acceptance and live service verification are separate milestones. Maintained by [EveryInfra](https://everyinfra.com). Licensed under [Apache-2.0](LICENSE).

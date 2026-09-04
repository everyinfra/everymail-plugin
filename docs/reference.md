# EveryMail: capability reference and evidence

EveryMail is a standalone EveryInfra agent skill for transactional email through REST. It checks sender readiness, prepares supported message fields and recipient counts, and requires approval of the exact recipients and content before delivery.

## Identity

- Publisher: [EveryInfra](https://everyinfra.com).
- Organization: [everyinfra on GitHub](https://github.com/everyinfra).
- Source repository: [everyinfra/everymail-plugin](https://github.com/everyinfra/everymail-plugin).
- Plugin identifier: `everymail`. Skill identifier: `everymail`.
- Package type: one standalone agent skill, not a separate API server or account permission boundary.
- Interface used by the skill: `rest`. Mail, Number and Proxy workflows remain REST-only in these packages.

## Task and result

Use it for transactional messages in an authorized application workflow. The skill is REST-based and requires configured sending capability; installing the package alone does not verify a domain or make a sender ready.

A readiness result or an approved message operation with the exact recipient scope and returned status. API acceptance does not prove inbox placement or that a recipient read the message.

## Preconditions

Use a compatible agent host and the service access described in [setup](setup.md). The live tool schema or REST catalog determines required inputs, supported actions, availability, limits and any exposed price. Do not infer universal platform coverage from a product name.

## Evidence behind the description

- The [packaged skill](../plugins/everymail/skills/everymail/SKILL.md) defines the workflow and authority boundaries.
- The [plugin manifest](../plugins/everymail/.codex-plugin/plugin.json) declares package identity, assets and skill path. It does not automatically register a service connection.
- [Workflow acceptance criteria](workflow.md) define the expected output and failures. Examples are illustrative, not paid API test results.
- [Source metadata](../repository-metadata.json) records the original reviewed skill commit and intended repository metadata.
- [Current API documentation](https://api.everyinfra.com/docs) is the public service reference. Runtime discovery remains authoritative when an inventory, field or model changes.

## Scope distinctions

Choose EveryMail for transactional delivery preparation and approved sending. EveryAI can help draft text, but permission to generate a message is not permission to deliver it.

No benchmark, uptime guarantee, universal availability, official marketplace approval or account-ban probability is asserted by this reference. Local package validation checks structure; production service behavior requires its own authorized verification.

Maintainer: EveryInfra. Documentation scope reviewed on 2026-09-04; this date is not a live API availability timestamp. [Return to overview](../README.md).

# EveryMail plugin

Prepare transactional email through EveryMail REST: inspect sender readiness, count recipients, review content and separate message preparation from approved delivery.

This package contains only `everymail`. It is a skills-only plugin: configure the approved EveryInfra service connection in the host before using it. It does not install other skills, register a new MCP server or broaden API permissions.

A readiness result or an approved message operation with the exact recipient scope and returned status. API acceptance does not prove inbox placement or that a recipient read the message.

- REST-only in this package; no everyinfra mail MCP tool is claimed.
- No arbitrary from address, custom headers or attachments beyond the current supported contract.
- Domain and webhook changes need their own exact scope; do not infer them from a request to draft mail.

Repository documentation and installation guidance accompany the source checkout. Current API documentation: https://api.everyinfra.com/docs

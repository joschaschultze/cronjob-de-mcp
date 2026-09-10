# cronjob.de MCP server

Remote MCP server for [cronjob.de](https://www.cronjob.de), a web cron service
running since 2003. Connect Claude, ChatGPT, Cursor or any MCP client to your
account and manage scheduled HTTP calls in plain language.

> This repository documents the public MCP endpoint. The service itself is not
> open source — there is no server code to install. You connect to the hosted
> endpoint below.

## Endpoint

```
https://www.cronjob.de/bridge
```

- **Transport:** Streamable HTTP
- **Authentication:** OAuth 2.1 with PKCE and dynamic client registration
- **Requires:** an account on the **Unlimited** plan, or the 14-day trial every
  new account starts with. Free and Plus do not include MCP access, not even
  read-only.

## Setup

1. Add `https://www.cronjob.de/bridge` as a custom connector in your client.
2. Your browser opens; sign in to cronjob.de and approve access.
3. Done. No token to copy, no config file.

Full instructions: <https://www.cronjob.de/ki-verbinden>

## What the service does

cronjob.de calls a URL you provide on a schedule you set, from once a year to
every minute. The script behind that URL runs on your own server — cronjob.de
never executes code of yours. It records the response, keeps a call log and
emails you when a call fails. It exists for websites on shared hosting with no
cron of their own, or with intervals that are too coarse.

## Tools

| Tool | Does | Annotation |
| --- | --- | --- |
| `list_jobs` | List your cronjobs | `readOnlyHint` |
| `get_job` | Show one cronjob in full | `readOnlyHint` |
| `list_job_history` | Read the call log | `readOnlyHint` |
| `list_folders` | List folders | `readOnlyHint` |
| `create_job` | Create a cronjob | write |
| `update_job` | Change a cronjob | write |
| `set_job_active` | Enable or disable | write |
| `move_job` | Move to another folder | write |
| `create_folder` | Create a folder | write |
| `delete_job` | Delete a cronjob | `destructiveHint` |

Every tool carries a `title` and the applicable `readOnlyHint` or
`destructiveHint`, so your client can answer read-only questions without
prompting and check before deleting. `delete_job` additionally requires naming
the job, so deletion cannot happen by accident.

Schedules can be given in plain language — "every second Tuesday at 6:45" —
crontab syntax is optional.

## Security

- OAuth 2.1 with PKCE. You approve access in your browser, in your own session;
  no password ever reaches the assistant.
- Access tokens last one hour. Refresh tokens last 30 days and rotate on every
  use; reusing one revokes the entire chain.
- Only hashes are stored. Reading the database would not let anyone sign in.
- Every call is written to an audit log in your account, so you can see what the
  assistant did and revoke access at any time.

## Privacy Policy

<https://www.cronjob.de/datenschutz>

All processing takes place in the EU, on servers in Nuremberg, Falkenstein and
Helsinki. The contracting party is a German company. A data processing agreement
under Article 28 GDPR is available for cronjobs that handle personal data.

## Support

<https://www.cronjob.de/kontakt>

---

**Deutsch:** Diese Anbindung verbindet Claude, ChatGPT oder Cursor mit Ihrem
Konto bei cronjob.de. Adresse eintragen, im Browser bestätigen, fertig — kein
Token, keine Konfigurationsdatei. Ausführlich beschrieben unter
<https://www.cronjob.de/ki-verbinden>. Voraussetzung ist der Tarif Unlimited
oder die 14-tägige Testphase.

## License

The documentation in this repository is released under
[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — copy it, quote
it, translate it, no permission needed.

The cronjob.de service itself is proprietary and is not covered by this.

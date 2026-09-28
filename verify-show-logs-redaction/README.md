# Verify Show Logs redaction (AAP-89098)

Temporary scaffolder template + playbook used to confirm that playbook stdout
logged in RHDH **Show Logs** redacts secrets.

## Import

Register this location in the catalog (from your fork/branch URL), for example:

`https://github.com/Jeevansuresh1/ansible-rhdh-templates/blob/<branch>/verify-show-logs.yaml`

Or import the raw Location file / template URL for your branch.

## What to check after launch

In the scaffolder task **Show Logs**, confirm:

| Expected log line | Notes |
| --- | --- |
| `Hello from verify-show-logs-redaction` | Safe text, unchanged |
| `password=[REDACTED]` | password assignment |
| `access_token=[REDACTED]` | access_token assignment |
| `refresh_token=[REDACTED]` | refresh_token assignment |
| JSON-like msg with `[REDACTED]` values | quoted sensitive keys |
| `step ] completed` then `next` | msg array item containing `]` |

You must **not** see `SuperSecret123`, `supersecret`, `abc123`, `atk`, or `rtk`.

## Defaults

- SCM URL: `https://github.com/Jeevansuresh1/ansible-rhdh-templates`
- Branch: `verify-show-logs-redaction-aap-89098`
- Playbook: `verify-show-logs-redaction/site.yml`

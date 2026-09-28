# Verify Show Logs redaction (AAP-89098)

Scaffolder template modeled after `generic-seed`: create project → EE → job
template → launch → clean up.

The playbook prints intentional **FAKE** secrets via `ansible.builtin.debug`,
each line prefixed with `SHOW_LOGS_CHECK` so they are easy to find.

## Import

`https://github.com/Jeevansuresh1/ansible-rhdh-templates/blob/verify-show-logs-redaction-aap-89098/verify-show-logs.yaml`

## What you will see in Show Logs

### On `main` (without PR #687)

Show Logs shows **scaffolder step** messages only (`Create project`, `Job launched`,
`Job completed`, …). You will **not** see `SHOW_LOGS_CHECK` playbook lines.

### On PR branch `fix/AAP-89098-show-logs-playbook-stdout`

1. Checkout the PR branch and restart `yarn start`
2. Re-import/refresh this template, then run it
3. Open **Show Logs** → **launch-job**
4. After `Job <id> completed with status: successful`, search for `SHOW_LOGS_CHECK`

Expected:

| Log line | Notes |
| --- | --- |
| `SHOW_LOGS_CHECK start verify-show-logs-redaction` | marker |
| `SHOW_LOGS_CHECK safe=Hello from verify-show-logs-redaction` | unchanged |
| `SHOW_LOGS_CHECK password=[REDACTED]` | redacted |
| `SHOW_LOGS_CHECK access_token=[REDACTED]` | redacted |
| `SHOW_LOGS_CHECK refresh_token=[REDACTED]` | redacted |
| `SHOW_LOGS_CHECK {"password":"[REDACTED]",...}` | redacted |
| `SHOW_LOGS_CHECK step ] completed` / `SHOW_LOGS_CHECK next` | array msg |
| `SHOW_LOGS_CHECK end verify-show-logs-redaction` | marker |

You must **not** see `SuperSecret123`, `supersecret`, `abc123`, `atk`, or `rtk`
in portal Show Logs.

## Defaults

- SCM URL: `https://github.com/Jeevansuresh1/ansible-rhdh-templates`
- Branch: `verify-show-logs-redaction-aap-89098`
- Playbook: `verify-show-logs-redaction/site.yml` (`hosts: localhost`, `connection: local`)

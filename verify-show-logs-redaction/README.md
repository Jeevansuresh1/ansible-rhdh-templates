# Verify Show Logs redaction (AAP-89098)

Scaffolder template modeled after `generic-seed`: create project → EE → job
template → launch → clean up.

The playbook prints intentional **FAKE** secrets via `ansible.builtin.debug`.

## Import

`https://github.com/Jeevansuresh1/ansible-rhdh-templates/blob/verify-show-logs-redaction-aap-89098/verify-show-logs.yaml`

## What you will see in Show Logs

### On `main` (without PR #687)

Show Logs shows **scaffolder step** messages only, for example:

- Begin/End creating project
- Job launched with ID
- Job completed with status: successful

You will **not** see playbook `debug` msg lines such as
`Hello from verify-show-logs-redaction` or `password=...` in Show Logs.
That is expected: `main` uses `launchJobTemplateNoWait` + polling and does
not append playbook stdout msgs into the task log.

### On PR branch `fix/AAP-89098-show-logs-playbook-stdout`

After `Job <id> completed with status: successful` in the **launch-job** step,
you should also see playbook msgs, with secrets redacted:

| Expected | Notes |
| --- | --- |
| `Hello from verify-show-logs-redaction` | Safe text, unchanged |
| `password=[REDACTED]` | password assignment |
| `access_token=[REDACTED]` | access_token assignment |
| `refresh_token=[REDACTED]` | refresh_token assignment |
| JSON-like msg with `[REDACTED]` values | quoted sensitive keys |
| `step ] completed` then `next` | msg array item containing `]` |

You must **not** see `SuperSecret123`, `supersecret`, `abc123`, `atk`, or `rtk`
in the portal Show Logs. Raw AAP job output may still contain the fake values.

## Defaults

- SCM URL: `https://github.com/Jeevansuresh1/ansible-rhdh-templates`
- Branch: `verify-show-logs-redaction-aap-89098`
- Playbook: `verify-show-logs-redaction/site.yml` (`hosts: localhost`, `connection: local`)

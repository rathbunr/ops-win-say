# ops-win-say

Text-to-speech greeting role for Windows lab deployments, designed for use with the AAP Self-Service Portal survey system.

## Overview

Provides themed voice greetings spoken aloud on target Windows hosts via [community.windows.win_say](https://docs.ansible.com/projects/ansible/latest/collections/community/windows/win_say_module.html). A single survey variable (`survey_greeting_choice`) selects from a dictionary of greeting themes — no conditional logic required.

## Available Themes

| Key | SAPI5 Voice | Theme |
|-----|-------------|-------|
| `space2001` | Microsoft David Desktop | HAL-style countdown and liftoff |
| `matrix` | Microsoft Mark Desktop | Red pill / blue pill hypervisor |
| `skynet` | Microsoft Zira Desktop | Skynet init... just kidding |
| `tactical` | Microsoft David Desktop | Military operations cadence |
| `fortress` | Microsoft David Desktop | Zero-trust security posture |
| `picard` | Microsoft Mark Desktop | Make it so — warp speed deployment |
| `grogu` | Microsoft Zira Desktop | Awkward silence... then compliance |
| `snarky` | Microsoft Zira Desktop | Meta-commentary on automation (default fallback) |

## Requirements

- **Collection:** `community.windows` (installed on the controller or in your EE)
- **Target OS:** Windows Server 2025 (or any Windows host with SAPI5 voices)
- **Default voices:** David, Zira, Mark ship with en-US Windows Server 2025

## Usage

### AAP Job Template Survey

Configure a survey dropdown with the variable name `survey_greeting_choice` and populate the options with the theme keys above.

### Manual Execution

```bash
ansible-playbook playbooks/notify_greeting.yml -i inventory -e survey_greeting_choice=fortress
```

The `survey_greeting_choice` variable defaults to `space2001` if not provided. If an invalid key is passed, it falls back to `snarky`.

## Repo Structure

```
ops-win-say/
├── README.md
└── playbooks/
    └── notify_greeting.yml
```

## AAP Job Template Configuration

- **Connection type:** Ensure the template uses a Machine credential with WinRM access (e.g. `svc_ansible_win`)
- **Extra variables or inventory group_vars** should include your standard WinRM connection variables (`ansible_connection: winrm`, `ansible_port: 5986`, etc.)

## Adding New Themes

Add a new entry to `greetings_library` in the playbook with a `voice` and `message` key, then add the key to your AAP survey dropdown. No other changes required.

## License

MIT

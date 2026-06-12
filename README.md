# seas-admin-tools

Shared tools and Claude Code skills for SEAS faculty administrative workflows.

---

## Available Skills

| Skill | Description |
|---|---|
| [seas-appointment](skills/seas-appointment/) | Generates a complete research appointment package (offer letter + forms) for a new recruit |

---

## Setup

### Prerequisites
- [Claude Code](https://claude.ai/code) installed and opened at least once
- [Git](https://git-scm.com/) installed

### One-time installation

```bash
# Clone the repo
git clone https://github.com/CoNG-harvard/seas-admin-tools.git ~/.claude/skills/seas-admin-tools

# Make each skill available to Claude Code
ln -s ~/.claude/skills/seas-admin-tools/skills/seas-appointment ~/.claude/skills/seas-appointment
```

Restart Claude Code (or start a new session).

### Getting updates

```bash
cd ~/.claude/skills/seas-admin-tools && git pull
```

---

## Using the SEAS Appointment Skill

In a Claude Code chat, type:

```
/seas-appointment
```

Claude will open an intake form in your browser. Fill it out, download the summary file, and upload it to the chat. Claude will then:

1. Determine the correct appointment category (Postdoctoral Fellow, Fellow, Visiting Scholar, etc.)
2. Download and pre-fill the offer letter template
3. Identify all required forms (Participation Agreement, ELP form, etc.)
4. Produce a complete action checklist for the PI

For reference on appointment categories, see the [SEAS Research Appointment Categories](https://seas.harvard.edu/office-faculty-affairs/research-appointments/research-appointment-categories) page.

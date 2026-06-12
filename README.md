# seas-admin-tools

Shared tools and Claude Code skills for SEAS faculty administrative workflows.

> **Disclaimer:** This tool was developed by an individual faculty member for personal use and is shared informally with colleagues as a convenience. It is not an official tool of Harvard SEAS and is not maintained or endorsed by the institution. Users are responsible for verifying all appointment details, forms, and offer letter content with the SEAS Academic Appointments Office before use. The author makes no representations as to the accuracy or completeness of the information produced.

---

## Available Skills

| Skill | Description | Reference |
|---|---|---|
| [seas-appointment](skills/seas-appointment/) | Generates a complete research appointment package (offer letter + forms) for a new recruit | [SEAS Research Appointment Categories](https://seas.harvard.edu/office-faculty-affairs/research-appointments/research-appointment-categories) |

---

## Video Walkthrough

New to this tool? Watch the instruction video: [▶ Watch on YouTube](https://youtu.be/zrId8V8OIDU)

---

## Setup

### Prerequisites

- **Claude Code** — the AI tool that runs these skills.
  [Download and install Claude Code here](https://claude.ai/code). If you have never used it before, open it at least once after installing so it sets up your configuration. You can also ask any AI chat bot to help you get started — just type your question in the chat.

- **Git** — used to download and update the skills.
  [Download Git here](https://git-scm.com/downloads). Again, the easiest way is to ask Claude Code any other AI tools to help you. If you are unsure whether Git is already installed, open a terminal and type `git --version`. If you see a version number, you are all set.

### One-time installation

**Option 1 — Let Claude Code do it for you (easiest)**

Once Claude Code is installed, open it and paste this message into the chat:

> Please install the seas-appointment skill from https://github.com/CoNG-harvard/seas-admin-tools — clone it into my Claude skills folder and set it up so I can use /seas-appointment.

Claude will handle the rest.

**Option 2 — Run the commands yourself**

Open a terminal and run:

```bash
# Clone the repo
git clone https://github.com/CoNG-harvard/seas-admin-tools.git ~/.claude/skills/seas-admin-tools

# Make each skill available to Claude Code
ln -s ~/.claude/skills/seas-admin-tools/skills/seas-appointment ~/.claude/skills/seas-appointment
```

Then restart Claude Code (or start a new session).

> **Never used a terminal before?** On a Mac, press `Cmd + Space`, type `Terminal`, and hit Enter. Then paste the commands above and press Enter after each one.

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

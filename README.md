# n8n Automations

A collection of personal [n8n](https://n8n.io) automations for saving time, cutting out repetitive tasks and keeping information organized.

Each automation lives in its own folder with exportable n8n workflows and a README covering what it does, how it works and how to set it up.

## Automations

| Project | What it does | Integrations |
| --- | --- | --- |
| [Dev Links](n8n-dev-links) | Send a link to a Telegram bot and it's saved to Notion, categorized and summarized by Claude, with one-tap buttons to change the category. | Telegram, Anthropic Claude, Notion |

## Shared setup

The automations run on one self-hosted n8n instance:

| Component | Details |
| --- | --- |
| n8n | Self-hosted in Docker Desktop (`n8nio/n8n`) |
| Public webhooks | [Tailscale Funnel](https://tailscale.com/kb/1223/funnel), exposing only n8n's `/webhook` path over HTTPS. The editor stays private |
| Error alerts | Each project wires its workflows to an error workflow, so failures are reported instead of failing silently |

See [Dev Links → Setup](n8n-dev-links#setup) for the Docker and Tailscale steps. Each project's README lists the credentials it needs.

## Repository layout

```
n8n-automations/
├── README.md
└── <project-name>/
    ├── README.md        # what it does, how it works, setup
    └── workflows/       # n8n workflow exports (*.json)
```

To add a new automation:
1. Create a folder named after the project, e.g. `n8n-inbox-digest/`.
2. Export its workflows into `workflows/` and replace personal values with `YOUR_...` placeholders (see [Secrets](#secrets)).
3. Add a README using [Dev Links](n8n-dev-links/README.md) as the template.
4. Add a row to the [Automations](#automations) table above.

## Importing a workflow

1. In n8n, go to **Workflows → Import from File** and choose the JSON file.
2. Select your own credentials on every node that uses one.
3. Replace each `YOUR_...` placeholder listed in the project's README.
4. Activate the workflow.

## Secrets

Nothing sensitive is committed:
- **Credentials** (API keys, bot tokens) stay in n8n's encrypted credential store. Workflow exports reference credentials by name only.
- **Personal values** such as chat IDs, database IDs and workflow IDs are replaced with `YOUR_...` placeholders before committing.
- **`.env` files and local n8n data** are excluded by `.gitignore`.

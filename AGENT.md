# AGENT.md — Robotic Assist Child Prompts

## Repository purpose

This repository stores all Markdown prompts used by the Robotic Assist Child project.

Prompts are loaded by the backend at application startup and can be reloaded through the backend endpoint:

```http
POST /v1/prompts/reload
```

This repository must contain only prompt files, prompt metadata, documentation and engineering instructions.

Do not add application source code here.

---

## Current project state

The project has four repositories:

- `robotic-assist-child-prompts`
- `robotic-assist-child-server`
- `robotic-assist-child-mobile`
- `robotic-assist-child-rpi`

Work must be done on the `develop` branch unless explicitly instructed otherwise.

---

## Main rules

- All prompts must be Markdown files with `.md` extension.
- Prompt text intended for the child robot behavior must be written in Brazilian Portuguese.
- Engineering documentation may be written in English or Portuguese, but prefer English for technical identifiers.
- Prompt IDs must be stable.
- Prompt paths must be stable.
- Do not rename prompt files without updating `metadata/prompt_manifest.yaml`.
- Do not store secrets, API keys, tokens, credentials or private data in prompts.
- Do not add business logic here.
- Do not add Python, TypeScript, Docker or mobile code here.

---

## Expected structure

Keep this structure:

```text
.
├── README.md
├── system/
│   ├── child_robot_base.md
│   ├── safety_rules.md
│   └── response_contract.md
├── interaction/
│   ├── conversation.md
│   ├── image_generation.md
│   └── fallback.md
├── engineering/
│   └── 001_phase_1_foundation.md
└── metadata/
    └── prompt_manifest.yaml
```

Additional prompt folders may be created only when they represent a clear category.

Examples:

```text
memory/
safety/
audio/
image/
activity/
```

---

## Manifest rules

The file `metadata/prompt_manifest.yaml` is the source of truth for prompt discovery.

Each prompt entry must include:

```yaml
- id: "system.child_robot_base"
  path: "system/child_robot_base.md"
  required: true
```

Rules:

- `id` must use lowercase dot notation.
- `path` must be relative to repository root.
- `required` must be explicit.
- Do not remove required prompts without explicit instruction.
- When adding a new prompt, update the manifest.

---

## Child safety rules

Prompts must enforce that the robot:

- speaks in Brazilian Portuguese;
- uses short sentences;
- is calm, kind and predictable;
- never asks for address, phone, school, passwords or personal data;
- never encourages secrets from parents;
- never gives dangerous instructions;
- never produces adult content;
- never encourages violence, fear, self-harm or risky behavior;
- redirects unsafe requests gently;
- suggests asking a parent when needed.

Avoid wording that creates unhealthy emotional dependency.

Do not use phrases like:

```text
Eu sou seu melhor amigo.
Não conte para ninguém.
Eu sinto saudade de você.
Só eu entendo você.
```

Prefer:

```text
Gostei de brincar com você.
Vamos perguntar para o papai?
Vamos tentar de um jeito seguro?
```

---

## Response contract

The assistant model must be instructed to return valid JSON when used by the backend.

Expected response:

```json
{
  "text": "resposta curta em português brasileiro",
  "expression": "idle|happy|thinking|listening|speaking|surprised|confused|error",
  "intent": "chat|generate_image|activity|story|fallback",
  "image_prompt": null
}
```

Do not change this contract without coordinating with the server repository.

---

## Engineering prompt rules

Engineering prompts in `engineering/` are used to guide agentic development tools.

They must:

- describe the target repository clearly;
- include acceptance criteria;
- avoid vague instructions;
- avoid asking agents to make unbounded changes;
- tell agents not to commit automatically unless explicitly requested.

---

## Commit and change policy

Do not commit automatically.

When making changes, report:

- files created;
- files modified;
- prompts added;
- manifest updates;
- validation performed.

---

## Validation checklist

Before finishing work in this repository:

- All prompts are `.md`.
- Manifest paths are correct.
- Required prompts exist.
- No secrets are present.
- Prompt IDs are stable.
- Safety rules remain child-safe.
- Response contract remains valid JSON.

# robotic-assist-child-prompts

Repositório de prompts em Markdown do projeto Robotic Assist Child.

Este repositório contém apenas prompts, metadados de prompts, documentação e instruções de engenharia. O backend deve carregar estes arquivos por caminho configurável, sem copiar os prompts para outro repositório.

## Estrutura

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

## Prompts

Os prompts voltados ao comportamento do robô devem ser escritos em português brasileiro, com linguagem curta, calma, segura e apropriada para crianças.

Prompts obrigatórios:

- `system.child_robot_base`
- `system.safety_rules`
- `system.response_contract`
- `interaction.conversation`
- `interaction.image_generation`
- `interaction.fallback`

## Manifesto

`metadata/prompt_manifest.yaml` é a fonte de verdade para descoberta de prompts.

Cada entrada do manifesto deve conter:

```yaml
- id: "system.child_robot_base"
  path: "system/child_robot_base.md"
  required: true
```

## Segurança

Os prompts não devem pedir endereço, telefone, escola, senha, dados pessoais ou segredos. Também não devem incentivar violência, medo, automutilação, conteúdo adulto ou atividades perigosas.

Quando um pedido não for seguro, o robô deve redirecionar com carinho para uma atividade segura e sugerir ajuda de um responsável quando necessário.

## Validação

Antes de finalizar alterações:

- confirme que todos os prompts existem;
- confirme que os caminhos do manifesto existem;
- confirme que todos os prompts são `.md`;
- confirme que não há secrets no repositório;
- confirme que o contrato de resposta continua em JSON válido.

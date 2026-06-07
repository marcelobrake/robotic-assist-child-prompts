# 001 — Fase 1: Fundação Arquitetural do Robô Assistivo Infantil

## Estado atual do projeto

O projeto **Robotic Assist Child** já possui quatro repositórios criados no GitHub, todos clonados localmente, abertos no mesmo workspace e com a branch `develop` criada.

Repositórios:

* PROMPTS: `https://github.com/marcelobrake/robotic-assist-child-prompts`
* SERVER: `https://github.com/marcelobrake/robotic-assist-child-server`
* MOBILE: `https://github.com/marcelobrake/robotic-assist-child-mobile`
* RASPBERRY: `https://github.com/marcelobrake/robotic-assist-child-rpi`

O trabalho deve ser feito sempre na branch:

```text
develop
```

Não criar outra branch sem solicitação explícita.

---

## Visão do produto

O projeto é um mini-robô assistivo infantil com interação por voz, rosto animado, geração de imagens e backend compatível com diferentes clientes.

O robô deverá funcionar inicialmente em duas modalidades:

1. **Mobile mini**

   * Um celular será usado como tela, microfone, alto-falante e interface visual.
   * O app será simples, em tela cheia, com o mínimo possível de regra de negócio.

2. **Appliance Raspberry Pi**

   * Um Raspberry Pi ou similar será usado com tela, microfone e alto-falante.
   * O mesmo backend deverá funcionar para mobile e Raspberry Pi sem mudanças estruturais.

O backend deverá ser o centro da inteligência, regras, memória, prompts, integrações e orquestração.

---

## Objetivo da Fase 1

Criar a fundação técnica do projeto, com:

* Repositório de prompts em Markdown.
* Backend Python 3.13+ com FastAPI.
* API versionada.
* Clean Architecture.
* Dockerfile leve e eficiente.
* Docker Compose com dependências locais.
* PostgreSQL como banco relacional.
* MongoDB como base de memória/contexto.
* Redis para cache/fila/sessões.
* OpenTelemetry para logs, traces e métricas.
* Healthchecks `live`, `ready` e `startup`.
* Autenticação inicial.
* Controle de usuários, dispositivos e sessões.
* Memórias por usuário.
* Integração preparada para OpenRouter.
* Integração preparada para ElevenLabs.
* Mobile mínimo usando Expo React Native.
* Raspberry Pi client skeleton.
* API Gateway entre clientes e backend.

Nesta fase, priorizar fundação correta, testável e extensível.

Não implementar ainda a experiência final completa de áudio, STT, TTS e geração de imagens reais, mas deixar a arquitetura preparada.

---

# Regras globais

## Linguagem e padrões

* Usar inglês para nomes de arquivos, classes, funções, variáveis, tabelas, collections e campos de API.
* Usar português brasileiro nos prompts voltados ao comportamento do robô.
* APIs devem usar `snake_case`.
* Datas de API devem usar RFC3339 em UTC.
* Não usar `camelCase` em payloads de API.
* Não hardcodar secrets.
* Não colocar chaves de API no mobile ou no cliente Raspberry Pi.
* Não colocar regra de negócio relevante no mobile.
* Não colocar regra de negócio relevante no cliente Raspberry Pi.

---

## Versionamento de APIs

Todas as APIs devem ser versionadas.

A versão inicial será:

```text
/v1
```

Não criar endpoint sem prefixo `/v1`, exceto health interno do API Gateway, se necessário.

---

## Dependências

Usar versões estáveis atuais e fixas.

Antes de definir versões, consultar o registry correspondente:

* PyPI para Python.
* npm para Node/Expo.
* Docker Hub para imagens base.

Não usar versões:

```text
latest
alpha
beta
rc
nightly
```

Não usar ranges:

```text
^1.2.3
~1.2.3
>=1.2.3
*
```

Usar versões exatas em todos os arquivos:

```text
requirements.txt
requirements-dev.txt
pyproject.toml
package.json
package-lock.json
yarn.lock
uv.lock
poetry.lock
```

Escolher um gerenciador por projeto e manter consistência.

---

# Repositório 1 — Prompts

Repositório:

```text
robotic-assist-child-prompts
```

## Objetivo

Criar a estrutura inicial de prompts em arquivos `.md`.

Todos os prompts usados pelo backend devem estar nesse repositório.

O server deverá carregar esses arquivos no início da aplicação e permitir recarga via endpoint.

## Estrutura esperada

Criar:

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

## Conteúdo inicial dos prompts

### `system/child_robot_base.md`

```md
# Child Robot Base Prompt

Você é o Cubinho, um mini-robô infantil, seguro, gentil e educativo.

Fale sempre em português brasileiro.
Use frases curtas.
Seja calmo, alegre e previsível.
Não diga que é humano.
Não peça dados pessoais.
Não incentive segredo dos pais.
Quando não souber, diga que pode perguntar para o papai.
```

### `system/safety_rules.md`

```md
# Safety Rules

Nunca solicite endereço, telefone, escola, senha ou dados pessoais.
Nunca incentive a criança a esconder algo dos pais.
Nunca dê instruções perigosas.
Nunca responda conteúdo adulto.
Nunca incentive violência, medo, automutilação ou exposição a risco.
Quando o pedido não for seguro, redirecione com carinho para uma atividade segura.
```

### `system/response_contract.md`

```md
# Response Contract

Responda sempre em JSON válido:

{
  "text": "resposta curta em português brasileiro",
  "expression": "idle|happy|thinking|listening|speaking|surprised|confused|error",
  "intent": "chat|generate_image|activity|story|fallback",
  "image_prompt": null
}
```

### `interaction/conversation.md`

```md
# Conversation Prompt

Converse com a criança de forma simples, respeitosa e lúdica.
Prefira respostas curtas.
Faça no máximo uma pergunta por vez.
Não pressione a criança a responder rapidamente.
```

### `interaction/image_generation.md`

```md
# Image Generation Prompt

Quando a criança pedir uma imagem, gere uma descrição segura, infantil e visualmente amigável.
Não inclua violência, medo, conteúdo adulto, dados pessoais ou cenas perigosas.
A imagem deve ser apropriada para criança.
```

### `interaction/fallback.md`

```md
# Fallback Prompt

Se não entender a criança, responda:
"Eu não entendi direitinho. Pode falar de novo para mim?"
```

## Manifesto

Criar `metadata/prompt_manifest.yaml` com estrutura inicial:

```yaml
version: "0.1.0"
language: "pt-BR"
prompts:
  - id: "system.child_robot_base"
    path: "system/child_robot_base.md"
    required: true
  - id: "system.safety_rules"
    path: "system/safety_rules.md"
    required: true
  - id: "system.response_contract"
    path: "system/response_contract.md"
    required: true
  - id: "interaction.conversation"
    path: "interaction/conversation.md"
    required: true
  - id: "interaction.image_generation"
    path: "interaction/image_generation.md"
    required: true
  - id: "interaction.fallback"
    path: "interaction/fallback.md"
    required: true
```

---

# Repositório 2 — Server

Repositório:

```text
robotic-assist-child-server
```

## Stack obrigatória

Usar:

```text
Python 3.13+
FastAPI
Pydantic
Uvicorn
PostgreSQL
MongoDB
Redis
SQLAlchemy ou SQLModel
Alembic
OpenTelemetry
Pytest
Docker
Docker Compose
```

## Arquitetura

Usar Clean Architecture / Hexagonal Architecture.

Separar:

```text
domain
application
infrastructure
interfaces
config
shared
```

O domínio não deve depender de:

* FastAPI
* banco de dados
* MongoDB
* Redis
* OpenRouter
* ElevenLabs
* AWS
* HTTP
* SDK externo

A aplicação deve depender de portas/interfaces.

A infraestrutura deve implementar adapters concretos.

Controllers HTTP e WebSocket devem apenas converter entrada/saída e chamar casos de uso.

---

## Estrutura esperada do server

```text
.
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── README.md
├── pyproject.toml
├── requirements.txt
├── requirements-dev.txt
├── alembic.ini
├── alembic/
├── gateway/
│   └── nginx.conf
├── otel/
│   └── otel-collector-config.yaml
├── src/
│   └── robotic_assist_child_server/
│       ├── main.py
│       ├── config/
│       │   ├── settings.py
│       │   └── providers.py
│       ├── domain/
│       │   ├── entities/
│       │   ├── value_objects/
│       │   ├── enums/
│       │   └── events/
│       ├── application/
│       │   ├── ports/
│       │   ├── use_cases/
│       │   └── services/
│       ├── infrastructure/
│       │   ├── auth/
│       │   ├── database/
│       │   ├── mongodb/
│       │   ├── redis/
│       │   ├── openrouter/
│       │   ├── elevenlabs/
│       │   ├── telemetry/
│       │   ├── prompts/
│       │   └── repositories/
│       ├── interfaces/
│       │   ├── http/
│       │   │   └── v1/
│       │   └── websocket/
│       └── shared/
│           ├── logging/
│           ├── datetime.py
│           ├── errors.py
│           └── ids.py
└── tests/
```

---

## Configuração

Todas as configurações devem vir de:

1. AWS Secrets Manager.
2. AWS Systems Manager Parameter Store.
3. Variáveis de ambiente.
4. `.env` como fallback local.

Na Fase 1, implementar funcionalmente:

```text
EnvironmentConfigProvider
DotEnvConfigProvider
CompositeConfigProvider
```

Deixar preparado:

```text
AwsSecretsManagerConfigProvider
AwsParameterStoreConfigProvider
```

## `.env.example`

Criar `.env.example` com:

```env
APP_NAME=robotic-assist-child-server
APP_ENV=dev
APP_VERSION=0.1.0
LOG_LEVEL=INFO

SERVER_HOST=0.0.0.0
SERVER_PORT=8000

DATABASE_URL=postgresql+psycopg://robot_user:robot_password@postgres:5432/robotic_assist_child

MONGODB_URI=mongodb://mongo:27017/robotic_assist_child
REDIS_URL=redis://redis:6379/0

PROMPTS_REPOSITORY_PATH=/app/prompts
PROMPTS_AUTOLOAD=true

OPENROUTER_API_KEY=
OPENROUTER_BASE_URL=https://openrouter.ai/api/v1
OPENROUTER_CHAT_MODEL=
OPENROUTER_IMAGE_MODEL=

ELEVENLABS_API_KEY=
ELEVENLABS_BASE_URL=https://api.elevenlabs.io
ELEVENLABS_VOICE_ID=
ELEVENLABS_TTS_MODEL=
ELEVENLABS_STT_MODEL=

JWT_SECRET_KEY=change_me
JWT_ALGORITHM=HS256
JWT_ACCESS_TOKEN_EXPIRE_MINUTES=60
JWT_REFRESH_TOKEN_EXPIRE_MINUTES=1440

AWS_REGION=sa-east-1
AWS_SECRETS_MANAGER_ENABLED=false
AWS_PARAMETER_STORE_ENABLED=false

CORS_ALLOW_ORIGINS=http://localhost:19006,http://localhost:8081,http://localhost:8080
CORS_ALLOW_CREDENTIALS=true

OTEL_SERVICE_NAME=robotic-assist-child-server
OTEL_SERVICE_VERSION=0.1.0
OTEL_ENVIRONMENT=dev
OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4317
OTEL_EXPORTER_OTLP_PROTOCOL=grpc
OTEL_TRACES_EXPORTER=otlp
OTEL_METRICS_EXPORTER=otlp
OTEL_LOGS_EXPORTER=otlp

AUTO_RUN_MIGRATIONS=false

POSTGRES_EXTERNAL=false
MONGODB_EXTERNAL=false
REDIS_EXTERNAL=false
```

---

## PostgreSQL

Usar PostgreSQL como base relacional principal.

Tabelas mínimas:

```text
users
devices
sessions
interactions
interaction_responses
user_settings
auth_tokens
audit_events
```

Usuários devem suportar papéis:

```text
admin
parent
child
device
```

Um `parent` pode gerenciar um ou mais perfis `child`.

Um `device` pode estar vinculado a um `child`.

Uma sessão deve estar associada a:

```text
user_id
device_id opcional
client_type
session_id
```

`client_type` deve aceitar:

```text
mobile
raspberry
web
test
```

---

## MongoDB

Usar MongoDB como base de memória/contexto.

Collections mínimas:

```text
user_memories
memory_events
conversation_summaries
model_context_snapshots
```

Cada memória deve conter:

```json
{
  "memory_id": "mem_123",
  "user_id": "usr_123",
  "session_id": "session_123",
  "memory_type": "interest",
  "content": "A criança gosta de dinossauros.",
  "confidence": 0.8,
  "source": "user_interaction",
  "created_at": "2026-06-07T20:00:00Z",
  "updated_at": "2026-06-07T20:00:00Z",
  "expires_at": null
}
```

Tipos iniciais de memória:

```text
preference
routine
restriction
interest
interaction_summary
safety_note
parent_instruction
```

Criar interfaces:

```text
MemoryRepository
MemoryRetriever
MemoryUpdater
MemoryPolicy
```

Implementações iniciais:

```text
MongoMemoryRepository
SimpleMemoryRetriever
RuleBasedMemoryUpdater
```

---

## Redis

Usar Redis para:

* cache de prompts carregados;
* cache de sessão ativa;
* fila assíncrona simples;
* locks leves;
* rate limiting futuro;
* pub/sub futuro para WebSocket.

Na Fase 1, Redis pode ser simples, mas deve estar no `docker-compose.yml`.

---

## Autenticação inicial

Implementar endpoints:

```http
POST /v1/auth/register
POST /v1/auth/login
POST /v1/auth/refresh
POST /v1/auth/logout
GET  /v1/auth/me
```

Usar:

* username ou email;
* senha com hash seguro;
* JWT access token;
* JWT refresh token.

Nunca armazenar senha em texto puro.

Usar algoritmo moderno, preferencialmente `argon2` ou `bcrypt`.

Response de login:

```json
{
  "access_token": "jwt",
  "refresh_token": "jwt",
  "token_type": "bearer",
  "expires_in": 3600
}
```

---

## Sessões

Criar controle de sessões.

Toda interação deve possuir:

```text
session_id
user_id
client_type
device_id opcional
```

O backend deve ser capaz de diferenciar usuários e adaptar o contexto por usuário.

---

## Memória por usuário

Toda interação deve seguir este fluxo:

```text
1. Receber interação.
2. Identificar user_id e session_id.
3. Buscar memórias relevantes do usuário no MongoDB.
4. Buscar preferências/configurações no PostgreSQL.
5. Compor prompt final.
6. Chamar provider de conversação.
7. Validar resposta.
8. Retornar resposta ao cliente.
9. Persistir interação de forma assíncrona.
10. Avaliar se alguma memória deve ser criada, atualizada ou expirada.
```

Na Fase 1, o atualizador de memória pode ser baseado em regras.

Exemplo:

Se o usuário disser:

```text
Eu gosto de dinossauros.
```

Criar ou atualizar memória:

```json
{
  "user_id": "usr_123",
  "memory_type": "interest",
  "content": "A criança gosta de dinossauros.",
  "confidence": 0.8,
  "source": "user_interaction",
  "created_at": "2026-06-07T20:00:00Z"
}
```

Evitar salvar informações sensíveis desnecessárias.

---

## Prompts

O server deve carregar prompts `.md` a partir de:

```env
PROMPTS_REPOSITORY_PATH=/app/prompts
```

Cada prompt carregado deve conter:

```text
id
path
content
content_hash
loaded_at
```

Endpoints:

```http
GET  /v1/prompts
GET  /v1/prompts/{prompt_id}
POST /v1/prompts/reload
```

`POST /v1/prompts/reload` deve recarregar prompts sem reiniciar a aplicação.

---

## Interações

Implementar inicialmente interação por texto:

```http
POST /v1/interactions/text
GET  /v1/interactions/{interaction_id}
```

Request:

```json
{
  "session_id": "session_123",
  "client_type": "mobile",
  "input_text": "Olá, robô!",
  "metadata": {
    "device_id": "dev_local",
    "locale": "pt-BR"
  }
}
```

Response:

```json
{
  "interaction_id": "int_123",
  "session_id": "session_123",
  "status": "accepted",
  "assistant_text": "Olá! Que bom falar com você.",
  "expression": "happy",
  "intent": "chat",
  "created_at": "2026-06-07T20:00:00Z"
}
```

Enums:

```text
intent:
- chat
- generate_image
- activity
- story
- fallback

expression:
- idle
- happy
- thinking
- listening
- speaking
- surprised
- confused
- error
```

---

## OpenRouter

Encapsular atrás da interface:

```text
ConversationModelProvider
```

Implementações:

```text
OpenRouterConversationProvider
FakeConversationProvider
```

Na Fase 1, implementar `FakeConversationProvider` funcional.

Deixar `OpenRouterConversationProvider` preparado.

O provider deve retornar:

```json
{
  "text": "Olá! Eu sou o Cubinho.",
  "expression": "happy",
  "intent": "chat",
  "image_prompt": null
}
```

Prompt engineering deve ficar no backend, em serviço específico:

```text
PromptComposer
```

---

## ElevenLabs

Encapsular atrás das interfaces:

```text
SpeechToTextProvider
TextToSpeechProvider
```

Implementações previstas:

```text
ElevenLabsSpeechToTextProvider
ElevenLabsTextToSpeechProvider
FakeSpeechToTextProvider
FakeTextToSpeechProvider
```

Na Fase 1, não é obrigatório gerar áudio real.

Mas a arquitetura deve estar preparada para:

* receber áudio;
* transcrever áudio;
* gerar resposta;
* gerar áudio de resposta;
* enviar resposta ao cliente;
* permitir animação da boca no cliente.

Áudios não devem ser armazenados.

---

## Safety Guard

Criar serviço:

```text
SafetyGuard
```

Métodos:

```text
validate_input_text
validate_assistant_response
```

Regras iniciais:

* não solicitar dados pessoais;
* não incentivar segredo dos pais;
* não responder com conteúdo adulto;
* não orientar atividades perigosas;
* redirecionar de forma gentil quando necessário.

---

## OpenTelemetry

Instrumentar desde a Fase 1:

```text
traces
metrics
logs
```

Instrumentar:

* FastAPI;
* HTTP client;
* PostgreSQL;
* MongoDB;
* Redis;
* OpenRouter;
* ElevenLabs;
* carregamento de prompts;
* composição de prompt;
* persistência assíncrona;
* atualização de memória.

Criar spans:

```text
prompt.load_all
prompt.reload
interaction.handle_text
memory.retrieve
memory.update
conversation.generate
safety.validate_input
safety.validate_response
repository.interaction.save
```

Métricas iniciais:

```text
http.server.request.duration
interaction.request.count
interaction.error.count
conversation.provider.duration
conversation.provider.error.count
prompt.reload.count
prompt.loaded.count
memory.retrieve.duration
memory.update.count
```

Logs devem ser JSON compatíveis com OpenTelemetry.

Cada log deve conter, quando aplicável:

```text
timestamp
severity_text
severity_number
service_name
service_version
deployment_environment
trace_id
span_id
session_id
user_id
device_id
interaction_id
event_name
message
attributes
```

Não logar:

```text
senhas
tokens
chaves de API
áudio
dados sensíveis da criança
```

---

## Healthchecks

Criar:

```http
GET /v1/health/live
GET /v1/health/ready
GET /v1/health/startup
GET /v1/health
```

### Liveness

`/v1/health/live`

Não deve depender de banco externo.

Resposta:

```json
{
  "status": "ok",
  "check": "live",
  "checked_at": "2026-06-07T20:00:00Z"
}
```

### Readiness

`/v1/health/ready`

Verificar:

```text
PostgreSQL
MongoDB
Redis
prompts carregados
configuração mínima
```

Se falhar, retornar HTTP 503.

Resposta saudável:

```json
{
  "status": "ok",
  "check": "ready",
  "dependencies": {
    "postgresql": "ok",
    "mongodb": "ok",
    "redis": "ok",
    "prompts": "ok"
  },
  "checked_at": "2026-06-07T20:00:00Z"
}
```

### Startup

`/v1/health/startup`

Verificar:

```text
config carregada
migrações aplicadas
prompts carregados
workers assíncronos iniciados
OpenTelemetry inicializado
```

Enquanto não estiver pronto, retornar HTTP 503.

---

## Dockerfile

Gerar Dockerfile leve e eficiente.

Requisitos:

* imagem `python:3.13-slim` ou superior;
* multi-stage build quando fizer sentido;
* usuário não-root;
* cache eficiente;
* sem ferramentas de build na imagem final quando possível;
* `PYTHONDONTWRITEBYTECODE=1`;
* `PYTHONUNBUFFERED=1`;
* `HEALTHCHECK`;
* compatível com `linux/amd64` e `linux/arm64`.

---

## Docker Compose

Criar `docker-compose.yml` no server com:

```text
server
postgres
mongodb
redis
otel-collector
api-gateway
```

O API Gateway pode ser inicialmente Nginx.

Portas sugeridas:

```text
server: 8000
api-gateway: 8080
postgres: 5432
mongodb: 27017
redis: 6379
otel-collector: 4317/4318
```

O compose deve permitir usar dependências externas via variáveis:

```env
POSTGRES_EXTERNAL=false
MONGODB_EXTERNAL=false
REDIS_EXTERNAL=false
```

Documentar no README como usar dependências externas.

---

## API Gateway

Usar API Gateway entre backend e clientes.

Para Fase 1, usar Nginx.

Fluxo:

```text
mobile/rpi -> api-gateway -> server
```

O gateway deve:

* fazer proxy para `/v1`;
* suportar WebSocket;
* preparar CORS;
* preservar headers de tracing/correlação;
* permitir futuro rate limiting;
* permitir futuro TLS.

Headers importantes:

```text
traceparent
tracestate
x-request-id
x-correlation-id
authorization
```

Criar health do gateway:

```http
GET /gateway/health
```

---

## WebSocket

Implementar:

```http
WS /v1/ws/sessions/{session_id}
```

Eventos previstos:

```text
state
assistant_text
audio_url
image_url
mouth_cues
face_expression
error
```

Evento exemplo:

```json
{
  "type": "state",
  "session_id": "session_123",
  "value": "thinking",
  "created_at": "2026-06-07T20:00:00Z"
}
```

---

# Repositório 3 — Mobile

Repositório:

```text
robotic-assist-child-mobile
```

## Stack

Usar a implementação mais simples possível.

Preferência:

```text
Expo React Native
```

O mobile deve ser mínimo.

Não colocar regra de negócio relevante no mobile.

Não chamar OpenRouter.

Não chamar ElevenLabs.

Não acessar banco.

Não acessar MongoDB.

Não acessar Redis.

O mobile deve chamar apenas o API Gateway.

## Estrutura esperada

```text
.
├── app.json
├── package.json
├── package-lock.json
├── README.md
├── .env.example
└── src/
    ├── app/
    ├── components/
    │   ├── RobotFace.tsx
    │   ├── RobotEyes.tsx
    │   ├── RobotMouth.tsx
    │   └── LowerPanel.tsx
    ├── services/
    │   ├── apiClient.ts
    │   └── websocketClient.ts
    └── types/
        └── robotEvents.ts
```

## Variáveis

```env
EXPO_PUBLIC_API_BASE_URL=http://localhost:8080
EXPO_PUBLIC_WS_BASE_URL=ws://localhost:8080
```

## Funcionalidades da Fase 1

Implementar:

* tela cheia;
* rosto básico;
* olhos piscando aleatoriamente;
* boca simples;
* painel inferior reservado;
* chamada para `/v1/health`;
* tela simples de login ou configuração de token;
* envio de texto para `/v1/interactions/text`;
* conexão WebSocket básica.

---

# Repositório 4 — Raspberry Pi

Repositório:

```text
robotic-assist-child-rpi
```

## Stack

Usar:

```text
Python 3.13+
```

Criar apenas o esqueleto na Fase 1.

## Estrutura esperada

```text
.
├── README.md
├── pyproject.toml
├── requirements.txt
├── requirements-dev.txt
├── .env.example
└── src/
    └── robotic_assist_child_rpi/
        ├── main.py
        ├── display/
        ├── audio/
        ├── client/
        └── config/
```

## Funcionalidades da Fase 1

Implementar:

* config por `.env`;
* client HTTP básico;
* client WebSocket básico;
* placeholder para display;
* placeholder para áudio;
* chamada para `/v1/health`;
* autenticação/token configurável;
* nenhum acesso direto a OpenRouter, ElevenLabs, PostgreSQL, MongoDB ou Redis.

---

# Testes

Criar testes mínimos no server para:

```text
health live
health ready
health startup
prompt loading
prompt reload
auth register
auth login
interaction text
memory repository
async persistence basic flow
```

No mobile, criar ao menos testes ou validações básicas para:

```text
renderização do rosto
configuração de API base URL
cliente HTTP
```

No RPI, criar ao menos testes para:

```text
config loading
HTTP client
WebSocket client
```

---

# Referência conceitual

Usar como referência conceitual o projeto:

```text
https://github.com/JPSAUD501/Ditado
```

A referência deve ser interpretada assim:

* separar UI/captura de áudio da orquestração;
* tratar interpretação de texto como fluxo central;
* manter interfaces claras entre captura, processamento e saída;
* usar testes automatizados;
* manter estrutura organizada para evolução.

Não copiar código diretamente.

Não transformar este projeto em Electron.

---

# Critérios de aceite da Fase 1

A Fase 1 estará pronta quando:

1. Os quatro repositórios tiverem estrutura inicial criada.
2. Tudo estiver na branch `develop`.
3. O repositório de prompts tiver arquivos `.md` organizados.
4. O server rodar com `docker compose up --build`.
5. O server usar Python 3.13+.
6. Todas as dependências estiverem fixas.
7. O server tiver Dockerfile leve, usuário não-root e healthcheck.
8. O compose subir server, PostgreSQL, MongoDB, Redis, OpenTelemetry Collector e API Gateway.
9. O server tiver `/v1/health/live`, `/v1/health/ready`, `/v1/health/startup` e `/v1/health`.
10. O server carregar prompts `.md`.
11. `/v1/prompts/reload` recarregar prompts sem reiniciar.
12. O server tiver autenticação inicial.
13. O server suportar usuários, dispositivos e sessões.
14. Memórias serem separadas por usuário.
15. Interações carregarem memórias relevantes.
16. Interações atualizarem memórias quando necessário.
17. Logs serem JSON compatíveis com OpenTelemetry.
18. Traces e métricas estarem configurados.
19. O API Gateway suportar HTTP e WebSocket.
20. O mobile Expo abrir em tela cheia e chamar o gateway.
21. O mobile renderizar rosto básico com olhos piscando.
22. O RPI client ter skeleton funcional.
23. Nenhuma secret estar hardcoded.
24. Todos os payloads de API usarem `snake_case`.
25. Todas as datas de API usarem RFC3339 UTC.
26. Os READMEs explicarem como rodar localmente.

---

# Ordem de execução recomendada para o agente

Execute nesta ordem:

```text
1. robotic-assist-child-prompts
2. robotic-assist-child-server
3. robotic-assist-child-mobile
4. robotic-assist-child-rpi
```

Para cada repositório:

```text
1. Verifique se está na branch develop.
2. Crie ou atualize apenas os arquivos necessários.
3. Não faça commit automaticamente, a menos que seja solicitado.
4. Ao final, informe os arquivos criados/alterados.
5. Informe os comandos para testar.
```

---

# Comandos esperados para validação local

## Server

```bash
docker compose up --build
```

```bash
curl http://localhost:8080/v1/health/live
```

```bash
curl http://localhost:8080/v1/health/ready
```

```bash
curl http://localhost:8080/v1/prompts
```

```bash
curl -X POST http://localhost:8080/v1/prompts/reload
```

```bash
curl -X POST http://localhost:8080/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "parent@example.com",
    "password": "change_me",
    "role": "parent"
  }'
```

```bash
curl -X POST http://localhost:8080/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "parent@example.com",
    "password": "change_me"
  }'
```

```bash
curl -X POST http://localhost:8080/v1/interactions/text \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ACCESS_TOKEN_HERE" \
  -d '{
    "session_id": "session_local",
    "client_type": "test",
    "input_text": "Olá, Cubinho!",
    "metadata": {
      "device_id": "local_test",
      "locale": "pt-BR"
    }
  }'
```

---

# Observação final ao agente

Priorize fundação bem feita em vez de features avançadas.

Não implemente STT real, TTS real ou geração de imagem real nesta fase, mas deixe as interfaces prontas.

Antes de finalizar, revise se:

* providers externos podem ser trocados sem alterar casos de uso;
* clientes mobile e Raspberry Pi usam o mesmo backend;
* memórias são por usuário;
* observabilidade está presente desde o início;
* API Gateway está no caminho entre clientes e backend;
* o projeto continua simples o suficiente para evoluir.

# 🦾 Homelab AI Ops

Laboratório pessoal de infraestrutura Linux, automação, observabilidade e AI Ops self-hosted.  
Construído do zero com hardware real, do bare metal ao time de agentes de IA.

---

## 🎯 Objetivo

Este projeto foi criado para estudo prático e evolução profissional em:

- Infraestrutura Linux (headless server)
- Docker e Docker Compose
- Observabilidade (Prometheus, Grafana, Loki)
- Automação operacional com n8n
- AI Ops com agentes LLM especializados
- Redes, reverse proxy e acesso remoto seguro

---

## 🖥️ Hardware

| Componente | Especificação |
|------------|---------------|
| CPU | Intel i7-4790 (4C/8T) |
| RAM | 32 GB DDR3 |
| SSD | Kingston A400 480 GB (SO + containers) |
| HDD | 500 GB (dados e volumes) |
| GPU | Gigabyte Windforce R9 290/390 — 8 GB VRAM (GCN 2.0) |
| Placa-mãe | ASRock Fatal1ty Z97 Killer |
| SO | Zorin OS 18.1 (headless) |
| Hostname | `lord` |
| IP local | `----------` |
| Tailscale |  `----------` |

> **Nota GPU:** A R9 290/390 usa arquitetura GCN 2.0, sem suporte ao ROCm.  
> Driver `radeon` (estável). Inferência Ollama roda na CPU — com 32 GB de RAM os modelos cabem tranquilamente.

---

## 🏗️ Arquitetura

```
Internet / Tailscale
       ↓
Traefik v2.11 (reverse proxy — porta 80)
       ↓
Docker Network: homelab
       ↓
┌──────────────────────────────────────────┐
│  Serviços    │  Observabilidade          │
│  Portainer   │  Prometheus               │
│  code-server │  Grafana                  │
│              │  Loki                     │
│              │  Node Exporter            │
├──────────────┴───────────────────────────┤
│  AI                                       │
│  Open WebUI  ←→  Ollama (CPU)            │
│  OpenClaw Gateway (porta 18789)          │
│  Time de Agentes (6 agentes)             │
├───────────────────────────────────────────┤
│  Automação                                │
│  n8n  ←→  PostgreSQL                     │
│  Workflow: Claw Memory (webhook POST)    │
└───────────────────────────────────────────┘
```

---

## 🐳 Containers rodando

| Container | URL / Porta | Função |
|-----------|-------------|--------|
| `traefik` | `:80`, `:8080` | Reverse proxy |
| `portainer` | `portainer.homelab.local` | Gerenciamento Docker |
| `prometheus` | `prometheus.homelab.local` | Coleta de métricas |
| `node-exporter` | `:9100` | Métricas do SO |
| `grafana` | `grafana.homelab.local` | Dashboards |
| `loki` | `:3100` | Agregação de logs |
| `open-webui` | `openwebui.homelab.local` / `:3000` | Interface IA |
| `code-server` | `:8443` | VS Code Web |
| `n8n` | `:5678` | Automação de workflows |
| `postgresql` | `:5432` | Memória persistente do Claw |

---

## 🤖 AI Ops — Time de Agentes OpenClaw

O coração do projeto. Um time de 6 agentes LLM especializados, coordenados pelo **Claw 🦞**, rodando via [OpenClaw](https://openclaw.ai) com acesso pelo Telegram.

### Agentes

| Agente | Emoji | Especialidade | Modelo |
|--------|-------|---------------|--------|
| **Claw** (main) | 🦞 | Coordenador geral | `openrouter/meta-llama/llama-3.3-70b-instruct:free` |
| loki-agent | 🔍 | Logs e monitoramento | `openrouter/nvidia/nemotron-3-super-120b:free` |
| davy-agent | 🐳 | Docker e containers | `openrouter/openai/gpt-oss-120b:free` |
| shade-agent | 🔐 | Segurança | `openrouter/nvidia/nemotron-3.5-content-safety:free` |
| nexus-agent | 🤖 | IA e modelos | `openrouter/moonshotai/kimi-k2.6:free` |
| forge-agent | ⚙️ | Automação e scripts | `openrouter/qwen/qwen3-coder:free` |

### Providers configurados

- **OpenRouter** — modelos gratuitos de 70B+ para os agentes
- **Groq** — fallback rápido (`llama-3.3-70b-versatile`)
- **Ollama local** — `llama3.2:3b` e `llama3.1:8b` rodando na CPU

### Acesso

- **Telegram:** `@Hugowenner_bot`
- **Gateway:** porta `18789` (loopback, gerenciado via systemd)

---

## ⚙️ Automação — n8n + PostgreSQL

Infraestrutura de automação com memória persistente.

### Workflow: Claw Memory

Pipeline que salva resumos de conversas do Claw no PostgreSQL:

```
Webhook POST /claw-memory
        ↓
  n8n (Postgres Insert)
        ↓
  tabela: conversations
  campos: summary, tags, created_at
```

**Endpoint:** `http://192.168.0.50:5678/webhook/claw-memory`

---

## 📊 Observabilidade

- **Prometheus** — scrape a cada 15s nos targets
- **Node Exporter** — métricas de CPU, RAM, disco e rede
- **Grafana** — dashboard Node Exporter Full (#1860) importado
- **Loki** — indexação e agregação de logs dos containers

---

## 🌐 Acesso Remoto

- **SSH:** `hugowenner@192.168.0.50` (principal)
- **Tailscale:** `100.74.77.110` (acesso remoto seguro)
- **RDP:** Remmina (eventual)

---

## 📁 Estrutura do projeto

```
~/homelab/
├── services/
│   ├── .env
│   ├── traefik/
│   └── portainer/
├── observability/
│   ├── prometheus/
│   ├── grafana/
│   └── loki/
├── automation/
│   ├── scripts/
│   ├── n8n/
│   └── agents/
│       ├── loki/
│       ├── davy/
│       ├── shade/
│       ├── nexus/
│       └── forge/
├── ai/
│   ├── ollama/
│   └── open-webui/
└── data/
    ├── volumes/
    └── backups/
```

---

## 🗺️ Roadmap

- [x] Servidor Linux headless configurado
- [x] Docker + Docker Compose
- [x] Traefik reverse proxy + DNS local
- [x] Stack de observabilidade (Prometheus + Grafana + Loki)
- [x] Open WebUI + OpenRouter (modelos gratuitos)
- [x] OpenClaw + Time de Agentes (6 agentes especializados)
- [x] Ollama — inferência local na CPU
- [x] n8n + PostgreSQL — memória persistente do Claw
- [ ] Alertas Telegram via n8n (container down)
- [ ] Tailscale — configuração avançada
- [ ] Pipeline AI Ops completa (coleta → análise → alerta → remediação)
- [ ] GitOps leve
- [ ] Auto-remediation controlada
- [ ] Integração multi-host

---

## 🧠 Lições aprendidas

- **Traefik v3** incompatível com Docker API 29.5.x — usar **v2.11** com `DOCKER_API_VERSION=1.40`
- **OpenClaw** requer Node.js 22.19+ — instalar via **nvm**, não apt
- **`openrouter/auto`** seleciona modelos pagos automaticamente — especificar modelo `:free` explicitamente
- **Modelos 8B locais** têm instruction-following fraco para agentes com personalidade — preferir 70B via API
- **Volumes Docker** precisam de `chown` para os UIDs internos dos containers (Grafana: 472, Prometheus: 65534, Loki: 10001)
- **GPU R9 290/390** não suporta ROCm (GCN 2.0) — inferência na CPU com 32 GB de RAM funciona bem
- **n8n webhook em produção:** `/webhook/<path>` | em teste: `/webhook-test/<path>`

---

## 📄 Licença

Projeto pessoal de aprendizado. Fique à vontade para usar como referência.

---

*Servidor: `lord` @ `Lord2` | Última atualização: Jun/2026*

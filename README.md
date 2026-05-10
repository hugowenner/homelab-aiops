# Homelab AI Ops

Laboratório pessoal de infraestrutura Linux, automação, observabilidade e AI Ops self-hosted.

## Objetivo

Este projeto foi criado para estudo prático e evolução profissional em:

* Infraestrutura Linux
* Docker e Docker Compose
* Observabilidade
* Automação operacional
* DevOps
* AI Ops
* Redes
* Reverse Proxy
* Containers
* Ambientes self-hosted
* Troubleshooting real

O foco principal do ambiente é construir uma infraestrutura modular, leve e totalmente funcional utilizando hardware limitado.

---

# Hardware

| Recurso             | Especificação      |
| ------------------- | ------------------ |
| Host                | Dell OptiPlex 3020 |
| CPU                 | 4 cores            |
| RAM                 | 8 GB               |
| Sistema Operacional | Zorin OS 18        |

---

# Stack Principal

## Infraestrutura

* Linux
* Docker
* Docker Compose
* Traefik v2
* PostgreSQL

## Observabilidade

* Prometheus
* Grafana
* cAdvisor
* node-exporter
* Uptime Kuma
* Dozzle

## Automação

* n8n
* Workflows automatizados
* Alertas operacionais

## AI Local

* Ollama
* Open WebUI
* qwen2.5:3b

---

# Arquitetura

```txt
Browser
   ↓
Traefik Reverse Proxy
   ↓
Docker Network (proxy)
   ↓
Containers e serviços
```

---

# Serviços do ambiente

| Serviço     | Função                |
| ----------- | --------------------- |
| Traefik     | Reverse proxy         |
| Homepage    | Dashboard do ambiente |
| Portainer   | Gerenciamento Docker  |
| n8n         | Automação             |
| PostgreSQL  | Banco do n8n          |
| Ollama      | Inferência local      |
| Open WebUI  | Interface IA          |
| Grafana     | Dashboards            |
| Prometheus  | Métricas              |
| cAdvisor    | Métricas Docker       |
| Uptime Kuma | Status monitor        |
| Dozzle      | Logs Docker           |
| code-server | VS Code Web           |

---

# AI Ops

O ambiente possui uma pipeline inicial de AI Ops utilizando:

* Docker
* Scripts shell
* n8n
* Ollama
* Discord alerts

## Fluxo atual

```txt
Docker
↓
Relatórios JSON
↓
n8n
↓
Ollama
↓
Classificação inteligente
↓
Discord alert
```

## Funcionalidades

* análise automática de containers
* classificação INFO/WARNING/CRITICAL
* análise de logs
* análise de CPU e RAM
* redução de ruído operacional
* alertas automáticos

---

# Estrutura do projeto

```txt
homelab/
├── backups/
├── configs/
├── containers/
├── docs/
├── reports/
├── scripts/
├── volumes/
└── vms/
```

---

# Tecnologias estudadas

* Linux Administration
* Docker Networking
* Reverse Proxy
* Virtualização
* VPN
* Monitoramento
* Observabilidade
* AI aplicada à infraestrutura
* Persistência
* Troubleshooting
* Infraestrutura self-hosted

---

# Próximos passos

* centralização de logs com Loki
* dashboards AI Ops
* backup automatizado
* deploy automatizado
* análise inteligente de incidentes
* auto-remediation controlada
* GitOps leve
* integração multi-host

---

# Objetivo profissional

Consolidar experiência prática em:

* Infraestrutura Linux
* DevOps
* Observabilidade
* Containers
* Automação
* AI Ops
* Ambientes corporativos
* Troubleshooting real

---

# Status do projeto

Em evolução contínua.

Projeto focado em aprendizado prático, arquitetura moderna e automação operacional.

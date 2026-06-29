<div align="center">
  <h1>🧪 Homelab — Infraestrutura Self-Hosted</h1>
  <h3>Proxmox &nbsp;•&nbsp; LXC + VMs &nbsp;•&nbsp; Rede &nbsp;•&nbsp; Automação &nbsp;•&nbsp; Observabilidade</h3>
</div>

## 📖 Visão Geral

Ambiente de laboratório que rodo em casa para praticar e operar infraestrutura real — não simulada. Uso um hypervisor **Proxmox** com serviços segmentados em containers **LXC** e máquinas virtuais, cobrindo desde rede e ingress seguro até automação de workflows e aplicações com banco de dados.

O objetivo é consolidar fundamentos de **DevOps/SRE** na prática: provisionamento, isolamento de serviços, acesso remoto seguro, automação e monitoramento.

> 📐 *Diagrama*

```
                    ┌─────────────────────────────┐
                    │      Proxmox VE (Host)      │
                    ├─────────────────────────────┤
   Containers LXC   │  Pi-hole        (DNS)       │
                    │  Cloudflare Tunnel (ingress)│
                    │  n8n            (automação) │
                    │  Dolibarr       (ERP/CRM)   │
                    │  Anytype        (workspace) │
                    ├─────────────────────────────┤
   Máquinas VM      │  Hermes Agent   (agente)    │
                    └─────────────────────────────┘
```

## ⚙️ Serviços

| Serviço | Tipo | Função | Competência demonstrada |
|---|---|---|---|
| **Proxmox VE** | Host | Hypervisor que orquestra LXC e VMs | Virtualização, gestão de hypervisor |
| **Pi-hole** | LXC | DNS local com bloqueio de anúncios/rastreadores | Rede, DNS |
| **Cloudflare Tunnel** | LXC | Acesso remoto seguro sem abrir portas no roteador | Zero-trust, ingress seguro |
| **n8n** | LXC | Automação de workflows e integrações | Automação, orquestração |
| **Dolibarr** | LXC | ERP/CRM self-hosted com banco de dados | Deploy de aplicação, persistência |
| **Anytype** | LXC | Workspace/knowledge base self-hosted | Self-hosting, manutenção de serviço |
| **Hermes Agent** | VM | Automação de monitoramento de e-mail: alertas de SLA, dashboard e relatórios *(ver destaque abaixo)* | Python, automação, observabilidade, integrações |

## 🤖 Destaque: Hermes Agent

Sistema de automação em **Python** que roda em VM e opera uma caixa de suporte de e-mail de ponta a ponta — do recebimento ao relatório gerencial. É o projeto que mais me aproximou de práticas de **SRE**: monitoramento contínuo, alertas de SLA e rotação de plantão.

**O que ele faz:**
- **Monitoramento em tempo real (IMAP):** processa e-mails novos via cron com controle *idempotente* por UID — o ponteiro só avança após a gravação ser confirmada, evitando registros perdidos ou duplicados.
- **Classificação automática:** filtra spam e mensagens de teste por regras, categoriza por área e determina o status do atendimento (respondido, pendente, aguardando retorno).
- **Alertas de SLA:** em horário comercial, dispara aviso via WhatsApp após 2h sem resposta e repete a cada 1h enquanto o caso segue pendente.
- **Gestão de plantão:** rotação automática do responsável da semana, no estilo *on-call*.
- **Logging e dashboard:** registra os atendimentos em Google Sheets (via API) e alimenta um dashboard web com gráficos de volume, status, categorias e tempo médio de atendimento.
- **Relatórios automatizados:** envia relatórios em HTML — diário, semanal e mensal — por e-mail.
- **Resiliência:** refresh automático de token OAuth, caminho de *fallback* para envio e tratamento de e-mails órfãos.

**Escala:** processa ~100+ e-mails/mês de uma equipe de suporte.

**Stack:** Python · IMAP · Google Sheets API · integração WhatsApp · cron · HTML/JS

<img width="935" height="521" alt="image" src="https://github.com/user-attachments/assets/463e6e5b-5017-45ce-9e57-8789484f3a65" />

> 🔒 Versão pública sanitizada — sem dados internos, identificadores de pessoas ou métricas individuais.

> 📎 Repositório dedicado: https://github.com/Sheila724/hermes

## 🛠️ Stack

<div align="center">
  <img src="https://skillicons.dev/icons?i=proxmox,linux,docker,cloudflare" />
</div>

- **Virtualização:** Proxmox VE (LXC + KVM/VMs)
- **Sistema:** Linux
- **Rede / Acesso:** Pi-hole, Cloudflare Tunnel
- **Automação:** n8n
- **Aplicações:** Dolibarr, Anytype
- **Observabilidade:** Prometheus + Grafana *(já configurados no lab; removidos temporariamente por capacidade — re-deploy planejado)*

## 📚 O que aprendi


- Segmentar serviços em LXC para isolar falhas e facilitar backup/restore
- Expor serviços internos com segurança via Cloudflare Tunnel, sem port forwarding
- Versionar e automatizar tarefas repetitivas com n8n
- Gerenciar capacidade do host: priorizar serviços e remover stack de monitoramento (Prometheus + Grafana) quando o recurso ficou escasso — uma decisão real de *capacity management*


## 🚧 Próximos passos

- [ ] Re-deploy do stack de observabilidade (Prometheus + Grafana) com limite de recursos ajustado
- [ ] Documentar processo de backup/restore

---

<div align="center">
  <i>🚀 Construindo habilidades reais de infraestrutura — um serviço por vez.</i>
</div>

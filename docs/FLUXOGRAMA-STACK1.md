# 📊 Stack 1 — Plenus Chatbot (WhatsApp + Dify)

> **Plenus Advocacia** — Documentação de Infraestrutura  
> **Data:** 05/04/2026  
> **Status:** 🟢 OPERACIONAL

---

## 🏗️ Fluxograma Arquitetural

```mermaid
graph TB
    subgraph "STACK 1: PLENUS CHATBOT — WhatsApp Atendimento Automático"
        WA[📱 WhatsApp Cliente<br/>+55 34 99727-9291]
        
        subgraph "Gateway WhatsApp"
            EV[🔌 Evolution API v1.8.6<br/>:8081 | plenus-evolution<br/>container Docker]
        end
        
        subgraph "Bot FastAPI"
            BOT[⚡ FastAPI Bot<br/>:5000 | systemd enabled<br/>35MB RAM | plenus-chatbot.service]
        end
        
        subgraph "Inteligência"
            DIFY[🧠 Dify 1.13.3<br/>https://chatbot.advocaciaplenus.com<br/>12 containers Docker]
            LLM[🤖 LLM: groq/llama-3.1-8b-instant<br/>via Dify workflow]
        end
        
        subgraph "Integrações Opcionais"
            RAG[📚 Plenus RAG API<br/>:8000]
            PGSQL[(PostgreSQL<br/>CRM + Prazos)]
        end
        
        WA -->|Mensagem| EV
        EV -->|Webhook MESSAGES_UPSERT| BOT
        BOT -->|POST /chat-messages| DIFY
        DIFY -->|LLM gera resposta| LLM
        LLM -->|Resposta text| DIFY
        DIFY -->|JSON answer| BOT
        BOT -->|sendText| EV
        EV -->|Entrega| WA
        
        DIFY -.->|Opcional: RAG search| RAG
        DIFY -.->|Opcional: dados cliente| PGSQL
        
        style WA fill:#25D366,color:#fff
        style EV fill:#4CAF50,color:#fff
        style BOT fill:#2196F3,color:#fff
        style DIFY fill:#FF9800,color:#fff
        style LLM fill:#9C27B0,color:#fff
    end
```

## 📋 Fluxo de Dados

```
1. Cliente envia mensagem no WhatsApp (+55 34 99727-9291)
2. Evolution API recebe via Baileys (container plenus-evolution)
3. Evolution dispara webhook POST → FastAPI Bot (:5000/webhook)
4. Bot extrai texto, pushName, remoteJid
5. Bot chama Dify API (POST /v1/chat-messages, mode=blocking)
6. Dify processa com LLM (groq/llama-3.1-8b-instant)
7. Dify retorna JSON com answer, conversation_id, usage
8. Bot envia resposta de volta via Evolution (POST /message/sendText)
9. Evolution entrega no WhatsApp do cliente
```

## 🔧 Componentes

### Evolution API
| Propriedade | Valor |
|---|---|
| **Imagem** | `atendai/evolution-api:v1.8.7` |
| **Container** | `plenus-evolution` |
| **Porta** | `8081 → 8080` |
| **Auth** | `apikey: plenus-evolution-key-2026` |
| **Instância** | `plenus-main` |
| **Status** | ✅ `open` (conectado) |
| **Owner** | `553497279291@s.whatsapp.net` |
| **Perfil** | "Andamento Processual Plenus" |
| **Webhook** | `http://localhost:5000/webhook` |
| **Eventos** | `MESSAGES_UPSERT`, `CONNECTION_UPDATE`, `SEND_MESSAGE` |

### FastAPI Bot
| Propriedade | Valor |
|---|---|
| **Path** | `/opt/plenus-chatbot/` |
| **Entrypoint** | `app.main:app` (uvicorn) |
| **Porta** | `5000` |
| **Service** | `plenus-chatbot.service` (systemd, enabled) |
| **RAM** | ~35MB |
| **Python** | 3.12 (venv: `/opt/plenus-chatbot/venv`) |
| **Dependências** | `fastapi`, `uvicorn`, `httpx`, `python-dotenv` |
| **Logs** | `/opt/plenus-chatbot/logs/bot.log` |

### Dify
| Propriedade | Valor |
|---|---|
| **Versão** | `1.13.3` (self-hosted) |
| **URL** | `https://chatbot.advocaciaplenus.com` |
| **API** | `https://chatbot.advocaciaplenus.com/v1` |
| **API Key** | `app-6cOBe45Sf15sgAj5mKqvuXpV` |
| **Containers** | 12 (api, web, worker, worker_beat, nginx, redis, db_postgres, weaviate, sandbox, plugin_daemon, ssrf_proxy) |
| **SSL** | Let's Encrypt (porta 3443) |
| **Response Mode** | `blocking` |
| **Máx Retries** | 2 |
| **Timeout** | 60s |

## 📊 Métricas Validadas (Teste 05/04/2026)

| Métrica | Valor |
|---|---|
| **Latência** | 1.88s |
| **Tokens** | 527 |
| **Custo** | $0.000050 |
| **Success Rate** | 100% |
| **Sessões Ativas** | 1 |

## ✅ O Que Funciona

| Componente | Status |
|---|---|
| Evolution API | ✅ Online, instância conectada |
| WhatsApp | ✅ Conectado, perfil ativo |
| FastAPI Bot | ✅ systemd active, porta 5000 respondendo |
| Dify | ✅ 12 containers healthy |
| LLM (Groq) | ✅ Respondendo via Dify |
| Webhook | ✅ 3 eventos configurados |
| Métricas | ✅ `/metrics` operacional |
| Teste end-to-end | ✅ 100% funcional |

## ⚠️ O Que Não Funciona (Ainda)

| Problema | Impacto | Solução Planejada |
|---|---|---|
| Resposta vai direto pro cliente | Sem revisão humana | Implementar modo review (forward_to_lawyer) |
| RAG API não integrada ao Dify | Bot sem contexto jurídico Plenus | Conectar como tool no workflow |
| PostgreSQL não usado | CRM e Prazos inativos | Implementar tools de CRM |
| Número de teste não existe | Envio falha com 400 | Normal — validar com número real |

## 🗂️ Estrutura do Projeto

```
/opt/plenus-chatbot/
├── .env                          # Variáveis de ambiente
├── requirements.txt              # Dependências Python
├── docker-compose.yml            # Evolution API + Redis
├── setup.sh                      # Script de setup
├── qrcode.png                    # QR Code WhatsApp (se existir)
├── app/
│   ├── __init__.py
│   ├── main.py                   # FastAPI + webhook handler
│   ├── agent.py                  # DifyAgent + métricas
│   └── tools/                    # Tools (se houver)
├── config/
│   └── settings.py               # Configurações (se houver)
├── logs/
│   ├── bot.log                   # Log principal
│   └── bot-error.log             # Log de erros
├── venv/                         # Ambiente virtual Python
└── docs/
    └── FLUXOGRAMA-STACK1.md      # Este arquivo
```

## 🔌 Endpoints

| Endpoint | Método | Descrição |
|---|---|---|
| `http://localhost:5000/` | GET | Status do bot |
| `http://localhost:5000/health` | GET | Health check |
| `http://localhost:5000/metrics` | GET | Métricas do agente |
| `http://localhost:5000/webhook` | POST | Recebe webhooks da Evolution |
| `http://localhost:8081/` | GET | Evolution API status |
| `http://localhost:8081/docs` | GET | Swagger Evolution API |

## 📝 Histórico de Mudanças

| Data | Mudança |
|---|---|
| 05/04/2026 | Stack estabilizada: Evolution + Bot + Dify end-to-end |
| 05/04/2026 | Removido conflito de porta 5000 (plenus-bot antigo desabilitado) |
| 05/04/2026 | Webhook configurado na Evolution API |
| 05/04/2026 | Teste end-to-end validado (1.88s, 527 tokens) |
| 05/04/2026 | Documentação criada neste repositório |

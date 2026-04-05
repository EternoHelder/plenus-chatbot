# Plenus Chatbot — Stack WhatsApp

Stack de atendimento automatizado via WhatsApp usando **Evolution API + FastAPI Bot + Dify**.

## 🏗️ Arquitetura

```
WhatsApp → Evolution API (:8081) → FastAPI Bot (:5000) → Dify API → LLM Resposta
```

## 📊 Status Atual

| Componente | Status |
|---|---|
| Evolution API v1.8.6 | ✅ Online |
| WhatsApp (+55 34 99727-9291) | ✅ Conectado |
| FastAPI Bot | ✅ Running (systemd) |
| Dify 1.13.3 | ✅ 12 containers |
| LLM (groq/llama-3.1-8b) | ✅ Respondendo |

## 📋 Métricas

- Latência média: **1.88s**
- Tokens: **527**
- Custo: **$0.000050/msg**
- Success rate: **100%**

## 📄 Documentação

- [Fluxograma Stack 1](docs/FLUXOGRAMA-STACK1.md) — Arquitetura completa, componentes, endpoints, histórico

## 🚀 Quick Start

```bash
# Subir Evolution API
cd /opt/plenus-chatbot && docker compose up -d

# Iniciar FastAPI Bot
systemctl enable --now plenus-chatbot

# Verificar status
curl http://localhost:5000/health
curl http://localhost:8081/instance/connectionState/plenus-main -H "apikey: plenus-evolution-key-2026"
```

---

*Plenus Advocacia — 05/04/2026*

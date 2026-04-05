# plenus-chatbot

> Assistente jurídico inteligente via WhatsApp — IA especializada em Direito.

---

## 📋 Sobre o Projeto

O **plenus-chatbot** é um chatbot para WhatsApp que utiliza Inteligência Artificial especializada na área jurídica. Desenvolvido para oferecer orientações legais rápidas, claras e acessíveis diretamente pelo WhatsApp, sem necessidade de instalação de aplicativos adicionais.

Com o plenus-chatbot, usuários podem tirar dúvidas sobre:

- Direito Civil (contratos, família, herança)
- Direito do Trabalho (CLT, rescisões, direitos trabalhistas)
- Direito do Consumidor (reclamações, reembolsos, garantias)
- Direito Penal (orientações gerais)
- Direito Tributário (impostos, declarações)
- E muito mais...

> ⚠️ **Aviso:** As respostas fornecidas pelo chatbot são de caráter informativo e **não substituem** a consulta com um advogado habilitado.

---

## 🚀 Funcionalidades

- 💬 Atendimento 24/7 via WhatsApp
- 🤖 Respostas geradas por IA com contexto jurídico
- 📚 Base de conhecimento atualizada com legislação brasileira
- 🔒 Conversas privadas e seguras
- 📊 Histórico de interações por usuário
- 🔄 Encaminhamento para advogados parceiros quando necessário

---

## 🛠️ Tecnologias

- **Node.js** — Runtime principal
- **WhatsApp Web API / Baileys** — Integração com WhatsApp
- **OpenAI / LLM** — Motor de Inteligência Artificial
- **MongoDB** — Armazenamento de dados e histórico
- **Docker** — Containerização da aplicação

---

## ⚙️ Instalação

### Pré-requisitos

- Node.js >= 18
- Docker & Docker Compose (opcional)
- Conta e chave de API do provedor de IA

### Configuração

1. Clone o repositório:

```bash
git clone https://github.com/EternoHelder/plenus-chatbot.git
cd plenus-chatbot
```

2. Instale as dependências:

```bash
npm install
```

3. Copie o arquivo de variáveis de ambiente e preencha as informações necessárias:

```bash
cp .env.example .env
```

4. Inicie a aplicação:

```bash
npm start
```

5. Escaneie o QR Code exibido no terminal com o WhatsApp para autenticar a sessão.

### Com Docker

```bash
docker-compose up -d
```

---

## 🔧 Variáveis de Ambiente

| Variável          | Descrição                                      | Obrigatório |
|-------------------|------------------------------------------------|-------------|
| `AI_API_KEY`      | Chave de API do provedor de IA                 | ✅           |
| `AI_MODEL`        | Modelo de linguagem a utilizar                 | ✅           |
| `MONGODB_URI`     | URI de conexão com o MongoDB                   | ✅           |
| `SESSION_SECRET`  | Segredo para criptografia de sessão            | ✅           |
| `LOG_LEVEL`       | Nível de log (`info`, `debug`, `error`)        | ❌           |

---

## 📁 Estrutura do Projeto

```
plenus-chatbot/
├── src/
│   ├── bot/          # Lógica principal do chatbot
│   ├── ai/           # Integração com IA e prompts jurídicos
│   ├── database/     # Modelos e conexão com banco de dados
│   ├── handlers/     # Manipuladores de mensagens
│   └── utils/        # Funções utilitárias
├── .env.example
├── docker-compose.yml
├── Dockerfile
├── package.json
└── README.md
```

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Siga os passos abaixo:

1. Faça um fork do projeto
2. Crie uma branch para a sua feature: `git checkout -b feature/minha-feature`
3. Commit suas alterações: `git commit -m 'feat: adiciona minha feature'`
4. Push para a branch: `git push origin feature/minha-feature`
5. Abra um Pull Request

---

## 📄 Licença

Este projeto é privado e de uso exclusivo da **Plenus**. Todos os direitos reservados.

---

## 📞 Contato

Para dúvidas ou sugestões, entre em contato com a equipe Plenus.

---

*Desenvolvido com ❤️ pela equipe Plenus*

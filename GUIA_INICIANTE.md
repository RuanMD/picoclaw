# Guia do Iniciante: PicoClaw 🦞

Bem-vindo ao **PicoClaw**! Este guia explica a estrutura de arquivos e como você pode configurar e usar seu assistente pessoal de IA.

## 📁 Estrutura de Arquivos Principal

No seu MacBook, os arquivos importantes estão em `docker/data/`.

### 1. Configuração do Sistema (`docker/data/config.json`)
Este é o "cérebro" técnico do PicoClaw.
- **`model_list`**: Lista de todos os modelos de IA disponíveis (OpenAI, Groq, Anthropic, etc).
- **`agents -> defaults`**: Define qual modelo será usado por padrão e configurações como "Max Tokens" (o tamanho da resposta).
- **`channels`**: Onde você configura integrações com Telegram, WhatsApp, Discord, etc.

### 2. O Workspace do Agente (`docker/data/workspace/`)
Aqui é onde você define a **personalidade** e o **conhecimento** do seu robô.

- **`IDENTITY.md`**: Define **quem** é o robô. Nome, propósito e história.
- **`AGENT.md`**: Define **regras de comportamento**. Ex: "Sempre responda em Português", "Seja curto e grosso".
- **`USER.md`**: Informações sobre **você**. Seu nome, fuso horário, gostos e o que você espera da IA.
- **`SOUL.md`**: Características mais profundas da "alma" da IA (valores e tom de voz).
- **`HEARTBEAT.md`**: Descreve tarefas que o robô deve fazer sozinho a cada 30 minutos (Ex: "Verifique o tempo e me avise se vai chover").
- **`memory/`**: Onde o robô guarda o que aprendeu sobre você para não esquecer em conversas futuras.
- **`sessions/`**: Histórico das suas conversas.

---

## 🚀 Como Usar via Terminal

Como você está usando Docker, aqui estão os comandos principais:

### 1. Perguntar algo rápido (Agente)
Use este comando para uma pergunta única. O comando apaga o contêiner logo após responder.
```bash
docker compose -f docker/docker-compose.yml --profile agent run --rm picoclaw-agent -m "Sua pergunta aqui"
```

### 2. Deixar o robô rodando (Gateway)
Use este comando se você configurou um canal (como Telegram). Ele fica rodando no fundo.
```bash
# Iniciar
docker compose -f docker/docker-compose.yml --profile gateway up -d

# Ver o que ele está fazendo (logs)
docker compose -f docker/docker-compose.yml logs -f picoclaw-gateway

# Parar o robô
docker compose -f docker/docker-compose.yml --profile gateway down
```

---

## 🛠️ Dicas para Personalizar

1. **Mudar o Idioma**: No arquivo `AGENT.md`, escreva "Responda sempre em Português do Brasil".
2. **Trocar de IA**: No `config.json`, mude o campo `"model_name"` dentro de `"defaults"` para um dos nomes que estão na lista `"model_list"` (ex: `gpt-4o` ou `llama-3.3-70b`).
3. **Novas Habilidades**: A pasta `workspace/skills/` permite adicionar roteiros (`SKILL.md`) que ensinam o robô a fazer coisas novas ou acessar ferramentas específicas.

---

## 🖥️ Existe uma tela ou painel?

O PicoClaw é um assistente **"headless"** (sem interface gráfica própria). Ele não tem uma página web ou aplicativo com botões. Sua "tela" é o lugar onde você conversa com ele:

1.  **O Terminal**: Onde você vê as respostas rápidas e os logs de funcionamento.
2.  **Aplicativos de Chat**: Se você configurar o Telegram (recomendado), a "tela" do seu PicoClaw será o próprio Telegram no seu celular ou computador.

### Como ver o que ele está pensando (Logs)
Para acompanhar as ações do robô em tempo real, use os logs do Docker:
```bash
docker compose -f docker/docker-compose.yml logs -f picoclaw-gateway
```

### Endpoints de Saúde (Apenas Técnico)
Se você for desenvolvedor, pode acessar os status técnicos via navegador:
- [http://localhost:18790/health](http://localhost:18790/health) (Verifica se está vivo)
- [http://localhost:18790/ready](http://localhost:18790/ready) (Verifica se está pronto para aceitar mensagens)

---

## 🧩 Como Instalar Novas Skills (VPS/Portainer)

Você pode adicionar novas habilidades ao seu PicoClaw de duas formas principais quando ele estiver rodando na sua VPS:

### 1. Pelo Canal de Chat (Telegram) 🪄
O PicoClaw possui ferramentas nativas para autogestão. No chat do Telegram, você pode pedir:
- `"Pesquise no ClawHub pela skill [Nome] e instale para mim."`
- `"Instale a skill desse repositório: [Link do GitHub]"`

O robô usará o acesso à internet (Tavily) para baixar e configurar o arquivo `SKILL.md` automaticamente na pasta correta.

### 2. Pelo Painel do Portainer (Manual) 🛠️
Se precisar instalar manualmente via navegador:
1. No Portainer, acesse os detalhes do contêiner `picoclaw-gateway`.
2. Clique no botão **Console** (`>_ Console`) e conecte-se.
3. Use o comando `wget` para baixar o arquivo diretamente na pasta de skills:
   ```bash
   # Crie a pasta da skill
   mkdir -p ~/.picoclaw/workspace/skills/minha-skill
   # Baixe o arquivo (exemplo vindo do GitHub)
   wget -O ~/.picoclaw/workspace/skills/minha-skill/SKILL.md https://raw.githubusercontent.com/usuario/repo/main/SKILL.md
   ```
4. O robô lerá a nova skill automaticamente ou após um reinício da stack.

---

皮皮虾， we走！ (PicoClaw, Partiu! 🦞)

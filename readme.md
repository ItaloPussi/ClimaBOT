# Chatbot de Previsão do Tempo (n8n + Telegram + OpenWeather)

Este projeto consiste em um workflow automatizado desenvolvido no **n8n** que integra o **Telegram** à API da **OpenWeatherMap**. O bot recebe o nome de uma cidade (ou Cidade, UF), processa os dados seguindo normas de padronização de strings e retorna a temperatura atual em tempo real.

## 🚀 Funcionalidades
* **Interface via Telegram**: Interação direta por mensagens de texto.
* **Tratamento de Dados**: Normalização automática de strings (remoção de acentos via `normalize("NFD")`, remoção de espaços extras e conversão para minúsculas) para garantir compatibilidade total com a API.
* **Geolocalização**: Suporte para o formato `Cidade, UF` (Ex: São Paulo, SP).
* **Validação de Resposta**: Uso de nó `IF` para verificar o sucesso da requisição e tratar erros de cidades não encontradas.

---

## 🛠️ Requisitos de Infraestrutura
Para o funcionamento correto deste workflow, é obrigatório:
1.  **Instância n8n**: Uma instalação ativa (Cloud, Docker ou Local).
2.  **HTTPS Ativo**: O n8n **deve** estar rodando sob HTTPS (SSL) para que os Webhooks do Telegram funcionem corretamente (exigência técnica do Telegram).
3.  **Bot no Telegram**: Token obtido via [@BotFather](https://t.me/botfather).
4.  **Chave de API OpenWeather**: Obtida no painel da [OpenWeatherMap](https://openweathermap.org/api).

---

## 📦 Instruções de Importação e Configuração

### 1. Importação
1.  Faça o download do arquivo `workflow-telegram-chatbot.json` presente neste repositório.
2.  No seu n8n, clique em **Workflows** > **Import from File**.
3.  Selecione o arquivo baixado.

### 2. Configuração de Credenciais (Telegram)
O workflow utiliza o recurso nativo de credenciais do n8n. Você deve configurar seu token nos seguintes nós:
* `Mensagem Recebida | Telegram`
* `Mensagem de Sucesso | Telegram`
* `Mensagem de Erro | Telegram`
* `OpenWeatherMap`

**Como configurar:** Em qualquer um desses nós, selecione sua credencial existente ou crie uma nova inserindo o seu `TELEGRAM_API_KEY` ou 'OPENWEATHER_API_KEY'.
---

## 🧬 Estrutura Técnica do Workflow

* **Trigger**: Inicia o fluxo ao receber uma mensagem de texto.
* **Code Node (`queue`)**: Captura o texto e aplica a lógica JavaScript para remover acentos (.normalize("NFD")), remover espaços extras e converter para minúsculas.
* **HTTP Request**: Realiza a chamada `GET` para a API OpenWeather usando a variável `queue`.
* **IF Node**: Valida se a requisição foi bem sucedida
* **Output Sucesso**: Retorna: `🌤️ A temperatura em [Cidade] é de [X]°C.`
* **Output Erro**: Retorna: `❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).`

---

## 🔒 Segurança e Privacidade
O arquivo JSON exportado **não contém credenciais ou tokens embutidos**. Todas as chaves devem ser configuradas manualmente após a importação conforme as instruções acima.

---
**Desenvolvido como parte da disciplina de "Agentes e Automação" da pós-graduação em Inteligência Artificial da Faculdade de Tecnologia Rocketseat (FTR).**
# CNS Bot

Bot do Telegram desenvolvido em Python para facilitar consultas operacionais e gerar documentos e relatórios a partir de dados do **CNS / Vista**, utilizando consultas ao Microsoft SQL Server.

O projeto reúne consultas de tarefas, acompanhamento de visitas e geração de QR Codes para contratos e estruturas, entregando resultados diretamente na conversa do Telegram.

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Telegram%20Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Bot">
  <img src="https://img.shields.io/badge/Microsoft%20SQL%20Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white" alt="Microsoft SQL Server">
  <img src="https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white" alt="SQLAlchemy">
</p>

## 📌 Funcionalidades

- **Consultas de tarefas diárias:** totaliza tarefas concluídas por contrato (CR), com filtro opcional pelo nome da tarefa.
- **Consultas mensais de tarefas:** apresenta os totais do mês atual por contrato e, opcionalmente, por tarefa.
- **Acompanhamento de visitas:** consulta a quantidade de visitas registradas por gerente, com possibilidade de informar o mês.
- **Visão regional de visitas:** consulta visitas por gerente regional.
- **Geração de QR Codes:** cria arquivos para contratos e estruturas, com seleção de nível e identidade visual.
- **Relatórios em imagem:** transforma resultados SQL em tabelas PNG e inclui a identidade visual configurada.
- **Interface no Telegram:** recebe comandos e devolve consultas, imagens e documentos diretamente no chat.

## 🧭 Comandos disponíveis

Envie os comandos ao bot pelo Telegram. Substitua os parâmetros pelos valores desejados.

| Comando | Descrição | Exemplo |
|---|---|---|
| `/start` ou `/init` | Exibe a mensagem inicial e os comandos disponíveis. | `/start` |
| `/help` ou `/ajuda` | Mostra a ajuda do bot. | `/ajuda` |
| `/total CR` ou `/tt CR` | Consulta tarefas concluídas no dia atual para o contrato. | `/total 12345` |
| `/total CR:TAREFA` | Filtra a consulta diária por parte do nome da tarefa. | `/total 12345:Limpeza` |
| `/totalm CR` ou `/tm CR` | Consulta tarefas concluídas no mês atual. | `/totalm 12345` |
| `/totalm CR:TAREFA` | Filtra a consulta mensal por parte do nome da tarefa. | `/totalm 12345:Limpeza` |
| `/visita GERENTE` | Consulta visitas do gerente no mês atual. | `/visita MARIA SILVA` |
| `/visita GERENTE:MÊS` | Consulta visitas do gerente no mês informado. | `/visita MARIA SILVA:8` |
| `/visitaR REGIONAL` | Consulta visitas da regional no mês atual. | `/visitaR JOÃO SILVA` |
| `/visitaR REGIONAL:MÊS` | Consulta visitas da regional no mês informado. | `/visitaR JOÃO SILVA:8` |
| `/qr CR:NÍVEL:EMPRESA` | Gera QR Codes para o contrato, com nível e logo opcionais. | `/qr 12345:3:Grupo GPS` |

**Parâmetros de QR Code**

- **CR:** número do contrato, apenas dígitos.
- **Nível:** nível da estrutura a consultar; quando omitido, o código usa o nível 3.
- **Empresa:** identidade visual solicitada. O código usa `Grupo GPS` como padrão quando o parâmetro não é informado. As opções documentadas na ajuda incluem `Poliservice`, `Grupo GPS`, `In Haus` e `Topservice`.

Os comandos de visita usam o ano atual e aceitam o número do mês. Confira os resultados antes de utilizá-los em relatórios oficiais.

## 🛠️ Tecnologias

- **Python** — linguagem principal.
- **pyTelegramBotAPI (`telebot`)** — integração com o Telegram Bot API.
- **SQLAlchemy** — criação da conexão com o banco de dados.
- **Microsoft SQL Server** — origem dos dados operacionais.
- **pandas** — execução e organização dos resultados SQL.
- **dataframe_image** — exportação de tabelas para imagens.
- **Pillow (PIL)** — composição de imagens e logotipos.
- **Requests** — obtenção da configuração remota utilizada pelo projeto.
- **Selenium** — mecanismo de renderização utilizado na exportação de tabelas.

## 📂 Estrutura do projeto

```text
CNS_Bot/
├── server.py          # Inicialização do bot, comandos e consultas SQL
├── models/
│   └── msgP.py        # Mensagens de ajuda e boas-vindas
├── utils/
│   ├── get_fb.py      # Leitura da configuração remota
│   └── qrcode.py      # Geração dos QR Codes
├── src/
│   ├── cores/         # Recursos visuais
│   ├── fonts/         # Fontes
│   └── logos/         # Logotipos
├── QRCodes/           # Diretório de saída dos arquivos gerados
├── GPS.png            # Logotipo usado nos relatórios
└── README.md
```

## ⚙️ Requisitos

- Python compatível com as bibliotecas utilizadas pelo projeto.
- Um bot criado pelo [BotFather](https://t.me/BotFather) no Telegram.
- Acesso autorizado ao Microsoft SQL Server e ao banco de dados Vista.
- Driver ODBC da Microsoft compatível com a versão do SQL Server.
- Dependências Python necessárias para consultas, exportação de imagens e geração de QR Codes.
- Configuração remota válida para os parâmetros de conexão e o token do bot.
- Recursos locais exigidos pela geração de relatórios, como o arquivo de logotipo.

## 🚀 Instalação e execução

### 1. Clone o repositório

```bash
git clone https://github.com/foxtec198/CNS_Bot.git
cd CNS_Bot
```

### 2. Crie um ambiente virtual

**Windows (PowerShell):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Instale as dependências

O repositório não possui um arquivo `requirements.txt` na raiz. As bibliotecas abaixo correspondem aos imports identificados no código; a versão exata deve ser validada no ambiente de execução.

```bash
python -m pip install --upgrade pip
python -m pip install pyTelegramBotAPI pandas dataframe-image SQLAlchemy Pillow requests pyodbc qrcode
```

A exportação de tabelas com Selenium pode exigir um navegador compatível e seu driver, conforme a versão instalada de `dataframe_image`. Instale também o Microsoft ODBC Driver for SQL Server apropriado ao sistema operacional.

### 4. Configure e execute

Antes de iniciar, valide o acesso à configuração remota, as credenciais do banco e o token do bot. Com os requisitos atendidos, execute:

```bash
python server.py
```

O processo inicia o polling do Telegram e aguarda comandos. Para execução contínua, utilize um serviço gerenciado e configure logs e reinicialização automática.

## 🔐 Segurança e uso responsável

- **Não publique tokens do Telegram, senhas ou credenciais de banco de dados.** Utilize variáveis de ambiente ou um gerenciador de segredos para novas implantações.
- Revise a configuração remota e suas regras de acesso; não presuma que um endpoint público esteja protegido.
- Restrinja o acesso ao bot e ao banco de dados a usuários autorizados.
- Valide os parâmetros recebidos nos comandos e utilize consultas parametrizadas para reduzir o risco de SQL injection.
- Não inclua dados pessoais, informações operacionais confidenciais ou documentos reais em commits públicos.
- Avalie os arquivos já versionados no diretório `QRCodes/` antes de manter o repositório público.
- Faça testes em ambiente autorizado antes de executar consultas em produção.

## 🧩 Melhorias recomendadas

- Adicionar um `requirements.txt` com versões fixadas e testadas.
- Mover credenciais e configurações para variáveis de ambiente.
- Parametrizar as consultas SQL.
- Separar a lógica de comandos, consultas e geração de relatórios em módulos.
- Adicionar validação dos parâmetros e mensagens de erro consistentes.
- Criar testes automatizados para os comandos e para a geração de QR Codes.
- Documentar a implantação e o procedimento de operação do bot.

## 👨‍💻 Sobre

Projeto desenvolvido por **Guilherme Barbosa André Breve**, durante sua atuação no Grupo GPS, com foco em automação de consultas operacionais e disponibilização de informações pelo Telegram.

---

<p align="center">
  <sub>Automação operacional, consultas e relatórios diretamente no Telegram.</sub>
</p>

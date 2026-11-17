# Testes de Performance - Banco API com K6

## Introdução

Este repositório contém a suíte automatizada de testes de performance desenvolvida para validar a estabilidade, tempo de resposta, vazão (throughput) e resiliência dos endpoints da aplicação **Banco API**. Os scripts simulam diferentes cenários de carga e estresse utilizando JavaScript e a ferramenta K6.

## Tecnologias Utilizadas

* **K6 (Grafana Labs):** Ferramenta open-source para execução de testes de carga e performance.
* **JavaScript (ES6+):** Linguagem utilizada para a escrita dos scripts de teste, validações (checks) e métricas customizadas.
* **Node.js / npm:** Gerenciamento de dependências auxiliares e scripts de automação (opcional, caso utilize pacotes externos ou linters).

## Estrutura do Repositório

A organização dos diretórios e arquivos do projeto segue a estrutura abaixo:

```text
banco-api-performace/
├── config/             # Configurações globais, variáveis de ambiente e parâmetros de carga
├── fixture/            # Massas de dados de entrada para os testes (JSON/CSV)
├── helpers/            # Funções auxiliares específicas de regras de negócio e requisições
├── testes/             # Scripts principais de execução dos testes de performance
├── utils/              # Funções utilitárias genéricas e reutilizáveis
├── .gitignore          # Arquivos e diretórios ignorados pelo controle de versão
└── README.md           # Documentação técnica do projeto
```

## Objetivo de Cada Grupo de Arquivo

* **`config/`**: Centraliza os arquivos de configuração do projeto, gerenciamento de variáveis de ambiente (como a `BASE_URL`), limites aceitáveis de latência e taxa de erro (`thresholds`), além da definição de estágios de carga (`stages`).
* **`fixture/`**: Armazena os arquivos de dados de entrada (como credenciais de usuários, payloads e parâmetros estáticos) utilizados para alimentar as requisições durante a execução dos testes.
* **`helpers/`**: Contém funções de apoio voltadas ao domínio da aplicação, como fluxos de autenticação (obtenção de tokens), montagem de requisições específicas e validações de negócio.
* **`testes/`**: Agrupa os scripts principais que executam os cenários de teste de performance (ex.: login, transferências, consultas) sob diferentes perfis de carga e estresse.
* **`utils/`**: Reúne funções utilitárias genéricas e independentes de regra de negócio, como geradores de dados aleatórios, formatadores e manipuladores de strings ou datas.

## Modo de Instalação e Execução do Projeto

### Pré-requisitos

* **K6** instalado na máquina local. Consulte a documentação oficial para instalação via gerenciador de pacotes (`winget`, `choco`, `brew`, `apt`) ou binário.
* **Git** para clonagem do repositório.
* Instância da **Banco API** em execução e acessível via rede.

### Instalação

1. Clone o repositório para o ambiente local:

```bash
git clone https://github.com/Renanrrs16/banco-api-performace.git
```

2. Acesse o diretório do projeto:

```bash
cd banco-api-performace
```

*(Nota: Caso o projeto utilize `package.json` para dependências auxiliares, execute `npm install` nesta etapa).*

### Execução dos Testes

A execução dos scripts exige a definição da variável de ambiente `BASE_URL`, que indica o endereço base da API alvo.

#### Execução Padrão (Terminal)

Para executar um script de teste enviando a saída apenas para o console:

**Linux / macOS:**

```bash
k6 run -e BASE_URL=http://localhost:3000 testes/nome-do-teste.js
```

**Windows (PowerShell):**

```powershell
k6 run -e BASE_URL=http://localhost:3000 testes/nome-do-teste.js
```

#### Execução com Relatório em Tempo Real e Exportação HTML

O K6 possui um dashboard web nativo que permite acompanhar métricas em tempo real durante a execução e exportar o resultado consolidado ao término do teste. Utilize as variáveis de ambiente `K6_WEB_DASHBOARD` e `K6_WEB_DASHBOARD_EXPORT` em conjunto com a `BASE_URL`:

**Linux / macOS:**

```bash
K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=html-report.html k6 run -e BASE_URL=http://localhost:3000 testes/nome-do-teste.js
```

**Windows (PowerShell):**

```powershell
$env:K6_WEB_DASHBOARD="true"; $env:K6_WEB_DASHBOARD_EXPORT="html-report.html"; k6 run -e BASE_URL=http://localhost:3000 testes/nome-do-teste.js
```

**Windows (CMD):**

```cmd
set K6_WEB_DASHBOARD=true && set K6_WEB_DASHBOARD_EXPORT=html-report.html && k6 run -e BASE_URL=http://localhost:3000 testes/nome-do-teste.js
```

* Durante a execução, o painel em tempo real ficará acessível no navegador através do endereço padrão `http://127.0.0.1:5665`.
* Ao finalizar o teste, o arquivo `html-report.html` será gerado na raiz do diretório de execução.

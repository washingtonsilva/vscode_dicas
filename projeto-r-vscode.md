# Do RStudio para VS Code: Criando Projetos R

Este guia explica como replicar a experiência de **Projetos RStudio** no **VS Code**, mantendo a organização e funcionalidades que você já conhece.

## Como usar este guia

> **Para iniciantes no VS Code:**
> 
> 1. **Procure as caixas azuis** com "O que é...?" - elas explicam conceitos técnicos
> 2. **Fique atento aos avisos** "Para Iniciantes" - são dicas importantes
> 3. **Leia as dicas** em caixas amarelas - são truques úteis
> 4. **Consulte o Glossário** no final se algum termo não fizer sentido
> 5. **Comece simples** - não tente usar tudo de uma vez!

> **Ordem recomendada de leitura:**
> 1. Leia as seções conceituais (O que é...)
> 2. Siga o setup passo-a-passo
> 3. Crie seu primeiro projeto
> 4. Explore funcionalidades avançadas depois

## O que é um Projeto RStudio?

No RStudio, um projeto é:
- Uma pasta com arquivo `.Rproj`
- Working directory automático
- Histórico de comandos isolado
- Environment variables específicas
- Git integration
- Package management isolado (com `renv`)

## Equivalente no VS Code: Workspaces

No VS Code, o equivalente mais próximo são os **Workspaces**, que oferecem:

> **O que é um Workspace?**
> 
> Um **Workspace** no VS Code é como uma "área de trabalho personalizada" para seu projeto. É uma forma de agrupar arquivos, configurações e ferramentas específicas para um projeto. Existem dois tipos principais:

### Opção 1: **Folder Workspace** (Recomendado para R)
> **Explicação Simples**: É quando você abre uma pasta no VS Code e ela se torna seu "projeto". Todas as configurações ficam numa pasta `.vscode/` dentro do seu projeto.

```
meu_projeto_r/
├── .vscode/                 # Configurações do workspace
│   ├── settings.json        # Configurações específicas do projeto
│   ├── launch.json          # Configurações de debug
│   └── tasks.json           # Tasks automatizadas
├── data/                    # Dados do projeto
├── scripts/                 # Scripts R
├── output/                  # Resultados
├── renv/                    # Ambiente R isolado (opcional)
├── .Rprofile               # Configurações R do projeto
├── .gitignore              # Arquivos ignorados pelo Git
└── README.md               # Documentação do projeto
```

### Opção 2: **Multi-root Workspace** (Para projetos complexos)
> **Explicação Simples**: É quando você precisa trabalhar com várias pastas ao mesmo tempo, como se fossem um projeto único. Por exemplo, dados em uma pasta, código em outra, e documentação em uma terceira.

```json
// arquivo .code-workspace
{
    "folders": [
        { "path": "./projeto-principal" },
        { "path": "./dados-externos" },
        { "path": "./documentacao" }
    ],
    "settings": {
        "r.rterm.windows": "C:/Program Files/R/R-4.3.0/bin/x64/Rgui.exe"
    }
}
```

> **Para Iniciantes**: Comece sempre com **Folder Workspace** (Opção 1). É mais simples e cobre 95% dos casos!

## Configuração Passo a Passo

### 1. **Criar o Projeto**

```bash
# Criar pasta do projeto
mkdir meu_projeto_r
cd meu_projeto_r

# Inicializar Git (opcional mas recomendado)
git init

# Abrir no VS Code
code .
```

### 2. **Configurar Workspace Settings**

> **O que são Workspace Settings?**
> 
> São configurações específicas do seu projeto que ficam salvas na pasta `.vscode/settings.json`. É como ter "preferências personalizadas" que só valem para aquele projeto específico. Por exemplo: você pode querer usar fonte maior em um projeto e menor em outro.

Crie `.vscode/settings.json` com configurações específicas do projeto:

```json
{
    "r.rterm.option": [],              // Opções extras para o R (deixe vazio por enquanto)
    "r.rpath.mac": "/usr/local/bin/R", // Onde o VS Code encontra o R no seu Mac
    "r.alwaysUseActiveTerminal": true, // Sempre usar o mesmo terminal para R
    "r.bracketedPaste": true,          // Colar código de forma segura
    "r.sessionWatcher": true,          // VS Code monitora sua sessão R
    "r.rtermSendDelay": 0,            // Sem delay ao enviar código (mais rápido)
    "files.associations": {            // Que programa abre cada tipo de arquivo
        "*.R": "r",                    //   Arquivos .R abrem como R
        "*.qmd": "quarto"              //   Arquivos .qmd abrem como Quarto
    },
    "workbench.startupEditor": "readme",     // Abre README ao iniciar projeto
    "explorer.confirmDelete": true,          // Confirmar antes de deletar arquivos
    "git.openRepositoryInParentFolders": "never" // Não procurar Git em pastas superiores
}
```

> **Dica para Iniciantes**: Não se preocupe em entender cada configuração agora. Copie e cole este arquivo - ele já está otimizado para trabalhar com R!

### 3. **Configurar R Environment (.Rprofile)**

> **O que é .Rprofile?**
> 
> É um arquivo que o R lê automaticamente toda vez que você inicia uma sessão R naquela pasta. É como ter um "script de inicialização" que configura seu ambiente R sempre da mesma forma. No RStudio, isso acontece automaticamente quando você abre um projeto.

Crie `.Rprofile` na raiz do projeto:

```r
# .Rprofile - Configurações específicas do projeto
cat("Projeto R carregado no VS Code!\n")  # Mensagem de boas-vindas

# Definir working directory
if (interactive()) {  # Só executa se estivermos em sessão interativa
  # Garantir que estamos no diretório do projeto
  if (basename(getwd()) != "meu_projeto_r") {
    setwd(here::here())  # here::here() sempre aponta para a raiz do projeto
  }
  
  # Carregar pacotes comuns (opcional)
  suppressMessages({
    library(here)      # Gerenciamento de paths (caminhos) relativos
    library(renv)      # Gerenciamento de pacotes (opcional)
  })
  
  cat("Working directory:", getwd(), "\n")  # Mostra onde estamos
}

# Opções R específicas do projeto
options(
  scipen = 999,                    # Evitar notação científica (1e+05 vira 100000)
  digits = 4,                      # Mostrar 4 casas decimais por padrão
  stringsAsFactors = FALSE,        # Texto vira texto, não fator
  repos = c(CRAN = "https://cran.rstudio.com/")  # De onde baixar pacotes
)
```

> **Para Iniciantes**: Este arquivo é opcional, mas muito útil! Ele garante que seu ambiente R sempre inicie da mesma forma, com as mesmas configurações.

### 4. **Estrutura de Pastas Recomendada**

```
meu_projeto_r/
├── .vscode/
│   └── settings.json
├── data/
│   ├── raw/                     # Dados brutos (não editar)
│   ├── processed/               # Dados processados
│   └── external/                # Dados externos
├── scripts/
│   ├── 01_import.R             # Importação de dados
│   ├── 02_clean.R              # Limpeza de dados
│   ├── 03_analysis.R           # Análises
│   └── 04_visualizations.R     # Visualizações
├── notebooks/
│   ├── exploratory.qmd         # Análise exploratória
│   ├── report.qmd              # Relatório principal
│   └── presentation.qmd        # Apresentação
├── functions/
│   └── utils.R                 # Funções customizadas
├── output/
│   ├── figures/                # Gráficos
│   ├── tables/                 # Tabelas
│   ├── reports/                # Relatórios renderizados
│   └── presentations/          # Apresentações renderizadas
├── renv/                       # Ambiente isolado (opcional)
├── .Rprofile                   # Configurações R
├── .gitignore                  # Git ignore
├── .here                       # Marcador do projeto (here package)
├── renv.lock                   # Lock file dos pacotes
└── README.md                   # Documentação
```

## Extensões Essenciais

> **O que são Extensões?**
> 
> Extensões são como "aplicativos" que você instala no VS Code para adicionar funcionalidades. O VS Code sozinho é apenas um editor de texto. As extensões é que transformam ele numa ferramenta poderosa para R, Python, etc.
> 
> **Como instalar**: Digite `⌘+⇧+P`, procure por "Extensions: Install Extensions" e digite o nome da extensão.

Instale estas extensões para replicar funcionalidades do RStudio:

### **Essenciais para R e Quarto:**
```bash
# Via Command Palette (⌘+⇧+P) → "Extensions: Install Extensions"

REditorSupport.r           # Suporte principal para R (OBRIGATÓRIA)
RDebugger.r-debugger       # Para encontrar erros no código R  
quarto.quarto              # Para trabalhar com arquivos .qmd
REditorSupport.r-lsp       # Inteligência artificial para R (autocomplete)
ms-toolsai.jupyter         # Para notebooks (se usar)
```

> **O que cada uma faz:**
> - **R Extension**: Transforma VS Code numa IDE para R (como RStudio)
> - **R Debugger**: Ajuda a encontrar erros no seu código
> - **Quarto**: Para escrever relatórios e apresentações
> - **R LSP**: Autocomplete inteligente (sugere funções enquanto você digita)
> - **Jupyter**: Para trabalhar com notebooks (opcional)

### **Produtividade Geral:**
```bash
ms-vscode.vscode-json      # Para trabalhar com arquivos de configuração
ms-python.python          # Se também usar Python
GitLens.gitlens            # Melhor interface para Git
ms-vscode.hexeditor        # Para examinar arquivos binários (avançado)
```

> **Para Iniciantes**: Instale apenas as extensões de R por enquanto. Adicione outras conforme a necessidade!

## Workflow Comparativo

| **RStudio** | **VS Code** | **Como Fazer** |
|-------------|-------------|----------------|
| Novo Projeto | Novo Workspace | `File > Open Folder` |
| Console R | Terminal R | `⌃ + `` → `R` |
| Environment | R: Show Environment | `⌘+⇧+P` → "R: Show Environment" |
| Help | R: Show Help | `⌘+⇧+P` → "R: Show Help" |
| Plots | R: Show Plot | `⌘+⇧+P` → "R: Show Plot" |
| Run Script | Send to Terminal | `⌘+⏎` (linha) ou `⌘+⇧+S` (arquivo) |
| Knit Document | Quarto Render | `⌘+⇧+K` ou `⌘+⇧+P` → "Quarto: Render" |
| R Markdown | Quarto Document | Arquivo `.qmd` com preview integrado |

## Trabalhando com Quarto (.qmd)

### **Configuração Quarto no Workspace**
Adicione ao `.vscode/settings.json`:
```json
{
    "quarto.render.renderOnSave": false,
    "quarto.render.previewType": "external",
    "files.associations": {
        "*.qmd": "quarto"
    },
    "[quarto]": {
        "editor.wordWrap": "on",
        "editor.quickSuggestions": {
            "comments": false,
            "strings": false,
            "other": true
        }
    }
}
```

### **Estrutura de Documento Quarto Padrão**
```yaml
---
title: "Análise de Dados"
author: "Seu Nome"
date: "`r Sys.Date()`"
format: 
  html:
    code-fold: true
    toc: true
    theme: cosmo
  pdf:
    documentclass: article
execute:
  warning: false
  message: false
---
```

### **Atalhos Quarto Essenciais**
| Ação | Atalho | Descrição |
|------|--------|-----------|
| **Render Document** | `⌘+⇧+K` | Renderizar documento atual |
| **Preview** | `⌘+⇧+P` → "Quarto: Preview" | Visualizar preview |
| **Run Cell** | `⌘+⇧+⏎` | Executar célula de código |
| **Run All Cells** | `⌘+⌥+R` | Executar todas as células |
| **Insert Code Cell** | `⌃+⇧+I` | Inserir nova célula de código |

### **Template de Projeto Quarto**

> **O que é _quarto.yml?**
> 
> É o arquivo de configuração principal do Quarto para seu projeto. É como um "manual de instruções" que diz ao Quarto como renderizar seus documentos, onde salvar os resultados, qual tema usar, etc.

Crie `_quarto.yml` na raiz do projeto:
```yaml
project:
  type: default                      # Tipo de projeto (padrão para a maioria)
  output-dir: output                 # Pasta onde salvar arquivos renderizados
  
format:
  html:                             # Configurações para HTML
    theme: cosmo                    # Tema visual (cosmo, flatly, etc.)
    css: styles.css                 # CSS customizado (opcional)
    toc: true                       # Índice automático
    code-fold: show                 # Código visível mas colapsável
  pdf:                              # Configurações para PDF
    documentclass: article          # Tipo de documento LaTeX
    
execute:
  freeze: auto                      # Não re-executar código se não mudou
  cache: true                       # Salvar resultados para acelerar
```

> **Principais opções de tema**: cosmo, flatly, bootstrap, cerulean, journal, lumen, paper, readable, sandstone, simplex, spacelab, united, yeti
> 
> **Para Iniciantes**: Use este arquivo como está. Você pode experimentar temas diferentes mudando apenas a linha `theme: cosmo`

## Dicas de Produtividade

### **1. Working Directory Automático**

> **O que é Working Directory?**
> 
> É a "pasta atual" onde o R está trabalhando. Quando você usa `read.csv("dados.csv")`, o R procura o arquivo na pasta de trabalho atual. No RStudio, isso é automático. No VS Code, precisamos configurar.

Use o package `here` para paths relativos:
```r
library(here)
data <- read.csv(here("data", "raw", "dataset.csv"))
```

> **Por que usar `here()`?**
> - `read.csv("data/raw/dataset.csv")` pode dar erro se você não estiver na pasta certa
> - `here("data", "raw", "dataset.csv")` sempre funciona, independente de onde você está
> - É mais seguro e confiável

### **2. Sessão R Persistente**

> **O que é uma Sessão R Persistente?**
> 
> É manter a mesma sessão R funcionando enquanto você trabalha, em vez de reiniciar o R toda vez. É como manter o RStudio aberto - suas variáveis e dados ficam na memória.

Configure no `settings.json`:
```json
{
    "r.sessionWatcher": true,           // VS Code monitora sua sessão R
    "r.alwaysUseActiveTerminal": true   // Sempre usar o mesmo terminal
}
```

> **Benefícios:**
> - Variáveis não são perdidas entre execuções
> - Pacotes permanecem carregados
> - Mais rápido (não precisa recarregar tudo)
> 
> **Cuidado**: Às vezes é bom reiniciar a sessão R para garantir que seu código funciona "do zero"

### **3. Snippets Customizados**

> **O que são Snippets?**
> 
> Snippets são "atalhos de código" - você digita uma palavra pequena e o VS Code expande para um bloco de código completo. Por exemplo: você digita `libs` e automaticamente aparece todo o código para carregar as bibliotecas principais do R.
> 
> **Como criar**: `⌘+⇧+P` → "Configure User Snippets" → escolha a linguagem

Crie snippets R e Quarto em `⌘+⇧+P` → "Configure User Snippets":

**Para R (r.json):**
```json
{
    "Load Libraries": {
        "prefix": "libs",                    // Digite "libs" e aperte Tab
        "body": [                           // Código que será inserido:
            "library(tidyverse)",           //    Carrega tidyverse
            "library(here)",                //    Carrega here
            "library(janitor)",             //    Carrega janitor
            "$0"                            //    Cursor fica aqui no final
        ]
    }
}
```

**Para Quarto (quarto.json):**
```json
{
    "Quarto Header": {
        "prefix": "qheader",                // Digite "qheader" e aperte Tab
        "body": [
            "---",
            "title: \"$1\"",               // $1 = primeiro lugar para digitar
            "author: \"${2:Seu Nome}\"",    // $2 = segundo lugar (com valor padrão)
            "date: \"`r Sys.Date()`\"",
            "format: ",
            "  html:",
            "    code-fold: true",
            "    toc: true",
            "execute:",
            "  warning: false",
            "  message: false",
            "---",
            "",
            "$0"                            // $0 = posição final do cursor
        ]
    },
    "R Code Block": {
        "prefix": "rblock",                 // Digite "rblock" e aperte Tab
        "body": [
            "```{r}",
            "#| label: $1",                 // Nome do bloco de código
            "#| echo: true",               // Mostrar o código no relatório
            "",
            "$0",                          // Aqui você digita seu código R
            "",
            "```"
        ]
    }
}
```

> **Como usar Snippets:**
> 1. Digite o `prefix` (ex: "libs")
> 2. Aperte `Tab`
> 3. O código aparece automaticamente
> 4. Use `Tab` para pular entre os campos $1, $2, etc.

### **4. Tasks Automatizadas**

> **O que são Tasks?**
> 
> Tasks são "comandos automatizados" que você pode executar com um clique. É como ter botões personalizados no VS Code. Por exemplo: um botão para renderizar seu documento Quarto, outro para executar todos os scripts R, etc.
> 
> **Como usar**: `⌘+⇧+P` → "Tasks: Run Task" → escolha qual task executar

Crie `.vscode/tasks.json`:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Render Quarto Document",     // Nome que aparece no menu
            "type": "shell",                      // Executa comando no terminal
            "command": "quarto",                  // Programa a executar
            "args": ["render", "${file}"],       // Argumentos: render + arquivo atual
            "group": "build",                     // Grupo (organização)
            "presentation": {
                "echo": true,                     // Mostrar comando executado
                "reveal": "always"                // Sempre mostrar terminal
            }
        },
        {
            "label": "Render All Quarto",         // Renderizar todos arquivos Quarto
            "type": "shell",
            "command": "quarto",
            "args": ["render"],                   // Sem argumentos = todos os arquivos
            "group": "build"
        },
        {
            "label": "Run R Script",              // Executar script R atual
            "type": "shell",
            "command": "Rscript",                 // Comando Rscript
            "args": ["${file}"],                 // Arquivo atual
            "group": "build"
        }
    ]
}
```

> **Variáveis úteis:**
> - `${file}` = arquivo que está aberto
> - `${workspaceFolder}` = pasta do projeto
> - `${fileBasename}` = nome do arquivo (sem caminho)
> 
> **Para Iniciantes**: Copie este arquivo exatamente como está. Você pode personalizar depois!

## Inicialização Rápida

### Template de Novo Projeto:
```bash
# Script para criar novo projeto R + Quarto
mkdir $1
cd $1
mkdir -p data/{raw,processed,external} scripts notebooks functions output/{figures,tables,reports,presentations} .vscode

# Criar arquivos básicos
touch .Rprofile .gitignore .here README.md _quarto.yml
touch .vscode/settings.json .vscode/tasks.json

# Criar template de notebook Quarto
cat > notebooks/template.qmd << 'EOF'
---
title: "Template de Análise"
author: "Seu Nome"
date: "`r Sys.Date()`"
format: 
  html:
    code-fold: true
    toc: true
    theme: cosmo
execute:
  warning: false
  message: false
---

## Carregamento de Bibliotecas

```{r}
#| label: setup
library(tidyverse)
library(here)
```

## Importação de Dados

```{r}
#| label: import

```

## Análise Exploratória

```{r}
#| label: eda

```
EOF

# Inicializar Git
git init

# Abrir no VS Code
code .
```

## Vantagens do VS Code sobre RStudio

1. **Performance** - Mais rápido com arquivos grandes
2. **Flexibilidade** - Suporte a múltiplas linguagens
3. **Extensibilidade** - Ecosystem de extensões imenso
4. **Git Integration** - Melhor interface para Git
5. **Terminal Integrado** - Acesso direto ao sistema
6. **Customização** - Totalmente configurável
7. **Quarto Native** - Suporte nativo superior para Quarto
8. **Multi-format** - Renderização simultânea HTML/PDF/Word

## Recursos Adicionais

- **Documentação R Extension**: [R Extension Guide](https://github.com/REditorSupport/vscode-R)
- **Quarto Integration**: [Quarto VS Code Extension](https://quarto.org/docs/tools/vscode.html)
- **Quarto Documentation**: [Quarto Official Docs](https://quarto.org/)
- **renv Documentation**: Para ambientes isolados
- **here Package**: Para gerenciamento de paths

---

## Glossário para Iniciantes

> **Termos técnicos explicados de forma simples:**

### **Conceitos do VS Code:**
- **Workspace**: Sua "área de trabalho" - um projeto com suas configurações
- **Extension**: Programinhas que adicionam funcionalidades ao VS Code
- **Command Palette**: Menu principal do VS Code (`⌘+⇧+P`) - acesso a tudo
- **Settings.json**: Arquivo com suas preferências e configurações
- **Tasks**: Comandos automatizados que você pode executar com um clique
- **Snippets**: Atalhos de código - digita pouco, aparece muito código

### **Conceitos do R:**
- **Working Directory**: Pasta onde o R está "trabalhando" no momento
- **.Rprofile**: Arquivo que configura o R automaticamente quando inicia
- **renv**: Sistema para gerenciar versões de pacotes R (como um "freezer")
- **here Package**: Facilita trabalhar com caminhos de arquivos

### **Conceitos do Quarto:**
- **.qmd**: Arquivo Quarto - mistura texto, código e resultados
- **YAML Header**: Configurações no topo do arquivo (entre `---`)
- **Render**: Processo de transformar .qmd em HTML, PDF, etc.
- **Code Chunks**: Blocos de código R dentro do documento
- **_quarto.yml**: Arquivo de configuração do projeto Quarto

### **Conceitos do Git:**
- **.gitignore**: Lista de arquivos que o Git deve ignorar
- **Repository**: Pasta com controle de versões (histórico de mudanças)
- **Commit**: "Salvar" uma versão do seu projeto no histórico

*Dica: Comece com um projeto simples usando Quarto e vá adicionando complexidade conforme se familiariza com o VS Code!*

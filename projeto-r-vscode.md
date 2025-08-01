# Do RStudio para VS Code: Criando Projetos R

Este guia explica como replicar a experiência de **Projetos RStudio** no **VS Code**, mantendo a organização e funcionalidades que você já conhece.

## 🤔 O que é um Projeto RStudio?

No RStudio, um projeto é:
- Uma pasta com arquivo `.Rproj`
- Working directory automático
- Histórico de comandos isolado
- Environment variables específicas
- Git integration
- Package management isolado (com `renv`)

## 🎯 Equivalente no VS Code: Workspaces

No VS Code, o equivalente mais próximo são os **Workspaces**, que oferecem:

### Opção 1: **Folder Workspace** (Recomendado para R)
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

## 🛠 Configuração Passo a Passo

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

Crie `.vscode/settings.json` com configurações específicas do projeto:

```json
{
    "r.rterm.option": [],
    "r.rpath.mac": "/usr/local/bin/R",
    "r.alwaysUseActiveTerminal": true,
    "r.bracketedPaste": true,
    "r.sessionWatcher": true,
    "r.rtermSendDelay": 0,
    "files.associations": {
        "*.R": "r",
        "*.qmd": "quarto"
    },
    "workbench.startupEditor": "readme",
    "explorer.confirmDelete": true,
    "git.openRepositoryInParentFolders": "never"
}
```

### 3. **Configurar R Environment (.Rprofile)**

Crie `.Rprofile` na raiz do projeto:

```r
# .Rprofile - Configurações específicas do projeto
cat("🚀 Projeto R carregado no VS Code!\n")

# Definir working directory
if (interactive()) {
  # Garantir que estamos no diretório do projeto
  if (basename(getwd()) != "meu_projeto_r") {
    setwd(here::here())
  }
  
  # Carregar pacotes comuns (opcional)
  suppressMessages({
    library(here)      # Gerenciamento de paths
    library(renv)      # Gerenciamento de pacotes (opcional)
  })
  
  cat("📁 Working directory:", getwd(), "\n")
}

# Opções R específicas do projeto
options(
  scipen = 999,                    # Evitar notação científica
  digits = 4,                      # Precisão padrão
  stringsAsFactors = FALSE,        # Strings como character
  repos = c(CRAN = "https://cran.rstudio.com/")
)
```

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

## 🔧 Extensões Essenciais

Instale estas extensões para replicar funcionalidades do RStudio:

### **Essenciais para R e Quarto:**
```bash
# Via Command Palette (⌘+⇧+P)
ext install REditorSupport.r
ext install RDebugger.r-debugger
ext install quarto.quarto
ext install REditorSupport.r-lsp
ext install ms-toolsai.jupyter
```

### **Produtividade Geral:**
```bash
ext install ms-vscode.vscode-json
ext install ms-python.python
ext install GitLens.gitlens
ext install ms-vscode.hexeditor
```

## 📊 Workflow Comparativo

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

## 📄 Trabalhando com Quarto (.qmd)

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
Crie `_quarto.yml` na raiz do projeto:
```yaml
project:
  type: default
  output-dir: output
  
format:
  html:
    theme: cosmo
    css: styles.css
    toc: true
    code-fold: show
  pdf:
    documentclass: article
    
execute:
  freeze: auto
  cache: true
```

## 🎯 Dicas de Produtividade

### **1. Working Directory Automático**
Use o package `here` para paths relativos:
```r
library(here)
data <- read.csv(here("data", "raw", "dataset.csv"))
```

### **2. Sessão R Persistente**
Configure no `settings.json`:
```json
{
    "r.sessionWatcher": true,
    "r.alwaysUseActiveTerminal": true
}
```

### **3. Snippets Customizados**
Crie snippets R e Quarto em `⌘+⇧+P` → "Configure User Snippets":

**Para R (r.json):**
```json
{
    "Load Libraries": {
        "prefix": "libs",
        "body": [
            "library(tidyverse)",
            "library(here)",
            "library(janitor)",
            "$0"
        ]
    }
}
```

**Para Quarto (quarto.json):**
```json
{
    "Quarto Header": {
        "prefix": "qheader",
        "body": [
            "---",
            "title: \"$1\"",
            "author: \"${2:Seu Nome}\"",
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
            "$0"
        ]
    },
    "R Code Block": {
        "prefix": "rblock",
        "body": [
            "```{r}",
            "#| label: $1",
            "#| echo: true",
            "",
            "$0",
            "",
            "```"
        ]
    }
}
```

### **4. Tasks Automatizadas**
Crie `.vscode/tasks.json`:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Render Quarto Document",
            "type": "shell",
            "command": "quarto",
            "args": ["render", "${file}"],
            "group": "build",
            "presentation": {
                "echo": true,
                "reveal": "always"
            }
        },
        {
            "label": "Render All Quarto",
            "type": "shell",
            "command": "quarto",
            "args": ["render"],
            "group": "build"
        },
        {
            "label": "Run R Script",
            "type": "shell",
            "command": "Rscript",
            "args": ["${file}"],
            "group": "build"
        }
    ]
}
```

## 🚀 Inicialização Rápida

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

## 💡 Vantagens do VS Code sobre RStudio

1. **Performance** - Mais rápido com arquivos grandes
2. **Flexibilidade** - Suporte a múltiplas linguagens
3. **Extensibilidade** - Ecosystem de extensões imenso
4. **Git Integration** - Melhor interface para Git
5. **Terminal Integrado** - Acesso direto ao sistema
6. **Customização** - Totalmente configurável
7. **Quarto Native** - Suporte nativo superior para Quarto
8. **Multi-format** - Renderização simultânea HTML/PDF/Word

## 🔗 Recursos Adicionais

- **Documentação R Extension**: [R Extension Guide](https://github.com/REditorSupport/vscode-R)
- **Quarto Integration**: [Quarto VS Code Extension](https://quarto.org/docs/tools/vscode.html)
- **Quarto Documentation**: [Quarto Official Docs](https://quarto.org/)
- **renv Documentation**: Para ambientes isolados
- **here Package**: Para gerenciamento de paths

---

*💡 **Dica**: Comece com um projeto simples usando Quarto e vá adicionando complexidade conforme se familiariza com o VS Code!*

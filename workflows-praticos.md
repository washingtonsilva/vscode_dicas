# Workflows Práticos: Primeiros Passos no VS Code

*Guias passo-a-passo para colocar a teoria em prática*

## 📖 Como usar este guia

Este arquivo contém **workflows completos** para você começar a usar o VS Code imediatamente. Cada seção é um **passo-a-passo real** que você pode seguir.

> **💡 Ordem recomendada:**
> 1. 🚀 Crie seu primeiro projeto
> 2. 📊 Faça uma análise completa
> 3. 📁 Use o template para próximos projetos

---

## 🚀 Workflow 1: Criando Seu Primeiro Projeto R

### **📋 Checklist Inicial (antes de começar):**
- [ ] VS Code instalado
- [ ] Extensão R instalada (`REditorSupport.r`)
- [ ] Extensão Quarto instalada (`quarto.quarto`)
- [ ] Settings.json atualizado (do repositório `versoes_atuais/`)

### **Passo 1: Criar a Estrutura**

```bash
# 1. Criar pasta do projeto
mkdir ~/meu_primeiro_projeto_r
cd ~/meu_primeiro_projeto_r

# 2. Criar estrutura básica
mkdir -p data/{raw,processed} scripts notebooks output/{figures,reports} .vscode

# 3. Criar arquivos de configuração
touch .Rprofile .gitignore .here README.md _quarto.yml
```

### **Passo 2: Configurar o Projeto**

**2.1. Abrir no VS Code:**
```bash
code .
```

**2.2. Criar `.vscode/settings.json`:**
```json
{
    "r.alwaysUseActiveTerminal": true,
    "r.bracketedPaste": true,
    "r.sessionWatcher": true,
    "files.associations": {
        "*.R": "r",
        "*.qmd": "quarto"
    },
    "workbench.startupEditor": "readme"
}
```

**2.3. Criar `.Rprofile`:**
```r
# Configuração do projeto
cat("🚀 Projeto R iniciado no VS Code!\n")

if (interactive()) {
  # Carregar pacotes essenciais
  suppressMessages({
    if (!require(here)) install.packages("here")
    library(here)
  })
  
  cat("📁 Pasta do projeto:", here(), "\n")
}

# Opções padrão
options(
  repos = c(CRAN = "https://cran.rstudio.com/"),
  scipen = 999,
  digits = 3
)
```

**2.4. Criar `.gitignore`:**
```gitignore
# R
.Rhistory
.RData
.Ruserdata
*.Rproj

# Sistema
.DS_Store
Thumbs.db

# Output temporário
*.log
.Rproj.user/
```

**2.5. Criar `README.md`:**
```markdown
# Meu Primeiro Projeto R

Descrição: Projeto de exemplo para aprender VS Code

## Estrutura
- `data/` - Dados do projeto
- `scripts/` - Scripts R
- `notebooks/` - Documentos Quarto
- `output/` - Resultados
```

### **Passo 3: Testar a Configuração**

**3.1. Criar script de teste (`scripts/01_teste.R`):**
```r
# Teste inicial do ambiente
cat("✅ VS Code + R funcionando!\n")

# Testar pacotes
library(here)
cat("📁 Estou em:", here(), "\n")

# Criar dados de exemplo
dados_teste <- data.frame(
  x = 1:10,
  y = rnorm(10)
)

print(dados_teste)

# Salvar resultado
write.csv(dados_teste, here("data", "processed", "teste.csv"), row.names = FALSE)
cat("💾 Arquivo salvo em data/processed/\n")
```

**3.2. Executar o teste:**
1. Abra o arquivo `scripts/01_teste.R`
2. Pressione `Ctrl+` (backtick) para abrir terminal
3. Digite `R` e pressione Enter
4. Use `Ctrl+Enter` para executar linha por linha

**✅ Se tudo funcionou, você verá:**
- Mensagem de boas-vindas
- Caminho do projeto
- Dados impressos
- Arquivo salvo

---

## 📊 Workflow 2: Análise Completa Passo-a-Passo

### **Cenário Real: Análise de Vendas**

Vamos criar uma análise completa usando dados fictícios de vendas.

### **Passo 1: Preparar os Dados**

**1.1. Criar dados de exemplo (`scripts/00_gerar_dados.R`):**
```r
# Gerar dados fictícios de vendas
library(here)

set.seed(123)
vendas <- data.frame(
  data = seq.Date(from = as.Date("2024-01-01"), 
                  to = as.Date("2024-12-31"), 
                  by = "day"),
  produto = sample(c("A", "B", "C"), 365, replace = TRUE),
  vendedor = sample(c("João", "Maria", "Pedro"), 365, replace = TRUE),
  valor = round(rnorm(365, mean = 1000, sd = 300), 2),
  quantidade = sample(1:10, 365, replace = TRUE)
)

# Salvar dados
write.csv(vendas, here("data", "raw", "vendas_2024.csv"), row.names = FALSE)
cat("📊 Dados de vendas criados!\n")
```

**1.2. Executar a geração:**
- `Ctrl+Enter` linha por linha ou `Ctrl+Shift+S` para todo o arquivo

### **Passo 2: Análise Exploratória**

**2.1. Criar script de análise (`scripts/01_exploracao.R`):**
```r
# Análise Exploratória de Vendas
library(here)

# Carregar dados
vendas <- read.csv(here("data", "raw", "vendas_2024.csv"))
vendas$data <- as.Date(vendas$data)

# Resumo dos dados
cat("📊 Resumo dos dados:\n")
print(summary(vendas))

# Vendas por produto
vendas_produto <- aggregate(valor ~ produto, vendas, sum)
print(vendas_produto)

# Vendas por vendedor
vendas_vendedor <- aggregate(valor ~ vendedor, vendas, sum)
print(vendas_vendedor)

# Salvar resultados
write.csv(vendas_produto, here("data", "processed", "vendas_por_produto.csv"), row.names = FALSE)
write.csv(vendas_vendedor, here("data", "processed", "vendas_por_vendedor.csv"), row.names = FALSE)

cat("✅ Análise exploratória concluída!\n")
```

### **Passo 3: Criar Relatório Quarto**

**3.1. Criar relatório (`notebooks/relatorio_vendas.qmd`):**
```markdown
---
title: "Relatório de Vendas 2024"
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

## Carregamento de Dados

```{r}
#| label: setup
library(here)
library(ggplot2)

# Carregar dados
vendas <- read.csv(here("data", "raw", "vendas_2024.csv"))
vendas$data <- as.Date(vendas$data)
```

## Resumo Executivo

Este relatório apresenta a análise das vendas de 2024.

```{r}
#| label: resumo
cat("Total de registros:", nrow(vendas), "\n")
cat("Valor total vendido: R$", format(sum(vendas$valor), big.mark = ".", decimal.mark = ","), "\n")
```

## Vendas por Produto

```{r}
#| label: fig-produtos
#| fig-cap: "Vendas por Produto"

vendas_produto <- aggregate(valor ~ produto, vendas, sum)

ggplot(vendas_produto, aes(x = produto, y = valor)) +
  geom_col(fill = "steelblue") +
  labs(title = "Vendas por Produto",
       x = "Produto", 
       y = "Valor (R$)") +
  theme_minimal()
```

## Evolução Temporal

```{r}
#| label: fig-temporal
#| fig-cap: "Evolução das Vendas ao Longo do Ano"

vendas_mensal <- aggregate(valor ~ format(data, "%Y-%m"), vendas, sum)
names(vendas_mensal) <- c("mes", "valor")
vendas_mensal$mes <- as.Date(paste0(vendas_mensal$mes, "-01"))

ggplot(vendas_mensal, aes(x = mes, y = valor)) +
  geom_line(color = "darkgreen", size = 1) +
  geom_point(color = "darkgreen") +
  labs(title = "Evolução Mensal das Vendas",
       x = "Mês", 
       y = "Valor (R$)") +
  theme_minimal() +
  scale_x_date(date_labels = "%b", date_breaks = "1 month")
```

## Conclusões

- Produto A teve maior volume de vendas
- Vendas mantiveram-se estáveis ao longo do ano
- Necessário investigar sazonalidade
```

**3.2. Renderizar o relatório:**
1. Abra o arquivo `.qmd`
2. Pressione `Ctrl+Shift+K` para renderizar
3. Ou use Command Palette: `⌘+⇧+P` → "Quarto: Render"

### **✅ Resultado Final:**
- Scripts R funcionais
- Dados processados salvos
- Relatório HTML profissional
- Gráficos interativos

---

## 📁 Workflow 3: Template Reutilizável

### **Script de Criação Automática**

Salve este script como `criar_projeto_r.sh`:

```bash
#!/bin/bash

# Script para criar novo projeto R no VS Code
# Uso: ./criar_projeto_r.sh nome_do_projeto

PROJECT_NAME=$1

if [ -z "$PROJECT_NAME" ]; then
    echo "❌ Erro: Forneça o nome do projeto"
    echo "Uso: ./criar_projeto_r.sh meu_projeto"
    exit 1
fi

echo "🚀 Criando projeto: $PROJECT_NAME"

# Criar estrutura
mkdir -p "$PROJECT_NAME"/{data/{raw,processed,external},scripts,notebooks,functions,output/{figures,tables,reports},.vscode}

cd "$PROJECT_NAME"

# Arquivos de configuração
cat > .Rprofile << 'EOF'
# Configuração do projeto
cat("🚀 Projeto R iniciado no VS Code!\n")

if (interactive()) {
  suppressMessages({
    if (!require(here)) install.packages("here")
    library(here)
  })
  cat("📁 Pasta do projeto:", here(), "\n")
}

options(
  repos = c(CRAN = "https://cran.rstudio.com/"),
  scipen = 999,
  digits = 3
)
EOF

cat > .gitignore << 'EOF'
# R
.Rhistory
.RData
.Ruserdata
*.Rproj

# Sistema
.DS_Store
Thumbs.db

# Output
*.log
.Rproj.user/
EOF

cat > README.md << EOF
# $PROJECT_NAME

Descrição do projeto aqui.

## Estrutura
- \`data/\` - Dados do projeto
- \`scripts/\` - Scripts R
- \`notebooks/\` - Documentos Quarto
- \`output/\` - Resultados

## Como usar
1. Abrir no VS Code: \`code .\`
2. Executar scripts na ordem numérica
3. Renderizar relatórios com Ctrl+Shift+K
EOF

cat > .vscode/settings.json << 'EOF'
{
    "r.alwaysUseActiveTerminal": true,
    "r.bracketedPaste": true,
    "r.sessionWatcher": true,
    "files.associations": {
        "*.R": "r",
        "*.qmd": "quarto"
    },
    "workbench.startupEditor": "readme"
}
EOF

cat > _quarto.yml << 'EOF'
project:
  type: default
  output-dir: output

format:
  html:
    theme: cosmo
    toc: true
    code-fold: show
  
execute:
  freeze: auto
  cache: true
EOF

# Script template
cat > scripts/01_setup.R << 'EOF'
# Setup do projeto
library(here)

cat("✅ Projeto configurado!\n")
cat("📁 Diretório:", here(), "\n")

# Instalar pacotes se necessário
# install.packages(c("tidyverse", "ggplot2"))
EOF

# Notebook template
cat > notebooks/relatorio.qmd << 'EOF'
---
title: "Relatório"
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

## Setup

```{r}
#| label: setup
library(here)
# library(tidyverse)
```

## Análise

Seu conteúdo aqui...

```{r}
#| label: analise
# Seu código aqui
```
EOF

# Arquivo .here (marcador de raiz)
touch .here

echo "✅ Projeto '$PROJECT_NAME' criado com sucesso!"
echo "📂 Para abrir: cd $PROJECT_NAME && code ."
```

### **Como usar o template:**

```bash
# Tornar executável
chmod +x criar_projeto_r.sh

# Criar novo projeto
./criar_projeto_r.sh analise_financeira

# Abrir no VS Code
cd analise_financeira && code .
```

---

## 💡 Dicas de Produtividade

### **🔄 Workflow Diário Típico:**

1. **Abrir projeto**: `code .` no terminal
2. **Verificar status**: README abre automaticamente
3. **Criar/editar script**: Use `Ctrl+N` para novo arquivo
4. **Executar código**: `Ctrl+Enter` linha por linha
5. **Salvar resultados**: Sempre em pastas organizadas
6. **Criar relatório**: Arquivo `.qmd` com `Ctrl+Shift+K`
7. **Commit mudanças**: Git integrado no VS Code

### **⚡ Atalhos para Velocidade:**

- `Ctrl+P` → Abrir arquivo rapidamente
- `Ctrl+Shift+P` → Command Palette
- `Alt+-` → Operador `<-` 
- `Ctrl+Shift+M` → Pipe `|>`
- `Ctrl+Alt+I` → Chunk R em Quarto
- `Ctrl+`` → Terminal toggle

### **📋 Checklist de Projeto:**

- [ ] Estrutura de pastas criada
- [ ] `.Rprofile` configurado
- [ ] Settings.json do projeto
- [ ] README.md descritivo
- [ ] Git inicializado (`git init`)
- [ ] Primeiro script funcionando
- [ ] Primeiro relatório renderizando

---

## 🎯 Próximos Passos

Após dominar estes workflows:

1. **Explore extensões adicionais** (GitLens, Bracket Pair Colorizer)
2. **Customize themes** para sua preferência
3. **Crie snippets personalizados** para seu código comum
4. **Configure tasks automáticas** para tarefas repetitivas
5. **Integre com GitHub** para backup automático

**🚀 Com estes workflows, você está pronto para ser produtivo no VS Code desde o primeiro dia!**

# Visual Studio Code - Visão Geral Didática

*Guia introdutório para VS Code 1.102.3 (Universal) - Transição do RStudio*

## O que é o Visual Studio Code?

O **Visual Studio Code** (VS Code) é um editor de código leve, mas poderoso, desenvolvido pela Microsoft. Diferente de uma IDE tradicional como o RStudio, o VS Code é:

- **Extensível** - Funcionalidades via extensões
- **Multiplataforma** - Windows, macOS, Linux  
- **Multi-linguagem** - R, Python, JavaScript, etc.
- **Gratuito e Open Source**
- **Altamente personalizável**

## Arquitetura do VS Code

### **Interface Principal**
```
┌─────────────────────────────────────────────────────────────┐
│  Menu Bar                                                   │
├─────────────────────────────────────────────────────────────┤
│ Activity │                                    │ Minimap      │
│ Bar      │        Editor Area                │              │
│          │   ┌─────────┬─────────┬─────────┐ │              │
│ ├─ Explorer│   │ Tab 1   │ Tab 2   │ Tab 3   │ │              │
│ ├─ Search │   ├─────────┴─────────┴─────────┤ │              │
│ ├─ Git    │   │                             │ │              │
│ ├─ Debug  │   │     Código aqui             │ │              │
│ └─ Exten. │   │                             │ │              │
│          │   │                             │ │              │
├──────────┼───┴─────────────────────────────┴─┼──────────────┤
│          │      Terminal / Problems / Output │              │
│          │                                   │              │
└─────────────────────────────────────────────────────────────┘
```

### **Principais Áreas:**

1. **Activity Bar** (esquerda) - Navegação principal
2. **Sidebar** - Explorador de arquivos, busca, controle de versão
3. **Editor Area** - Onde você escreve código
4. **Minimap** (direita) - Mapa de navegação do código
5. **Terminal/Panel** (inferior) - Terminal integrado e saídas

## Conceitos Fundamentais

### **1. Workspaces vs Pastas**
| Conceito | Descrição | Equivalente RStudio |
|----------|-----------|---------------------|
| **Folder** | Pasta simples aberta | Projeto RStudio básico |
| **Workspace** | Configuração com múltiplas pastas | Projeto multi-módulo |
| **Settings** | Configurações específicas | Project Options |

### **2. Extensões - O Coração do VS Code**
O VS Code por si só é "vazio". As funcionalidades vêm das extensões:

```
VS Code Básico = Editor de Texto
VS Code + R Extension = IDE R
VS Code + Python Extension = IDE Python  
VS Code + Quarto Extension = Editor Quarto
```

### **3. Command Palette (⌘+⇧+P)**
O **centro de comando** do VS Code - acesso a TUDO:
- Abrir arquivos: `> Open File`
- Instalar extensões: `> Extensions: Install`
- Configurações: `> Preferences: Open Settings`
- Git: `> Git: Clone`

## Sistema de Configuração

### **Hierarquia de Configurações:**
```
1. Configurações Padrão (VS Code)
           ↓
2. Configurações do Usuário (~/.vscode/settings.json)
           ↓  
3. Configurações do Workspace (.vscode/settings.json)
           ↓
4. Configurações da Pasta de Trabalho
```

### **Principais Arquivos de Configuração:**
| Arquivo | Localização | Função |
|---------|-------------|--------|
| `settings.json` | `~/.vscode/` ou `.vscode/` | Configurações gerais |
| `keybindings.json` | `~/.vscode/` | Atalhos personalizados |
| `tasks.json` | `.vscode/` | Automatização de tarefas |
| `launch.json` | `.vscode/` | Configurações de debug |

## Personalização

### **Temas e Aparência**
- **Tema de Cores**: `⌘+K ⌘+T` para alternar
- **Ícones**: Diferentes pacotes de ícones
- **Font**: Configurável via settings
- **Layout**: Painéis reorganizáveis

### **Configurações Essenciais para R/Quarto:**
```json
{
  "workbench.colorTheme": "Default Dark+",
  "editor.fontSize": 14,
  "editor.fontFamily": "SF Mono, Monaco, 'Cascadia Code'",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.wordWrap": "on",
  "files.autoSave": "afterDelay",
  "terminal.integrated.fontSize": 13,
  "r.alwaysUseActiveTerminal": true,
  "quarto.render.renderOnSave": false
}
```

## Workflows Essenciais

### **1. Fluxo de Trabalho Básico**
```
1. Abrir pasta (⌘+O)
2. Explorar arquivos (Activity Bar)
3. Editar código (Editor)
4. Executar no terminal (⌃+`)
5. Ver resultados (Terminal/Output)
```

### **2. Fluxo com Git**
```
1. Inicializar repositório (⌘+⇧+P → Git: Initialize)
2. Fazer mudanças (Editor)
3. Ver changes (Git icon na Activity Bar)
4. Stage changes (+ ao lado dos arquivos)
5. Commit (✓ com mensagem)
6. Push (... → Push)
```

### **3. Fluxo R/Quarto**
```
1. Abrir projeto R
2. Instalar extensão R
3. Abrir terminal R (⌃+`)
4. Executar código (⌘+⏎)
5. Ver resultados (R plots/output)
6. Renderizar Quarto (⌘+⇧+K)
```

## Funcionalidades Avançadas

### **1. Multi-cursor e Seleção**
- **Múltiplos cursores**: `⌥+click`
- **Selecionar palavra**: `⌘+D` (repetir para próxima)
- **Selecionar todas ocorrências**: `⌘+⇧+L`
- **Seleção em coluna**: `⌥+⇧+arrastar`

### **2. Busca e Navegação**
- **Buscar arquivos**: `⌘+P`
- **Buscar no projeto**: `⌘+⇧+F`
- **Ir para linha**: `⌃+G`
- **Ir para símbolo**: `⌘+⇧+O`

### **3. Refactoring e Code Actions**
- **Quick Fix**: `⌘+.`
- **Rename Symbol**: `F2`
- **Format Document**: `⌥+⇧+F`
- **Organizar imports**: `⌥+⇧+O`

## VS Code vs RStudio - Comparação Prática

| Aspecto | RStudio | VS Code |
|---------|---------|---------|
| **Filosofia** | IDE especializada em R | Editor universal + extensões |
| **Setup** | Pronto para R | Requer configuração |
| **Performance** | Limitado com arquivos grandes | Excelente performance |
| **Linguagens** | R + Python básico | Qualquer linguagem |
| **Git** | Interface básica | Interface avançada |
| **Terminal** | Console R integrado | Terminal completo do sistema |
| **Extensibilidade** | Limitada | Ilimitada |
| **Curva de Aprendizado** | Suave para R | Inicial íngreme, depois produtiva |

## Configuração Inicial Recomendada

### **1. Extensões Essenciais (ordem de instalação):**
```bash
# Core
ext install ms-vscode.vscode-json
ext install ms-vscode.theme-default

# R/Stats
ext install REditorSupport.r
ext install RDebugger.r-debugger  
ext install quarto.quarto

# Produtividade
ext install GitLens.gitlens
ext install ms-vscode.vscode-color-information
ext install bradlc.vscode-tailwindcss
```

### **2. Settings.json Inicial:**
```json
{
  "workbench.startupEditor": "readme",
  "editor.minimap.enabled": true,
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "files.autoSave": "afterDelay",
  "terminal.integrated.fontSize": 13,
  "git.openRepositoryInParentFolders": "never",
  "r.alwaysUseActiveTerminal": true,
  "r.bracketedPaste": true
}
```

### **3. Estrutura de Workspace Recomendada:**
```
meu_projeto/
├── .vscode/
│   ├── settings.json        # Configurações do projeto
│   ├── tasks.json          # Tasks automatizadas
│   └── launch.json         # Debug config
├── data/                   # Dados
├── scripts/                # Scripts R
├── notebooks/              # Documentos Quarto
├── functions/              # Funções customizadas
├── output/                 # Resultados
├── .gitignore             # Git ignore
├── .Rprofile              # Config R do projeto
├── _quarto.yml            # Config Quarto
└── README.md              # Documentação
```

## Próximos Passos para Dominar o VS Code

### **Fase 1: Familiarização (1-2 semanas)**
- [ ] Explorar interface básica
- [ ] Instalar extensões essenciais  
- [ ] Configurar primeiro projeto
- [ ] Aprender atalhos básicos

### **Fase 2: Produtividade (2-4 semanas)**
- [ ] Dominar Command Palette
- [ ] Configurar snippets personalizados
- [ ] Usar multi-cursor efetivamente
- [ ] Integrar workflow Git

### **Fase 3: Maestria (1-2 meses)**
- [ ] Criar tasks automatizadas
- [ ] Configurar debug avançado
- [ ] Explorar extensões específicas
- [ ] Otimizar configurações pessoais

## Dicas de Transição do RStudio

### **Para se sentir em casa:**
1. **Use o Terminal** como console R (`⌃+``, depois `R`)
2. **Configure shortcuts** similares ao RStudio
3. **Use R Extension** para environment e plots
4. **Mantenha estrutura** de projetos similar
5. **Aproveite** funcionalidades extras (Git, multi-linguagem)

### **O que você ganhará:**
- **Performance superior** com arquivos grandes
- **Flexibilidade** para outras linguagens
- **Controle total** sobre o ambiente
- **Ecosystem rico** de extensões
- **Integração Git** avançada
- **Customização** ilimitada

---

## Recursos para Continuar Aprendendo

- **Documentação Oficial**: [code.visualstudio.com](https://code.visualstudio.com/docs)
- **R Extension Guide**: [Guia específico para R](https://github.com/REditorSupport/vscode-R)
- **Quarto + VS Code**: [Integração oficial](https://quarto.org/docs/tools/vscode.html)
- **Marketplace**: [Extensões](https://marketplace.visualstudio.com/)

*Lembre-se: O VS Code é uma ferramenta que cresce com você. Comece simples e vá adicionando complexidade conforme sua necessidade!*

# Abrindo Arquivos de Configuração do VS Code pelo Terminal (macOS)

Este guia mostra como abrir os arquivos de configuração do **Visual Studio Code** diretamente no próprio editor, utilizando o terminal do macOS (incluindo o terminal integrado do VS Code).

---

## 1. Verifique o comando `code` no PATH

Para usar o comando `code` no terminal, é necessário habilitá-lo:

1. Abra o **VS Code**.
2. Pressione `Cmd + Shift + P` para abrir a **Command Palette**.
3. Digite e selecione:

```
Shell Command: Install 'code' command in PATH
```

Após isso, o comando `code` estará disponível no seu terminal.

---

## 2. Abrir o `settings.json`

O arquivo `settings.json` contém as **configurações do VS Code**.  
Para abri-lo no editor, execute:

```bash
code ~/Library/Application\ Support/Code/User/settings.json
```

---

## 3. Abrir o `keybindings.json`

O arquivo `keybindings.json` contém os **atalhos personalizados**.
Para abri-lo no editor, execute:

```bash
code ~/Library/Application\ Support/Code/User/keybindings.json
```

---

## 4. Outros arquivos úteis

### **Snippets personalizados:**
```bash
code ~/Library/Application\ Support/Code/User/snippets/
```

### **Configurações de workspace:**
```bash
code .vscode/settings.json  # Na pasta do projeto
```

---

## 5. Observações

* O caractere de **espaço** em `Application Support` deve ser **escapado com `\`**, conforme mostrado nos comandos.
* Use `code --help` para ver todas as opções disponíveis do comando.

---

## 6. Dica

Mantenha um backup destes arquivos (`settings.json` e `keybindings.json`) em um repositório Git para preservar suas configurações e facilitar a migração para outros dispositivos.

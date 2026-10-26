# 01 - Hello World

Este é o programa mais básico em Python - o famoso "Olá, Mundo!"

## 📝 Sobre

Todo programador começa sua jornada com um programa Hello World. É uma tradição na computação e serve para:

- Verificar se o ambiente de desenvolvimento está configurado
- Entender a estrutura básica de um programa
- Fazer seu primeiro código funcionar!

## 📂 Arquivos

- `main.py` - código fonte do programa

## 🚀 Como Executar

Dentro desta pasta:

```bash
python main.py     # Windows
python3 main.py    # macOS e Linux
```

### Saída Esperada

```
Olá, Mundo!
Bem-vindo ao Python!
Este é meu primeiro programa.
```

## 🔍 Entendendo o Código

```python
"""Programa mais básico em Python: imprime mensagens no terminal."""
```

- Uma **docstring** de módulo: texto entre aspas triplas que documenta o arquivo

```python
def main() -> None:
```

- `def` - palavra-chave para declarar uma função
- `-> None` - type hint dizendo que a função não retorna valor
- `:` - abre o bloco da função; o conteúdo vem indentado com 4 espaços

```python
print("Olá, Mundo!")
```

- `print` - função embutida que imprime o texto e pula linha

```python
if __name__ == "__main__":
    main()
```

- Só chama `main()` quando o arquivo é executado diretamente, e não quando é importado por outro módulo

## 💡 Conceitos Importantes

### 1. Indentação define os blocos

Não há chaves `{ }`: tudo que está indentado abaixo do `def` faz parte da função.

### 2. Sem ponto e vírgula

O fim da linha encerra a instrução.

### 3. Script direto também funciona

Uma única linha `print("Olá, Mundo!")` já é um programa Python válido. A função `main` e o `if __name__ == "__main__"` são uma convenção que facilita organizar e reutilizar o código à medida que ele cresce.

## 🎯 Experimente

1. **Mude a mensagem:**

   ```python
   print("Seu nome aqui!")
   ```

2. **Use uma f-string:**

   ```python
   nome = "Caramelo"
   print(f"Olá, {nome}!")
   ```

3. **Leia um valor do teclado:**

   ```python
   nome = input("Qual é o seu nome? ")
   print(f"Olá, {nome}!")
   ```

## 🔗 Conceitos Relacionados

- [📖 Configurando o Ambiente](../../notes/fundamentos/01-ambiente.md)
- [📖 Introdução ao Python](../../notes/fundamentos/02-introducao.md)

## ❓ Problemas Comuns

### Erro: "'python' não é reconhecido" / "python: command not found"

**Solução:** o Python não está no `PATH`. No Windows, tente `py main.py`; no macOS e Linux, use `python3`.
Veja: [Configurando o Ambiente](../../notes/fundamentos/01-ambiente.md)

### Erro: "IndentationError: expected an indented block"

**Solução:** o conteúdo da função precisa estar indentado (4 espaços) abaixo do `def main() -> None:`.

### Caracteres acentuados aparecem errados no terminal (Windows)

**Solução:** use o Windows Terminal ou rode `chcp 65001` antes de executar o programa.

**[⬅️ Voltar aos Exemplos](../README.md)**

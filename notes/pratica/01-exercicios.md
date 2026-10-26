# Exercícios

Pratique o que aprendeu nos conteúdos e exemplos de código.

## Como usar

1. Leia o enunciado do exercício
2. Tente resolver por conta própria antes de buscar ajuda
3. Revise as notas de [Fundamentos](/labs/python/fundamentos/01-ambiente/) se precisar relembrar algum conceito
4. Rode `ruff format` e `ruff check` na sua solução antes de considerar o exercício concluído
5. Compare sua solução com a de outras pessoas da comunidade

## Exercício 01 - Olá, Python

**Nível:** iniciante

**Objetivo:** Configurar um projeto com ambiente virtual e executar o primeiro programa.

**Instrução:**

1. Crie uma pasta `ola-python` e, dentro dela, um ambiente virtual com `python -m venv .venv`
2. Ative o ambiente e escreva um `main.py` que imprima `Olá, Caramelo Tech!`
3. Execute com `python main.py`

**Critérios de sucesso:**

- [ ] A pasta contém um `.venv` criado com `venv`
- [ ] O prompt mostra `(.venv)` quando o ambiente está ativo
- [ ] `python main.py` imprime a mensagem corretamente

## Exercício 02 - Variáveis e f-strings

**Nível:** iniciante

**Objetivo:** Ler dados do usuário, converter tipos e formatar a saída.

**Instrução:**

1. Leia o nome, a idade e a altura (em metros) com `input`
2. Converta a idade para `int` e a altura para `float`
3. Imprima uma frase combinando os três valores com uma f-string

Saída esperada:

```
Meu nome é Ana, tenho 25 anos e meço 1.68m.
```

**Critérios de sucesso:**

- [ ] Usa `int()` e `float()` para converter a entrada
- [ ] Formata a altura com duas casas decimais (`{altura:.2f}`)
- [ ] Usa type hints nas variáveis ou em uma função auxiliar

## Exercício 03 - FizzBuzz

**Nível:** iniciante

**Objetivo:** Praticar laços e condicionais.

**Instrução:**
Imprima os números de 1 a 100. Para múltiplos de 3, imprima `Fizz` no lugar do número; para múltiplos de 5, `Buzz`; para múltiplos de ambos, `FizzBuzz`.

**Critérios de sucesso:**

- [ ] Usa `for` com `range(1, 101)`
- [ ] A lógica fica em uma função `fizzbuzz(n: int) -> str`
- [ ] A saída está correta para 3, 5, 15 e 100

## Exercício 04 - Divisão Segura

**Nível:** iniciante

**Objetivo:** Tratar erros com exceções.

**Instrução:**
Escreva um programa que leia dois números e mostre o resultado da divisão. Trate a divisão por zero e entradas que não são números, mostrando mensagens claras sem encerrar o programa com erro.

**Critérios de sucesso:**

- [ ] Captura `ZeroDivisionError` e `ValueError` em blocos `except` separados
- [ ] Não usa `except:` genérico
- [ ] Pede os números de novo quando a entrada é inválida

## Exercício 05 - Contador de Palavras

**Nível:** intermediário

**Objetivo:** Praticar strings, dicionários e testes.

**Instrução:**
Escreva a função `contar_palavras(texto: str) -> dict[str, int]` que conta quantas vezes cada palavra aparece (ignorando maiúsculas e minúsculas) e imprima as palavras em ordem alfabética com suas contagens.

**Critérios de sucesso:**

- [ ] Usa `str.split` e `str.lower`
- [ ] Usa `dict` ou `collections.Counter` para as contagens
- [ ] Ordena as palavras com `sorted` antes de imprimir
- [ ] Tem ao menos um teste com `pytest` validando a contagem

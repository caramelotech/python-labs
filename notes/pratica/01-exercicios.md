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

## Exercício 06 - Pirâmide de Asteriscos

**Nível:** iniciante

**Objetivo:** Praticar laços aninhados.

**Instrução:**
Escreva a função `piramide(altura: int) -> None` que imprime uma pirâmide de asteriscos centralizada com a altura informada. Para `altura = 4`:

```
   *
  ***
 *****
*******
```

**Critérios de sucesso:**

- [ ] Usa dois laços `for` aninhados (um para as linhas, outro para os caracteres de cada linha)
- [ ] A pirâmide fica centralizada (espaços à esquerda calculados a partir da altura)
- [ ] Funciona para qualquer `altura` positiva, não só para 4

## Exercício 07 - Número Primo, Perfeito ou Nenhum dos Dois

**Nível:** iniciante

**Objetivo:** Praticar condicionais, laços e funções, com um pouco de teoria dos números.

**Instrução:**
Escreva duas funções: `eh_primo(numero: int) -> bool` (retorna `True` se o número só é divisível por 1 e por ele mesmo) e `eh_perfeito(numero: int) -> bool` (retorna `True` se a soma de todos os divisores do número, exceto ele mesmo, é igual ao próprio número - por exemplo, 28 é perfeito porque 1 + 2 + 4 + 7 + 14 = 28). Depois, para os números de 1 a 50, imprima se cada um é primo, perfeito, os dois, ou nenhum.

**Critérios de sucesso:**

- [ ] `eh_primo` não precisa testar divisores maiores que a raiz quadrada do número
- [ ] `eh_perfeito` não conta o próprio número como divisor de si mesmo
- [ ] O programa cobre os casos especiais 0 e 1 corretamente (nenhum dos dois é primo)

## Exercício 08 - Anagrama e Palíndromo

**Nível:** iniciante

**Objetivo:** Praticar manipulação de strings.

**Instrução:**
Escreva `sao_anagramas(a: str, b: str) -> bool` (retorna `True` se uma palavra é formada pelas mesmas letras da outra, em qualquer ordem, ignorando maiúsculas/minúsculas e espaços) e `eh_palindromo(texto: str) -> bool` (retorna `True` se o texto é igual lido de trás para frente, ignorando espaços, acentos e maiúsculas/minúsculas).

**Critérios de sucesso:**

- [ ] `sao_anagramas("Roma", "Amor")` retorna `True`
- [ ] `eh_palindromo("arara")` e `eh_palindromo("A base do teto desaba")` retornam `True`
- [ ] Usa fatiamento (`[::-1]`) ou `sorted()` em vez de reimplementar a comparação na mão

## Exercício 09 - Hierarquia de Animais

**Nível:** intermediário

**Objetivo:** Praticar herança e polimorfismo.

**Instrução:**
Crie uma classe base `Animal` com atributo `nome` e método `emitir_som() -> str` (retorna `"..."` por padrão). Crie as classes `Cachorro` e `Gato`, herdando de `Animal`, cada uma sobrescrevendo `emitir_som()` com seu próprio som. Escreva uma função `apresentar(animais: list[Animal]) -> None` que percorre uma lista de animais de tipos diferentes e imprime o som de cada um, sem precisar checar o tipo de cada animal com `if`.

**Critérios de sucesso:**

- [ ] `Cachorro` e `Gato` herdam de `Animal` com `class Cachorro(Animal):`
- [ ] `apresentar` funciona com uma lista que mistura `Cachorro` e `Gato`, sem nenhum `isinstance`/`if` checando o tipo
- [ ] Adicionar um terceiro animal (`Pato`, por exemplo) não exige mudar nada em `apresentar`

## Exercício 10 - Decorator Cronômetro

**Nível:** intermediário

**Objetivo:** Praticar decorators e closures.

**Instrução:**
Escreva um decorator `cronometrar` que, aplicado a qualquer função, imprime quanto tempo ela levou para executar, sem alterar o valor que a função retorna. Use-o para medir uma função que soma os números de 1 a 10 milhões.

**Critérios de sucesso:**

- [ ] O decorator funciona com `@cronometrar` acima da função, sem parênteses
- [ ] Funciona com funções que recebem qualquer quantidade de argumentos (`*args`, `**kwargs`)
- [ ] Usa `functools.wraps` para preservar o nome original da função decorada
- [ ] A função decorada continua retornando seu valor normalmente (o decorator só adiciona a medição)

# Sintaxe e Tipos Básicos

Antes de ir além do "Olá, mundo", vale conhecer as peças que formam qualquer programa Python: como o interpretador lê os blocos de código, quais tipos de dado existem por padrão e como combiná-los com operadores.

## Indentação e blocos

Em outras linguagens, chaves `{}` marcam onde um bloco de código começa e termina. Em Python, isso é feito pela indentação - o espaço em branco no início da linha.

```python
idade = 20

if idade >= 18:
    print("Pode entrar")
    print("Maioridade confirmada")
else:
    print("Não pode entrar")
```

As duas linhas dentro do `if` têm o mesmo recuo (4 espaços, o padrão recomendado pela [PEP 8](https://peps.python.org/pep-0008/)) porque pertencem ao mesmo bloco. Misturar tabs e espaços, ou usar recuos diferentes dentro do mesmo bloco, gera `IndentationError`.

Comentários servem para explicar o "porquê" de um trecho de código, não o "o quê" (isso o próprio código já diz):

```python
# comentário de uma linha
valor = 10  # comentário ao lado do código

"""
Comentário de várias linhas.
Na prática é uma string que não foi atribuída a nada,
mas é a forma mais comum de comentar blocos grandes.
"""
```

## Tipos básicos

Python tem cinco tipos embutidos que aparecem em praticamente todo programa:

| Tipo       | Exemplo         | Representa                |
| ---------- | --------------- | ------------------------- |
| `int`      | `42`, `-7`      | números inteiros          |
| `float`    | `3.14`, `-0.5`  | números com ponto decimal |
| `str`      | `"Caramelo"`    | texto                     |
| `bool`     | `True`, `False` | valor lógico              |
| `NoneType` | `None`          | ausência de valor         |

Para descobrir o tipo de qualquer valor, use `type()`:

```python
>>> type(42)
<class 'int'>
>>> type(3.14)
<class 'float'>
>>> type("Caramelo")
<class 'str'>
>>> type(None)
<class 'NoneType'>
```

Python tem **tipagem dinâmica**: você não declara o tipo de uma variável, ele é inferido do valor atribuído, e pode mudar durante a execução:

```python
valor = 42        # int
valor = "texto"   # agora é str, sem erro nenhum
```

Isso não significa que Python ignora tipos. Ele é **fortemente tipado**: tentar somar um número com um texto sempre gera um erro, nunca uma conversão silenciosa como acontece em JavaScript:

```python
>>> 1 + "1"
Traceback (most recent call last):
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

## Conversão de tipos

Quando você precisa transformar um valor de um tipo para outro, use as próprias funções `int()`, `float()`, `str()` e `bool()`:

```python
idade_texto = "25"
idade = int(idade_texto)      # 25 (int)
altura = float("1.68")        # 1.68 (float)
mensagem = str(idade)         # "25" (str)
```

Isso é necessário o tempo todo porque `input()` sempre devolve uma `str`, mesmo se a pessoa digitar um número:

```python
idade = int(input("Qual sua idade? "))
```

Algumas conversões falham e levantam exceção em vez de "adivinhar" o que você quis dizer:

```python
>>> int("3.14")
Traceback (most recent call last):
ValueError: invalid literal for int() with base 10: '3.14'
```

`int()` não aceita uma string com ponto decimal direto - seria preciso passar por `float()` primeiro (`int(float("3.14"))`). Já `bool()` segue uma regra simples: qualquer valor "vazio" (`0`, `0.0`, `""`, `None`, listas/dicionários vazios) converte para `False`; o resto converte para `True`.

## Operadores

### Aritméticos

```python
7 + 3   # 10  soma
7 - 3   # 4   subtração
7 * 3   # 21  multiplicação
7 / 3   # 2.3333...  divisão (sempre retorna float)
7 // 3  # 2   divisão inteira (descarta o resto)
7 % 3   # 1   resto da divisão (módulo)
7 ** 3  # 343 potenciação
```

Um erro comum de quem vem de outras linguagens: `/` em Python 3 **sempre** retorna `float`, mesmo dividindo dois inteiros exatos (`6 / 3` é `2.0`, não `2`). Para divisão inteira, use `//`.

### Comparação

```python
5 == 5   # True  igual
5 != 3   # True  diferente
5 > 3    # True
5 < 3    # False
5 >= 5   # True
5 <= 3   # False
```

### Atribuição

```python
x = 10
x += 5   # x = x + 5  -> 15
x -= 3   # x = x - 3  -> 12
x *= 2   # x = x * 2  -> 24
x /= 4   # x = x / 4  -> 6.0
x //= 2  # x = x // 2 -> 3.0
x **= 2  # x = x ** 2 -> 9.0
```

### Lógicos

```python
True and False   # False
True or False     # True
not True          # False
```

Python avalia `and`/`or` com **short-circuit**: em `a and b`, se `a` for falso, `b` nem é avaliado. Isso é usado como atalho comum, por exemplo para evitar `None`:

```python
nome = usuario and usuario.nome  # só acessa .nome se usuario não for None/vazio
```

### Identidade vs igualdade

Esse é o ponto que mais confunde quem está começando: `==` compara **valor**, `is` compara **identidade** (se são o mesmo objeto na memória).

```python
a = [1, 2, 3]
b = [1, 2, 3]
a == b   # True  -> mesmo conteúdo
a is b   # False -> objetos diferentes na memória

c = a
c is a   # True  -> c e a apontam para o mesmo objeto
```

Na prática, use `is` só para comparar com `None`, `True` e `False` (e nunca `==` para isso):

```python
if valor is None:
    ...
```

### Associação (pertencimento)

```python
"a" in "abc"        # True
3 in [1, 2, 3]      # True
"x" not in "abc"    # True
```

### Bit a bit

Operam diretamente nos bits da representação binária do número - menos comuns no dia a dia, mas aparecem em flags, protocolos de rede e otimizações:

```python
5 & 3   # 1    AND bit a bit
5 | 3   # 7    OR bit a bit
5 ^ 3   # 6    XOR bit a bit
~5      # -6   NOT bit a bit
5 << 1  # 10   shift à esquerda (multiplica por 2)
5 >> 1  # 2    shift à direita (divide por 2)
```

### Precedência

Quando uma expressão mistura operadores, Python segue uma ordem fixa (do maior para o menor nível de prioridade): parênteses, potenciação, multiplicação/divisão, soma/subtração, comparação, `not`, `and`, `or`. Na dúvida, use parênteses - deixa a intenção explícita e evita ter que decorar a tabela:

```python
resultado = 2 + 3 * 4      # 14, multiplicação primeiro
resultado = (2 + 3) * 4    # 20, parênteses primeiro
```

## f-strings

Desde o Python 3.6 ([PEP 498](https://peps.python.org/pep-0498/)), a forma recomendada de montar strings com valores de variáveis é a f-string: um `f` antes das aspas, com as expressões entre `{}`.

```python
nome = "Ana"
idade = 25
print(f"{nome} tem {idade} anos")
# Ana tem 25 anos
```

Dentro das chaves você pode colocar qualquer expressão válida, não só nomes de variáveis:

```python
print(f"Ano que vem terei {idade + 1} anos")
print(f"{nome.upper()}")
```

Um especificador de formatação depois de `:` controla como o valor aparece - útil para casas decimais, separador de milhar e alinhamento:

```python
altura = 1.6829
print(f"{altura:.2f}")       # 1.68 (duas casas decimais)

populacao = 1500000
print(f"{populacao:,}")      # 1,500,000 (separador de milhar)

print(f"{nome:>10}")         # alinha à direita em 10 colunas
```

E para depurar rapidamente, o modificador `=` (desde o Python 3.8) imprime a expressão e o valor juntos:

```python
print(f"{idade=}")
# idade=25
```

Desde o Python 3.12 ([PEP 701](https://peps.python.org/pep-0701/)), f-strings ficaram mais flexíveis: é possível reusar o mesmo tipo de aspas dentro das chaves e escrever expressões em várias linhas, algo que antes dava erro de sintaxe.

## Referências

- [O tutorial do Python - Introdução informal (pt-BR)](https://docs.python.org/pt-br/3/tutorial/introduction.html)
- [Tipos embutidos - documentação oficial (pt-BR)](https://docs.python.org/pt-br/3/library/stdtypes.html)
- [PEP 498 - Literal String Interpolation](https://peps.python.org/pep-0498/)
- [Real Python - Python's F-String for String Interpolation](https://realpython.com/python-f-strings/)

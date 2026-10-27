# Controle de Fluxo

Um programa que só executa linha por linha, de cima para baixo, sem nunca desviar ou repetir, não resolve muita coisa. As ferramentas de controle de fluxo são o que permite um programa decidir e repetir.

## Condicionais

A estrutura `if`/`elif`/`else` testa condições em sequência e executa o primeiro bloco cuja condição for verdadeira:

```python
nota = 7.5

if nota >= 9:
    print("Excelente")
elif nota >= 7:
    print("Bom")
elif nota >= 5:
    print("Regular")
else:
    print("Insuficiente")
```

O `elif` é opcional e pode se repetir quantas vezes for preciso; o `else` final também é opcional. Condicionais podem ser aninhadas, mas quando a indentação cresce demais (3, 4 níveis), geralmente é sinal de que vale a pena extrair uma função:

```python
if usuario.esta_logado:
    if usuario.eh_admin:
        print("Painel de administração")
    else:
        print("Painel comum")
```

### Valores truthy e falsy

Toda expressão em um `if` é avaliada como verdadeira ou falsa, mesmo quando não é um `bool` literal. Os seguintes valores são **falsy** (se comportam como `False`):

```python
0, 0.0, "", [], {}, set(), None, False
```

Qualquer outro valor é **truthy**. Isso permite escrever condições mais diretas:

```python
nomes = []

# em vez de:
if len(nomes) == 0:
    print("lista vazia")

# o idiomático em Python:
if not nomes:
    print("lista vazia")
```

### Expressão condicional (ternário)

Para escolher entre dois valores em uma linha, sem precisar de um `if` completo:

```python
idade = 20
status = "adulto" if idade >= 18 else "menor"
```

Use só quando a condição for simples - encadear vários ternários numa linha só deixa o código difícil de ler.

### `match`/`case`

Desde o Python 3.10, existe o _structural pattern matching_, uma forma de comparar um valor contra vários padrões sem empilhar `if`/`elif`:

```python
def descreve_status(codigo: int) -> str:
    match codigo:
        case 200:
            return "OK"
        case 404:
            return "Não encontrado"
        case 401 | 403:
            return "Acesso negado"
        case _ if codigo >= 500:
            return "Erro no servidor"
        case _:
            return "Status desconhecido"
```

O `_` funciona como "qualquer outro caso" (equivalente ao `else`). `match` também sabe desestruturar tuplas e objetos, o que vai além do que um `switch` de outras linguagens faz:

```python
ponto = (0, 5)

match ponto:
    case (0, 0):
        print("origem")
    case (0, y):
        print(f"no eixo y, em {y}")
    case (x, 0):
        print(f"no eixo x, em {x}")
    case (x, y):
        print(f"em ({x}, {y})")
```

Para a maioria dos casos simples, uma cadeia de `if`/`elif` continua perfeitamente válida - `match` brilha quando você precisa comparar a _forma_ do dado, não só valores soltos.

## Laços com `for`

O `for` em Python itera sobre qualquer sequência (lista, string, range, etc.), sem precisar controlar um índice manualmente:

```python
frutas = ["maçã", "banana", "uva"]

for fruta in frutas:
    print(fruta)
```

Quando você precisa de uma contagem de 0 até N, `range()` gera essa sequência sob demanda, sem criar uma lista na memória:

```python
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2): # 2, 4, 6, 8 (start, stop, step)
    print(i)
```

Se, além do valor, você também precisa do índice, `enumerate()` evita ter que criar e incrementar um contador à mão:

```python
for indice, fruta in enumerate(frutas):
    print(f"{indice}: {fruta}")
# 0: maçã
# 1: banana
# 2: uva
```

E para percorrer duas (ou mais) sequências ao mesmo tempo, posição a posição, `zip()` combina os itens em pares:

```python
nomes = ["Ana", "Bruno"]
idades = [25, 30]

for nome, idade in zip(nomes, idades):
    print(f"{nome} tem {idade} anos")
```

## Laços com `while`

Use `while` quando não se sabe de antemão quantas repetições serão necessárias - a condição é testada antes de cada repetição:

```python
tentativas = 0
senha_correta = False

while not senha_correta and tentativas < 3:
    senha = input("Digite a senha: ")
    senha_correta = senha == "1234"
    tentativas += 1
```

Um `while True` cria um laço infinito proposital, controlado por um `break` dentro do bloco - padrão comum em menus de terminal e servidores que ficam escutando eventos:

```python
while True:
    comando = input("> ")
    if comando == "sair":
        break
    print(f"Executando: {comando}")
```

## Controle de laço

- `break` interrompe o laço imediatamente, mesmo que a condição continue verdadeira
- `continue` pula o resto da iteração atual e vai direto para a próxima
- `pass` não faz nada - serve como placeholder onde a sintaxe exige um bloco, mas ainda não há código para escrever

```python
for numero in range(10):
    if numero == 5:
        break          # para o laço no 5
    if numero % 2 == 0:
        continue        # pula os pares
    print(numero)       # imprime 1, 3
```

### Cláusula `else` em laços

Um detalhe pouco conhecido mesmo entre quem já programa em Python: `for` e `while` aceitam um `else`, que executa quando o laço termina **sem** ter sido interrompido por `break`. É útil para padrões de busca:

```python
numeros = [1, 3, 5, 7, 9]

for n in numeros:
    if n % 2 == 0:
        print("Achei um par")
        break
else:
    print("Nenhum número par na lista")
```

Se o laço rodar até o fim sem encontrar nada, o `else` dispara - pense nele como "se o laço não encontrou motivo para sair antes do tempo".

## Referências

- [O tutorial do Python - Mais ferramentas de controle de fluxo (pt-BR)](https://docs.python.org/pt-br/3/tutorial/controlflow.html)
- [PEP 634 - Structural Pattern Matching: Specification](https://peps.python.org/pep-0634/)
- [Real Python - Python's Match Statement](https://realpython.com/python-match-statement/)

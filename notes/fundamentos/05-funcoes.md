# Funções

Repetir o mesmo bloco de código em vários lugares do programa é um convite a bugs: no dia em que precisar corrigir algo, você vai ter que lembrar de mudar em todos os lugares. Funções resolvem isso empacotando um comportamento com um nome, para ser chamado quantas vezes for preciso.

## Definindo e chamando funções

Uma função é definida com `def`, pode receber parâmetros e devolver um resultado com `return`:

```python
def saudacao(nome: str) -> str:
    return f"Olá, {nome}!"

mensagem = saudacao("Ana")
print(mensagem)  # Olá, Ana!
```

O código dentro da função só executa quando ela é **chamada** (com `()`), não quando é definida. Se não houver `return`, a função devolve `None` implicitamente:

```python
def imprime_log(texto: str) -> None:
    print(f"[LOG] {texto}")

resultado = imprime_log("iniciando")
print(resultado)  # None
```

Uma docstring (string logo abaixo do `def`, entre `"""`) documenta o que a função faz, e ferramentas como `help()` e editores de código leem esse texto:

```python
def area_circulo(raio: float) -> float:
    """Calcula a área de um círculo a partir do raio."""
    return 3.14159 * raio ** 2
```

## Parâmetros

### Posicionais e nomeados

Por padrão, os argumentos são associados aos parâmetros pela ordem (posicionais). Mas também é possível nomear o argumento na chamada, o que deixa claro o que cada valor representa:

```python
def apresenta(nome: str, idade: int) -> str:
    return f"{nome}, {idade} anos"

apresenta("Ana", 25)              # posicional
apresenta(nome="Ana", idade=25)   # nomeado
apresenta(idade=25, nome="Ana")   # nomeado, ordem não importa
```

### Valores padrão

Um parâmetro pode ter um valor padrão, usado quando a chamada não informa aquele argumento:

```python
def saudacao(nome: str, saudacao_inicial: str = "Olá") -> str:
    return f"{saudacao_inicial}, {nome}!"

saudacao("Ana")              # Olá, Ana!
saudacao("Ana", "Boa tarde") # Boa tarde, Ana!
```

Uma armadilha clássica: **nunca** use uma lista, dicionário ou outro objeto mutável como valor padrão. Esse valor é criado **uma única vez**, quando a função é definida, e compartilhado entre todas as chamadas que não informam esse argumento:

```python
# ERRADO: o valor padrão "vaza" entre chamadas diferentes
def adiciona_item(item, lista=[]):
    lista.append(item)
    return lista

adiciona_item("a")  # ['a']
adiciona_item("b")  # ['a', 'b']  -> a lista anterior não foi reiniciada!

# CORRETO: use None como padrão e crie a lista dentro da função
def adiciona_item(item, lista=None):
    if lista is None:
        lista = []
    lista.append(item)
    return lista
```

### `*args` e `**kwargs`

Quando o número de argumentos não é fixo, `*args` recolhe os argumentos posicionais extras em uma tupla, e `**kwargs` recolhe os argumentos nomeados extras em um dicionário:

```python
def soma(*numeros: float) -> float:
    return sum(numeros)

soma(1, 2, 3)        # 6
soma(1, 2, 3, 4, 5)   # 15

def cria_perfil(**dados):
    for chave, valor in dados.items():
        print(f"{chave}: {valor}")

cria_perfil(nome="Ana", idade=25, cidade="Fortaleza")
```

Os nomes `args` e `kwargs` são só convenção - o que importa são os operadores `*` e `**`. Quando parâmetros são combinados, a ordem obrigatória é: posicionais, depois `*args`, depois nomeados com padrão, depois `**kwargs`:

```python
def funcao(a, b, *args, c=10, **kwargs):
    print(a, b, args, c, kwargs)

funcao(1, 2, 3, 4, c=20, d=30)
# 1 2 (3, 4) 20 {'d': 30}
```

## Funções como objetos

Em Python, uma função é um valor como qualquer outro - pode ser guardada em uma variável, passada como argumento ou retornada por outra função.

### `lambda`

Uma `lambda` é uma função anônima, de uma única expressão, útil quando você precisa de uma função pequena e descartável, geralmente como argumento de outra função:

```python
dobro = lambda x: x * 2
dobro(5)  # 10

# equivalente a:
def dobro(x):
    return x * 2
```

`lambda` não aceita várias linhas nem `return` explícito - se a lógica for mais complexa que uma expressão simples, use `def` normalmente.

### Funções de alta ordem

Funções que recebem outra função como argumento são chamadas de alta ordem. `sorted()`, `map()` e `filter()` são os exemplos mais comuns:

```python
pessoas = [("Ana", 25), ("Bruno", 19), ("Carla", 30)]

# ordena pela idade (segundo item da tupla)
por_idade = sorted(pessoas, key=lambda p: p[1])

# aplica uma função a cada item
dobros = list(map(lambda x: x * 2, [1, 2, 3]))  # [2, 4, 6]

# filtra itens que satisfazem uma condição
pares = list(filter(lambda x: x % 2 == 0, range(10)))  # [0, 2, 4, 6, 8]
```

## Escopo

Escopo é a região do código onde um nome (variável, função) pode ser acessado.

```python
x = 10  # escopo global

def minha_funcao():
    y = 20  # escopo local, só existe dentro da função
    print(x)  # pode ler variável global
    print(y)

minha_funcao()
print(x)  # 10, funciona
print(y)  # NameError: y só existe dentro de minha_funcao
```

Por padrão, uma função só **lê** variáveis do escopo externo - para **reatribuir** uma variável global dentro de uma função, é preciso declarar `global` explicitamente:

```python
contador = 0

def incrementa():
    global contador
    contador += 1

incrementa()
print(contador)  # 1
```

`nonlocal` faz o mesmo papel, mas para o escopo de uma função que envolve outra (closures, a seguir), não o escopo global.

### Closures

Uma closure acontece quando uma função interna "lembra" de variáveis do escopo da função externa, mesmo depois que a função externa já terminou de executar:

```python
def cria_contador():
    contagem = 0

    def incrementar():
        nonlocal contagem
        contagem += 1
        return contagem

    return incrementar

contador = cria_contador()
contador()  # 1
contador()  # 2
contador()  # 3
```

Cada chamada a `cria_contador()` gera uma closure independente, com sua própria cópia de `contagem` - é assim que decorators (assunto para quando você chegar na seção [Python](/labs/python/python/06-decorators/)) conseguem "lembrar" de configurações entre chamadas.

## Retorno de múltiplos valores

Uma função Python só retorna um valor, mas esse valor pode ser uma tupla - o que, na prática, dá a sensação de retornar vários valores de uma vez:

```python
def divide_com_resto(a: int, b: int) -> tuple[int, int]:
    return a // b, a % b

quociente, resto = divide_com_resto(17, 5)
print(quociente, resto)  # 3 2
```

O desempacotamento (`quociente, resto = ...`) funciona porque a tupla retornada tem exatamente dois itens, e eles são atribuídos às duas variáveis na mesma ordem.

## Referências

- [O tutorial do Python - Definindo funções (pt-BR)](https://docs.python.org/pt-br/3/tutorial/controlflow.html#defining-functions)
- [Real Python - Python args and kwargs](https://realpython.com/python-kwargs-and-args/)
- [Real Python - Python Closures](https://realpython.com/python-closures/)

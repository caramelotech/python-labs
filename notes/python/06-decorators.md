# Decorators

Você já usou decorators sem talvez ter parado para pensar neles como tal: `@property`, `@classmethod` e `@staticmethod`, vistos em [Orientação a Objetos](/labs/python/python/02-orientacao-a-objetos/), são todos decorators embutidos. Esta nota mostra como eles funcionam por dentro - e como criar os seus.

## Closures

Um decorator depende de um conceito já visto em [Funções](/labs/python/fundamentos/05-funcoes/): closures, funções que "lembram" variáveis do escopo em que foram criadas, mesmo depois que esse escopo já terminou de executar.

```python
def cria_saudacao(saudacao: str):
    def saudar(nome: str) -> str:
        return f"{saudacao}, {nome}!"
    return saudar

ola = cria_saudacao("Olá")
ola("Ana")  # Olá, Ana!
```

`saudar` continua tendo acesso a `saudacao` mesmo depois que `cria_saudacao` já retornou - é exatamente esse mecanismo que um decorator explora.

## Decorators próprios

Um decorator é uma função que recebe outra função, e devolve uma terceira função que "envolve" o comportamento da primeira:

```python
import time

def cronometrar(funcao):
    def wrapper(*args, **kwargs):
        inicio = time.perf_counter()
        resultado = funcao(*args, **kwargs)
        duracao = time.perf_counter() - inicio
        print(f"{funcao.__name__} levou {duracao:.4f}s")
        return resultado
    return wrapper

@cronometrar
def soma_lenta(a: int, b: int) -> int:
    time.sleep(1)
    return a + b

soma_lenta(2, 3)
# soma_lenta levou 1.0003s
# 5
```

A sintaxe `@cronometrar` acima da função é só um atalho - é exatamente o mesmo que escrever `soma_lenta = cronometrar(soma_lenta)` depois de definir a função. `*args` e `**kwargs` no `wrapper` garantem que o decorator funcione com qualquer assinatura de função, não só uma específica.

Quando o decorator precisa receber seus próprios parâmetros (não os da função decorada), é preciso uma camada extra de função:

```python
def repetir(vezes: int):
    def decorator(funcao):
        def wrapper(*args, **kwargs):
            for _ in range(vezes):
                resultado = funcao(*args, **kwargs)
            return resultado
        return wrapper
    return decorator

@repetir(vezes=3)
def avisa():
    print("Executando!")

avisa()
# Executando!
# Executando!
# Executando!
```

Um problema sutil do `wrapper`: ele substitui a função original, inclusive seus metadados (`__name__`, `__doc__`). Isso prejudica depuração e ferramentas que leem esses metadados. `functools.wraps` corrige isso, copiando os metadados da função original para o `wrapper`:

```python
from functools import wraps

def cronometrar(funcao):
    @wraps(funcao)
    def wrapper(*args, **kwargs):
        ...
        return funcao(*args, **kwargs)
    return wrapper
```

Sem `@wraps`, `soma_lenta.__name__` passaria a ser `"wrapper"`; com ele, continua `"soma_lenta"`.

## Decorators embutidos

`@property`, `@classmethod` e `@staticmethod` (já vistos em [Orientação a Objetos](/labs/python/python/02-orientacao-a-objetos/)) são decorators prontos da linguagem, aplicados a métodos de classe. A diferença entre eles e os decorators de função comuns é só o contexto em que são usados - o mecanismo por trás ("função que recebe e transforma outra função") é o mesmo.

## Módulo `functools`

Além de `wraps`, o módulo `functools` traz outras ferramentas que resolvem problemas recorrentes sem reimplementar nada:

```python
from functools import reduce, lru_cache, singledispatch

# reduce: acumula um valor aplicando uma função repetidamente
fatorial_5 = reduce(lambda acumulado, n: acumulado * n, range(1, 6))  # 120

# lru_cache: guarda em cache o resultado de chamadas anteriores com os mesmos argumentos
@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# sem cache, essa recursão repetiria o mesmo cálculo exponencialmente;
# com lru_cache, cada valor de n só é calculado uma vez

# singledispatch: escolhe qual versão de uma função rodar, com base no tipo do primeiro argumento
@singledispatch
def processa(valor):
    print(f"Valor genérico: {valor}")

@processa.register
def _(valor: int):
    print(f"É um inteiro: {valor}")

@processa.register
def _(valor: str):
    print(f"É um texto: {valor}")

processa(10)       # É um inteiro: 10
processa("oi")      # É um texto: oi
processa(3.14)      # Valor genérico: 3.14
```

`singledispatch` é a forma pythônica de simular "sobrecarga de função" (ter várias versões da mesma função, uma por tipo de argumento) - algo que Python não suporta nativamente como Java ou C++, mas que esse decorator resolve de forma elegante.

## Referências

- [Documentação oficial - módulo functools](https://docs.python.org/3/library/functools.html)
- [Real Python - Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)
- [Real Python - Python Closures](https://realpython.com/python-closures/)

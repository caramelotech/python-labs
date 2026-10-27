# Exceções

Nem todo erro em um programa é um bug no sentido de "código errado" - um arquivo pode não existir, uma API pode estar fora do ar, o usuário pode digitar texto onde se esperava um número. Exceções são o mecanismo do Python para lidar com esses erros de forma controlada, sem derrubar o programa inteiro.

## O que é uma exceção

Um erro de sintaxe (`SyntaxError`) impede o programa de nem começar a rodar - é um problema no código em si. Uma exceção acontece **durante** a execução, quando uma operação válida sintaticamente não pode ser concluída:

```python
>>> 10 / 0
Traceback (most recent call last):
ZeroDivisionError: division by zero
```

Sem tratamento, uma exceção interrompe o programa e imprime o _traceback_ (o caminho que levou ao erro). Tratar a exceção é decidir o que fazer quando isso acontece, em vez de deixar o programa simplesmente parar.

## try/except

```python
try:
    numero = int(input("Digite um número: "))
    resultado = 10 / numero
except ValueError:
    print("Isso não é um número válido")
except ZeroDivisionError:
    print("Não é possível dividir por zero")
else:
    print(f"Resultado: {resultado}")
finally:
    print("Fim da tentativa")
```

- o bloco `try` contém o código que pode falhar
- cada `except` captura um tipo específico de exceção - tenha quantos precisar, do mais específico para o mais genérico
- `else` executa só se **nenhuma** exceção ocorreu no `try`
- `finally` executa sempre, com ou sem exceção - ideal para liberar recursos (fechar arquivo, conexão de rede)

Capture exceções específicas, nunca um `except:` genérico (ou `except Exception:` sem necessidade real) - isso esconde bugs reais que não têm nada a ver com o que você esperava tratar:

```python
# evite:
try:
    processar_dados()
except:
    print("Algo deu errado")  # qual erro? ninguém sabe

# prefira:
try:
    processar_dados()
except (ValueError, KeyError) as erro:
    print(f"Dados inválidos: {erro}")
```

## Exceções comuns da biblioteca padrão

| Exceção             | Quando acontece                                               |
| ------------------- | ------------------------------------------------------------- |
| `ValueError`        | valor do tipo certo, mas com conteúdo inválido (`int("abc")`) |
| `TypeError`         | operação aplicada a um tipo incompatível (`1 + "1"`)          |
| `ZeroDivisionError` | divisão ou módulo por zero                                    |
| `KeyError`          | chave inexistente em um dicionário                            |
| `IndexError`        | índice fora do alcance em uma lista/tupla                     |
| `FileNotFoundError` | tentativa de abrir um arquivo que não existe                  |

## Lançando exceções

Use `raise` para disparar uma exceção manualmente, quando seu próprio código detecta uma condição inválida:

```python
def definir_idade(idade: int) -> None:
    if idade < 0:
        raise ValueError("Idade não pode ser negativa")
```

Dentro de um bloco `except`, `raise ... from ...` encadeia a exceção original à nova, preservando o contexto de causa - útil quando você "traduz" um erro de baixo nível para um erro mais significativo no seu domínio:

```python
try:
    conectar_banco()
except ConnectionError as erro_original:
    raise RuntimeError("Falha ao iniciar a aplicação") from erro_original
```

O traceback resultante mostra as duas exceções, deixando claro que a segunda foi causada pela primeira - em vez de simplesmente substituir uma pela outra.

## Exceções customizadas

Criar uma exceção própria, herdando de `Exception` (ou de uma exceção mais específica), ajuda a diferenciar erros do seu domínio de erros genéricos da linguagem:

```python
class SaldoInsuficienteError(Exception):
    """Lançada quando uma conta não tem saldo suficiente para uma operação."""
    def __init__(self, saldo: float, valor_solicitado: float):
        self.saldo = saldo
        self.valor_solicitado = valor_solicitado
        super().__init__(f"Saldo de {saldo} insuficiente para sacar {valor_solicitado}")

def sacar(saldo: float, valor: float) -> float:
    if valor > saldo:
        raise SaldoInsuficienteError(saldo, valor)
    return saldo - valor
```

Em projetos maiores, é comum criar uma hierarquia de exceções próprias (uma exceção base do projeto, com subclasses específicas), permitindo capturar tanto um erro específico quanto toda a categoria de uma vez.

## Gerenciadores de contexto

O `with` garante que um recurso (arquivo, conexão, lock) seja liberado corretamente, mesmo que uma exceção aconteça no meio do bloco:

```python
with open("dados.txt") as arquivo:
    conteudo = arquivo.read()
# o arquivo é fechado automaticamente aqui, mesmo se read() falhar
```

Por trás disso estão dois métodos especiais: `__enter__` (executado ao entrar no bloco `with`) e `__exit__` (executado ao saí­-lo, sempre, até em caso de exceção). Criar seu próprio gerenciador de contexto é só implementar esses dois métodos:

```python
class Cronometro:
    def __enter__(self):
        import time
        self.inicio = time.perf_counter()
        return self

    def __exit__(self, tipo_exc, valor_exc, traceback):
        import time
        duracao = time.perf_counter() - self.inicio
        print(f"Levou {duracao:.4f}s")
        return False  # não suprime a exceção, se houver uma

with Cronometro():
    soma = sum(range(1_000_000))
```

## Exception groups (Python 3.11+)

Quando mais de uma exceção pode acontecer "ao mesmo tempo" (por exemplo, em tarefas concorrentes rodando em paralelo), um `except` tradicional só captura uma de cada vez. Desde o Python 3.11 ([PEP 654](https://peps.python.org/pep-0654/)), `ExceptionGroup` agrupa várias exceções em uma só, e a sintaxe `except*` captura por tipo dentro do grupo:

```python
try:
    raise ExceptionGroup(
        "falhas ao processar pedidos",
        [ValueError("pedido 1 inválido"), KeyError("pedido 2 sem campo obrigatório")],
    )
except* ValueError as erros_valor:
    print(f"Erros de valor: {erros_valor.exceptions}")
except* KeyError as erros_chave:
    print(f"Erros de chave: {erros_chave.exceptions}")
```

É um recurso recente e mais comum em código que usa `asyncio` com múltiplas tarefas simultâneas (veja [Concorrência](/labs/python/python/07-concorrencia/)) do que em scripts simples do dia a dia.

## Referências

- [O tutorial do Python - Erros e exceções (pt-BR)](https://docs.python.org/pt-br/3/tutorial/errors.html)
- [PEP 654 - Exception Groups and except\*](https://peps.python.org/pep-0654/)
- [Real Python - Python 3.11 Exception Groups](https://realpython.com/python311-exception-groups/)

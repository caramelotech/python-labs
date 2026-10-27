# Orientação a Objetos

Até aqui, dados e as funções que operam sobre eles ficaram separados: uma lista de pedidos de um lado, funções que calculam total e desconto do outro. Orientação a objetos agrupa os dois: um objeto guarda seus próprios dados (atributos) e os comportamentos que fazem sentido para eles (métodos).

## Classes e instâncias

Uma classe é o molde; uma instância é um objeto concreto criado a partir desse molde:

```python
class Cachorro:
    def __init__(self, nome: str, idade: int):
        self.nome = nome
        self.idade = idade

    def latir(self) -> str:
        return f"{self.nome}: au au!"

paçoca = Cachorro("Paçoca", 3)
biscoito = Cachorro("Biscoito", 1)

paçoca.latir()      # Paçoca: au au!
paçoca.nome          # Paçoca
```

`__init__` é o construtor - executa automaticamente quando você cria uma instância (`Cachorro(...)`), e é onde normalmente se definem os atributos iniciais. `self` representa a própria instância, e é o primeiro parâmetro de todo método - o Python passa ele automaticamente, você nunca o informa na chamada.

Atributos de **instância** (`self.nome`) pertencem a cada objeto individualmente. Atributos de **classe** são compartilhados por todas as instâncias:

```python
class Cachorro:
    especie = "Canis familiaris"  # atributo de classe, compartilhado

    def __init__(self, nome: str):
        self.nome = nome          # atributo de instância, individual

paçoca = Cachorro("Paçoca")
biscoito = Cachorro("Biscoito")

paçoca.especie    # 'Canis familiaris'
biscoito.especie  # 'Canis familiaris' (mesmo valor, mesmo objeto na memória)
```

## Herança

Herança permite que uma classe reaproveite (e estenda) o comportamento de outra:

```python
class Animal:
    def __init__(self, nome: str):
        self.nome = nome

    def emitir_som(self) -> str:
        return "..."

class Cachorro(Animal):
    def emitir_som(self) -> str:
        return f"{self.nome}: au au!"

class Gato(Animal):
    def emitir_som(self) -> str:
        return f"{self.nome}: miau!"
```

`super()` dá acesso aos métodos da classe-mãe, útil quando a classe filha precisa complementar (e não substituir completamente) o comportamento herdado:

```python
class Funcionario:
    def __init__(self, nome: str, salario: float):
        self.nome = nome
        self.salario = salario

class Gerente(Funcionario):
    def __init__(self, nome: str, salario: float, bonus: float):
        super().__init__(nome, salario)  # reaproveita o __init__ da classe-mãe
        self.bonus = bonus
```

Quando uma classe herda de mais de uma classe ao mesmo tempo (herança múltipla), o Python precisa decidir em que ordem procurar um método - essa ordem é o **MRO** (Method Resolution Order), calculado pelo algoritmo de linearização C3. Na prática, o MRO garante que cada classe na hierarquia seja consultada só uma vez, mesmo em hierarquias complexas em formato de diamante:

```python
class A:
    def saudacao(self):
        return "A"

class B(A):
    pass

class C(A):
    def saudacao(self):
        return "C"

class D(B, C):
    pass

D().saudacao()     # 'C' - o MRO consulta D, B, C, A, nessa ordem
D.__mro__           # mostra a ordem completa de resolução
```

## Polimorfismo e encapsulamento

**Polimorfismo** é a capacidade de objetos de classes diferentes responderem ao mesmo método, cada um com seu próprio comportamento - como os dois `emitir_som()` do exemplo de herança acima. Python não exige que as classes compartilhem uma classe-mãe para isso funcionar (é o chamado _duck typing_): se o objeto tem o método esperado, ele serve, independente da sua classe.

Sobre encapsulamento, Python não tem modificadores de acesso reais (como `private`/`protected` em Java) - tudo é público por convenção, com duas marcações informais:

```python
class ContaBancaria:
    def __init__(self, saldo_inicial: float):
        self._saldo = saldo_inicial      # "protegido": um underscore, convenção de "uso interno"
        self.__senha = "1234"             # "privado": dois underscores, ativa name mangling
```

O prefixo de um underscore (`_saldo`) é só uma convenção lida por quem programa - o Python não impede o acesso de fora. Já o prefixo de dois underscores (`__senha`) ativa o **name mangling**: o Python renomeia o atributo internamente para `_ContaBancaria__senha`, o que dificulta (mas não impede) o acesso externo acidental. Isso não é um controle de acesso de verdade, é uma camada de proteção contra colisão de nomes em hierarquias de herança.

Para expor um atributo controlado (validando o valor na escrita, por exemplo), `@property` é o jeito pythônico - por fora, continua parecendo um atributo comum, mas por dentro passa por um método:

```python
class ContaBancaria:
    def __init__(self, saldo_inicial: float):
        self._saldo = saldo_inicial

    @property
    def saldo(self) -> float:
        return self._saldo

    @saldo.setter
    def saldo(self, valor: float) -> None:
        if valor < 0:
            raise ValueError("Saldo não pode ser negativo")
        self._saldo = valor

conta = ContaBancaria(100)
conta.saldo = 50      # chama o setter, valida o valor
conta.saldo            # chama o getter, retorna 50
conta.saldo = -10      # ValueError
```

## Métodos especiais (dunder methods)

Métodos com nome entre dois underscores (`__algo__`) são "métodos mágicos" - o Python os chama automaticamente em situações específicas, como imprimir um objeto ou somar dois objetos:

```python
class Ponto:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    def __repr__(self) -> str:
        return f"Ponto({self.x}, {self.y})"

    def __eq__(self, outro) -> bool:
        return self.x == outro.x and self.y == outro.y

    def __add__(self, outro):
        return Ponto(self.x + outro.x, self.y + outro.y)

p1 = Ponto(1, 2)
p2 = Ponto(3, 4)
print(p1)        # Ponto(1, 2) - usa __repr__
p1 == Ponto(1, 2) # True - usa __eq__
p1 + p2           # Ponto(4, 6) - usa __add__
```

`__repr__` deve devolver uma representação que ajude a depurar (idealmente, um código que recria o objeto); `__str__`, quando definido, é o que `print()` usa - se só `__repr__` existir, `print()` cai de volta para ele. `__new__` é chamado antes de `__init__` e é responsável por efetivamente criar a instância; raramente precisa ser sobrescrito, exceto em casos avançados como singletons ou classes imutáveis.

## `@classmethod` e `@staticmethod`

Um método normal recebe `self` (a instância). Um `@classmethod` recebe `cls` (a própria classe) e é comum para construtores alternativos. Um `@staticmethod` não recebe nem um nem outro - é só uma função agrupada dentro da classe por organização:

```python
class Data:
    def __init__(self, dia: int, mes: int, ano: int):
        self.dia = dia
        self.mes = mes
        self.ano = ano

    @classmethod
    def de_string(cls, texto: str) -> "Data":
        dia, mes, ano = (int(parte) for parte in texto.split("/"))
        return cls(dia, mes, ano)

    @staticmethod
    def eh_ano_bissexto(ano: int) -> bool:
        return ano % 4 == 0 and (ano % 100 != 0 or ano % 400 == 0)

data = Data.de_string("25/12/2026")
Data.eh_ano_bissexto(2028)  # True
```

## Abstração

O módulo `abc` permite definir uma classe que **não pode ser instanciada diretamente**, servindo como contrato para as classes filhas - garantindo que todas implementem certos métodos:

```python
from abc import ABC, abstractmethod

class FormaGeometrica(ABC):
    @abstractmethod
    def area(self) -> float:
        ...

class Quadrado(FormaGeometrica):
    def __init__(self, lado: float):
        self.lado = lado

    def area(self) -> float:
        return self.lado ** 2

FormaGeometrica()   # TypeError: Can't instantiate abstract class
Quadrado(4).area()   # 16, funciona porque implementou area()
```

Se uma classe filha não implementar todos os métodos marcados com `@abstractmethod`, o Python também impede a instanciação dela - é uma forma de garantir, em tempo de execução, que o "contrato" da classe base seja cumprido.

## Dataclasses

Para classes que só guardam dados (sem muita lógica própria), escrever `__init__`, `__repr__` e `__eq__` à mão é repetitivo. O decorator `@dataclass` gera esse código automaticamente a partir das anotações de tipo:

```python
from dataclasses import dataclass

@dataclass
class Produto:
    nome: str
    preco: float
    estoque: int = 0

p = Produto("Caneca", 25.90)
print(p)              # Produto(nome='Caneca', preco=25.9, estoque=0)
p == Produto("Caneca", 25.90)  # True, __eq__ gerado automaticamente
```

## Referências

- [O tutorial do Python - Classes (pt-BR)](https://docs.python.org/pt-br/3/tutorial/classes.html)
- [Documentação oficial - módulo abc](https://docs.python.org/3/library/abc.html)
- [Real Python - Object-Oriented Programming in Python 3](https://realpython.com/python3-object-oriented-programming/)
- [Real Python - Inheritance and Composition: A Python OOP Guide](https://realpython.com/inheritance-composition-python/)

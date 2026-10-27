# Biblioteca Padrão

Uma das frases mais repetidas sobre Python é que ele vem com "baterias incluídas" - a biblioteca padrão resolve boa parte das tarefas comuns de um programa sem precisar instalar nada extra. Esta nota é um tour pelos módulos que você vai encontrar com mais frequência.

## Sistema de arquivos

`pathlib` representa caminhos de arquivo como objetos, com uma API mais legível que manipular strings de caminho na mão:

```python
from pathlib import Path

caminho = Path("dados") / "relatorios" / "2026.csv"
caminho.exists()         # checa se existe
caminho.parent            # Path('dados/relatorios')
caminho.suffix             # '.csv'
caminho.read_text()        # lê o conteúdo como texto

for arquivo in Path("dados").glob("*.csv"):
    print(arquivo)
```

`os` complementa com operações de sistema mais gerais - diretório atual, variáveis de ambiente, informações do processo:

```python
import os

os.getcwd()                   # diretório de trabalho atual
os.environ.get("HOME")        # variável de ambiente
os.listdir(".")                # lista arquivos do diretório
```

Para manipular caminhos, prefira `pathlib` em código novo - é mais legível e funciona igual em Windows, Linux e macOS. `os` continua essencial para o resto (processos, variáveis de ambiente, permissões).

## Dados estruturados

`json` converte entre texto JSON e estruturas Python (dict, list) - essencial para consumir e expor APIs:

```python
import json

dados = {"nome": "Ana", "idade": 25}
texto_json = json.dumps(dados)            # '{"nome": "Ana", "idade": 25}'
de_volta = json.loads(texto_json)         # {'nome': 'Ana', 'idade': 25}

with open("usuario.json", "w") as arquivo:
    json.dump(dados, arquivo)
```

`csv` lida com arquivos de planilha simples, linha por linha, sem precisar instalar o pandas para uma tarefa básica de leitura/escrita:

```python
import csv

with open("usuarios.csv", newline="") as arquivo:
    leitor = csv.DictReader(arquivo)  # cada linha vira um dict, usando o cabeçalho como chaves
    for linha in leitor:
        print(linha["nome"])

with open("saida.csv", "w", newline="") as arquivo:
    escritor = csv.DictWriter(arquivo, fieldnames=["nome", "idade"])
    escritor.writeheader()
    escritor.writerow({"nome": "Ana", "idade": 25})
```

## Datas e horários

`datetime` representa datas, horas e intervalos de tempo:

```python
from datetime import datetime, timedelta

agora = datetime.now()
amanha = agora + timedelta(days=1)

agora.strftime("%d/%m/%Y %H:%M")   # formata para exibição, ex: '27/10/2026 14:30'
```

Para trabalhar com fusos horários de forma correta (um problema notoriamente cheio de pegadinhas), `zoneinfo`, parte da biblioteca padrão desde o Python 3.9, usa o banco de dados IANA de fusos horários:

```python
from datetime import datetime
from zoneinfo import ZoneInfo

horario_fortaleza = datetime(2026, 10, 27, 15, 0, tzinfo=ZoneInfo("America/Fortaleza"))
horario_utc = horario_fortaleza.astimezone(ZoneInfo("UTC"))
```

## Texto e padrões

`re` implementa expressões regulares - para buscar, validar ou extrair padrões em texto que vão além do que `str.split()`/`str.replace()` resolvem:

```python
import re

texto = "Contato: ana@email.com ou (85) 99999-0000"

email = re.search(r"[\w.]+@[\w.]+", texto)
if email:
    print(email.group())   # ana@email.com

todos_numeros = re.findall(r"\d+", texto)  # ['85', '99999', '0000']
```

Regex resolve bem casos de busca/validação de padrão (e-mail, telefone, CPF), mas tende a ficar ilegível para parsing de formatos estruturados (HTML, JSON) - para esses casos, prefira um parser dedicado (como o próprio módulo `json`).

## Utilitários de aplicação

`logging` é a forma correta de registrar o que um programa está fazendo - muito mais flexível que `print()`, porque permite níveis de severidade e múltiplos destinos (arquivo, terminal, serviço externo) configurados de uma vez:

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.info("Aplicação iniciada")
logger.warning("Configuração X não encontrada, usando padrão")
logger.error("Falha ao conectar ao banco")
```

`argparse` monta interfaces de linha de comando, lendo e validando os argumentos passados ao script:

```python
import argparse

parser = argparse.ArgumentParser(description="Processa um arquivo")
parser.add_argument("arquivo", help="caminho do arquivo a processar")
parser.add_argument("--verbose", action="store_true", help="mostra mais detalhes")

args = parser.parse_args()
print(args.arquivo, args.verbose)
```

```bash
python script.py dados.csv --verbose
```

`sqlite3` dá acesso a um banco de dados SQL completo, guardado em um único arquivo, sem precisar instalar nem configurar um servidor de banco separado - ótimo para prototipar ou para aplicações pequenas que não precisam de um Postgres/MySQL dedicado:

```python
import sqlite3

conexao = sqlite3.connect("app.db")
cursor = conexao.cursor()

cursor.execute("CREATE TABLE IF NOT EXISTS usuarios (id INTEGER PRIMARY KEY, nome TEXT)")
cursor.execute("INSERT INTO usuarios (nome) VALUES (?)", ("Ana",))
conexao.commit()

cursor.execute("SELECT * FROM usuarios")
print(cursor.fetchall())
```

## Referências

- [Documentação oficial - módulo pathlib](https://docs.python.org/pt-br/3/library/pathlib.html)
- [Documentação oficial - módulo json](https://docs.python.org/pt-br/3/library/json.html)
- [Documentação oficial - módulo datetime](https://docs.python.org/pt-br/3/library/datetime.html)
- [Real Python - Logging in Python](https://realpython.com/python-logging/)

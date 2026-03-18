# 1. Learning uv
Descrever boas práticas de desenvolvimento de AIs e Dados

Obs.: 
a. Todos os comandos aqui se aplicam ao MacOS/Linux
b. Com o **uv**, você não precisa mais baixar o Python no site oficial. Ele faz tudo. Ele é absurdamente rápido e resolve um problema que o `pip` nunca resolveu bem: ele gerencia o próprio Python para você.

# 1.1. O que é o uv?

O `uv` é uma ferramenta "tudo-em-um". Ele substitui o `pip` (instalador), o `venv` (ambientes virtuais), o `pip-tools` (trava de versões) e até o `pyenv` (instalador de versões do Python).

---

# 1.2. Instalação (Só uma vez)

Antes de tudo, você precisa do `uv` no seu sistema.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh

```

*Reinicie o seu terminal após a instalação.*

---

# 1.3. Criando um Projeto do Zero (O jeito moderno)

Esqueça o `python -m venv`. Com o `uv`, seguimos este fluxo:

## Passo 1.3.1: Inicie o projeto

Crie uma pasta e entre nela. Em seguida, digite:

```bash
uv init

```

> **O que aconteceu?** O `uv` criou um arquivo `pyproject.toml` (o cérebro do seu projeto) e um `hello.py`.

## Passo 1.3.2: Criando o ambiente e escolhendo a versão

Quer usar o Python 3.12, mas não tem ele instalado? Sem problemas:

```bash
uv venv --python 3.12

```

O `uv` vai baixar o Python 3.12 isoladamente para esse projeto e criar a pasta `.venv`. **É instantâneo.**

## Passo 1.3.3: Ativando o ambiente

Igual ao método tradicional:

```bash
source .venv/bin/activate
```

# 1.4. Instalando pacotes (A mágica da velocidade)

Em vez de `pip install`, usamos `uv add`.

```bash
uv add pandas requests

```

**Por que isso é melhor que o pip?**

1.4.1. **Velocidade:** Ele instala em milissegundos usando cache compartilhado.
1.4.2. **Lockfile:** Ele cria automaticamente um arquivo `uv.lock`. Esse arquivo garante que qualquer pessoa que baixar seu projeto terá **exatamente** as mesmas versões de bibliotecas que você, evitando o famoso "na minha máquina funciona".

---

# 1.5. Resumo de Comandos "uv" vs "Tradicional"

| Ação | Jeito Antigo (Slow) | Jeito uv (Fast ⚡) |
| --- | --- | --- |
| Criar ambiente | `python -m venv .venv` | `uv venv` |
| Instalar pacote | `pip install pandas` | `uv add pandas` |
| Remover pacote | `pip uninstall pandas` | `uv remove pandas` |
| Rodar um script | `python main.py` | `uv run main.py` |
| Gerar lista de deps | `pip freeze > req.txt` | (Automático no `uv.lock`) |

---

# Dica de Ouro: `uv run`

Você nem precisa ativar o ambiente para rodar algo. Se você digitar:

```bash
uv run python main.py

```

O `uv` detecta o ambiente, garante que as dependências estão instaladas e roda o script. Se o ambiente não existir, ele cria na hora.


# 1.6. Usando uv juntamente com o arquivo requirements.txt


Converter um projeto do `requirements.txt` para o **uv** é como trocar um motor a combustão por um elétrico: o carro fica mais leve, mais rápido e muito mais fácil de dar manutenção.

O segredo aqui é que o `uv` não apenas lê o seu arquivo antigo, ele o organiza dentro de um padrão moderno chamado **PEP 621**, que centraliza tudo no arquivo `pyproject.toml`.

Aqui está o passo a passo para fazer essa migração com segurança:

---

## Passo 1.6.1: Inicialize o `uv` na pasta do projeto

Vá até a pasta onde está o seu arquivo `requirements.txt` e execute:

```bash
uv init

```

> **O que isso faz:** Cria o arquivo `pyproject.toml`, que será o novo "RG" do seu projeto, substituindo a necessidade de gerenciar o `requirements.txt` manualmente no futuro.

---

## Passo 1.6.2: Importe as dependências

Agora, em vez de instalar uma por uma, vamos dizer ao `uv` para ler o seu arquivo antigo e adicionar tudo ao novo formato:

```bash
uv add -r requirements.txt

```

> **A mágica aqui:** O `uv` vai ler cada linha do seu `requirements.txt`, resolver as versões para garantir que não haja conflitos e escrever essas dependências dentro do `pyproject.toml`. Além disso, ele criará o arquivo `uv.lock`.

---

## Passo 1.6.3: Entenda a diferença dos arquivos

Após a conversão, você terá dois arquivos principais que substituem o antigo método:

1. **`pyproject.toml` (O que você edita):** Contém as bibliotecas que você realmente precisa (ex: `pandas`, `flask`). É legível por humanos.
2. **`uv.lock` (O que o computador lê):** É uma "fotografia" exata de todas as sub-dependências e versões instaladas. **Nunca edite este arquivo manualmente**, mas sempre o envie para o GitHub.

---

## Passo 1.6.4: Limpeza (Opcional, mas recomendado)

Agora que suas dependências estão seguras no novo formato, você pode se livrar do passado:

1.6.4.1. **Delete o `requirements.txt**` (ou renomeie para `.old` se estiver inseguro).
1.6.4.2. **Delete a pasta `venv` antiga** (se você tiver uma):

```bash
rm -rf venv
```

1.6.4.3. **Crie o novo ambiente padrão do uv:**
```bash
uv venv
```

---

## Como trabalhar a partir de agora?

Esqueça o `pip`. Sua rotina agora será:

* **Quer adicionar uma biblioteca nova?** `uv add nome_da_lib`
* **Quer rodar seu código?** `uv run python seu_script.py` (O `uv` garante que o ambiente está atualizado antes de rodar).
* **Alguém novo baixou seu projeto?** Eles só precisam digitar `uv sync` e o ambiente será montado identico ao seu em milissegundos.

---

## Dica de Especialista: Dependências de Desenvolvimento

Se você tinha um `requirements-dev.txt` (com coisas como `pytest` ou `black`), o `uv` tem um lugar especial para isso:

```bash
uv add --dev pytest

```

Isso separa o que é necessário para o sistema rodar do que é necessário apenas para você programar.


# 2. Instalando Terraform

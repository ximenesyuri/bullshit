----------------------------------------------------
- title:  você realmente precisa de `venvs`?
- author: Yuri Ximenes `CSYS:TLY`
----------------------------------------------------

# INTRODUÇÃO

- para começar um projeto em Python:
    1. `cd my-project`
    2. `python -m venv .venv`
        - `uv venv`
        - ...
- mas...
    - o que, de fato, é `venv`?
    - pra que serve um `venv`?
    - como construir um `venv`?
        - precisa-se da biblioteca `venv`?
        - precisa-se de `uv` ou outra ferramenta?
    - usar `venv` é _realmente_ necessário?

# OBJETIVO

1. Python:
    - o que é uma linguagem de programação?
    - o que é Python?
    - o que é um interpretador?
    - o que é `python`?
    - como Python se relaciona com `python`?

2. distribuições
    - o que são _módulos_ em Python?
    - o que são _extensões_ em Python?
    - o que são _bibliotecas_ em Python?
    - o que é uma _distribuição_ Python?
    - o que significa ser _builtin_?

3. environment:
    - o que é um _environment_?
    - como construir _environments_?
    - o que é um _prefixed environment_?
    - o que é um _system environment_?
    - o que é um _virtual environment_ (`venv`)?
    - quando (não) usar um _virtual environment_?

====================================================================
1. PYTHON
====================================================================

- Python é a prescrição de uma  _linguagem de programação_
- `python` é uma _implementação_ do sistema de validação de Python em uma máquina.

=====================================================================

## 1.1 PYTHON: PRESCRIÇÃO

- linguagem formal:
    - conjunto de "símbolos":
        - chamados de _characters_ 
        - organizados em _lexical categories_
            - _keywords_
            - _delimiters_
            - _separators_
            - _operators_
            - _identifiers_
            - _literals_
            - ...
        - chamadas de _tokens_
    - conjunto de "regras gramaticais":
        - formam a _formal grammar_
        - determinam a que _lexical category_ um símbolo pertence

- sistema de validação:
    - tem-se "critérios pré-definidos" que:
        - dada uma _string_:
            - isto é, um amontado de _characters_
            - formando um _source code_
        - classificam ela como:
            - _válida_ na linguagem formal
            - _inválida_ na linguagem formal

-----------------------------------------------------------------------------------
__OBS.__ O sistema de validação é tipicamente formada por
-----------------------------------------------------------------------------------
1. _lexical analysis_: associa-se a cada _character_ um _token_
2. _syntax analysis_:
    - define-se novos símbolos, chamados de _constructors_
    - associa-se um _constructor_ a cada par `(char, token)`
    - constrói-se uma árvore, chamada de _abstract syntax tree_, cujos nós são _constructors_.
3. _semantic analysis_:
    - recebe a _abstract syntax tree_
    - utiliza dos "critérios pré-definidos" para decidir se um dado caminho caminho na árvore é válido ou não.
---------------------------------------------------------------------------------------

## 1.2 PYTHON: IMPLEMENTAÇÃO

Tipicamente, `python` consiste em:

1. compilação:
    - existe _linguagem de programação_ intermediária:
        - com seu próprio:
            - conjunto de _símbolos_ 
            - conjunto de _lexical categories_
            - sistema de validação
    - junto de um "dicionário" que transforma:
        - _abstract syntax tree_ de Python
        - em _source code_ na linguagem _intermediária_

2. interpretação:
    - existe um _executável_ na máquina que:
        - recebe uma _string_
        - verifica se ela é _válida_ na linguagem _intermediária_
        - executa tal _string_ na máquina

-------------------------------------------------------
__OBS.__ Normalmente:
-------------------------------------------------------
1. os símbolos da linguagem _intermediária_ são:
   - _opcodes_
   - _operands_
2. o sistema de validação constrói:
    - não uma _abstract syntax tree_
    - mas uma _stack_
3. o _executável_ na máquina é:
    - um binário
    - obtido da compilação de um software
    - escrito em alguma linguagem de programação compilada
        - `C`, `C++`
        - `Java`, `C#`
        - `Go`, `Rust`
        - ...
    - o qual emula uma máquina de Turing
    - cujas funções que manejam:
        - estado
        - armazenamento em RAM
        - ...
    - recebem variáveis do tipo "byte array"
    - sendo, portanto:
        - capazes de implementar _stacks de opcodes e operands_.
4. tal _executável_ é chamado de _interpretador_.
5. é nesse sentido que se diz que Python é:
    - _compilado_ pra `bytecode`
    - _interpretado_ em uma VM.
---------------------------------------------------------

## 1.3 PYTHON: VARIABILIDADE

- existem várias implementações `python` de Python:
    1. `CPython`
    2. `PyPy`
    3. `MicroPython`
    4. ...

- para cada uma delas tem-se:
    - uma _linguagem intermediária_
    - um _executável_ na máquina

## 1.4 PYTHON: PRESCRIÇÃO `vs` IMPLEMENTAÇÃO

- vimos acima que:
    - Python é a prescrição de uma linguagem de programação
    - `python` é a implementação de Python em uma máquina

- para a _mesma_ prescrição:
    - pode-se ter diferentes _implementações_
    - as quais podem ser executadas em uma _mesma_ máquina

- cada _implementação_ determina:
    - se uma _string_ é (isto é um _source code_):
        - _válida_
        - _inválida_

-------------------------------------------------------------
__PERGUNTA.__
-------------------------------------------------------------
- dado um _source code_ de Python, ele pode ser
    - _válido_ em uma implementação 
    - _inválido_ em outra?
-------------------------------------------------------------

## 1.5 PYTHON: UNIVOCIDADE

1. uma _sentença_ é formada por:
    - um _contexto_, formado de _elementos_
    - um regra `F: context -> bool`:
        - diz se um elemento `element` é:
            - _válido_
            - _inválido_
        - enquanto membro do contexto `context`.

2. dada uma sentença `F: context -> bool`:
    - diz-se que ela é uma _sentença sobre Python_, se:
        - o contexto `context` contém Python

-------------------------------------------------------------
__DEFINIÇÃO.__
-------------------------------------------------------------
- seja `F: context -> bool` uma sentença sobre Python.
- diz-se que `F` é _bem definida_, se
    - o fato de `element in context` ser:
        - _válido_
        - _inválido_
    - independe da implementação `python` de Python.
-------------------------------------------------------------

====================================================================
2. DISTRIBUIÇÕES
====================================================================

- uma _distribuição_ é:
    - uma _implementação_ Python
    - junto de uma coleção de _bibliotecas builtins_

====================================================================

## 2.1 DISTRIBUIÇÕES: MÓDULOS 

- um _módulo Python_ é:
    - um arquivo de _texto_  `module.py`
        - no _file system_ de uma _máquina_
        - cujo conteúdo é um _source code_ de Python

- considere:
    - um módulo Python  `module.py`
    - uma implementação `python` de Python.

- diz-se que `module.py` é:
    - _válido_ em `python`
    - se `python module.py` não retorna nenhum erro.

-----------------------------------------------------
__PERGUNTA.__
-----------------------------------------------------
- considere a _sentença sobre Python_ `F: context -> bool`:
    => um módulo `module.py` é _válido_ em `python`

- `F` é _bem definida_?
-----------------------------------------------------

## 2.2 DISTRIBUIÇÕES: IMPORTÁVEIS

- sejam:
    - `python` uma implementação Python
    - `module.py` um módulo Python _válido_ em `python`.
    - `some_file` algum arquivo.

- seja `new_module.py` o arquivo de texto:
    - obtido de `module.py`
    - adicionando a linha `import some_file`

- diz que `some_file` é:
    - _importável em_ `module.py` (com respeito à `python`), se:
        - o arquivo de texto `new_module.py` é:
            - um módulo Python _válido_ em `python`.

- diz-se que `some_file` é:
    - _importável_ (ou _biblioteca_) em `python`, se:
        - é _importável em algum módulo_ Python
            - o qual é _válido_ em `python`

-----------------------------------------------------
__PERGUNTA.__
-----------------------------------------------------
- considere a _sentença sobre Python_ `F: context -> bool`:
    => um arquivo `some_file` é _importável_ em `python`

- `F` é _bem definida_?
-----------------------------------------------------


## 2.3 DISTRIBUIÇÕES: `MÓDULOS <= IMPORTÁVEIS`

- sejam:
    - `python` uma implementação Python
    - `dir/` um diretório
    - `dir/module.py` um módulo Python _válido_ em `python`

----------------------------------------------------------
__FATO.__
----------------------------------------------------------
- se `dir/other_module.py` é qualquer outro:
    - módulo Python _válido_ em `python`
    - cujo diretório é `dir/`
- então:
    - `module.py` é _importável em_ `other_module.py`
    - `other_module.py` é _importável em_ `module.py`
---------------------------------------------------------

--------------------------------------------------------
__CONCLUSÃO.__
--------------------------------------------------------
- todo módulo Python
    - que é válido em _alguma_ implementação Python
    - é importável em  _qualquer_ implementação Python
---------------------------------------------------------

## 2.4 DISTRIBUIÇÕES: IMPORTABILIDADE

- como parte da _prescrição_ (do sistema de importação) de Python:
- para todo _módulo_ `module.py`
- está associada uma lista `IMPORTABLES(module.py)`:
    - formada de diretórios `dir1, dir2, ...` no file system
    - determinados em _tempo de execução_
    - tais que:
        - uma biblioteca `lib` é importável em `module_py`
        - somente se:
            - `lib` está na raiz de algum diretório de `IMPORTABLES(module.py)`.

- O __FATO__ anterior significa que:
    - para todo módulo Python `module.py`:
        - seu diretório está em `IMPORTABLES(module)`.    

## 2.5 DISTRIBUIÇÕES: EXTENSÕES

- sejam:
    - `python` uma implementação Python
    - `some_file` um arquivo

- diz-se que:
    - `some_file` é uma _extensão_ Python em `python`, se:
        - é _importável_ em `python`
        - __não é__ um módulo Python em `python`

-----------------------------------------------------
__PERGUNTA.__
-----------------------------------------------------
- considere a _sentença sobre Python_ `F: context -> bool`:
    => um arquivo `some_file` é uma _extensão_ em `python`

- `F` é _bem definida_?
------------------------------------------------------

## 2.6 DISTRIBUIÇÕES: DEFINIÇÃO

- uma _distribuição_ Python é formada por:
    1. uma _implementação_ Python `python`
    2. um conjunto `libs` de _bibliotecas_ Python em `python`

---------------------------------------------------------------------
__NOTAÇÃO.__ no que segue:
---------------------------------------------------------------------
- `dist`            denota uma _distribuição_ Python
- `dist.python`     denota sua _implementação_ Python
- `dist.libs`       denota seu conjunto de _bibliotecas_ Python em `python`
- `dist.modules`    denota os _módulos_ em `dist.libs`
- `dist.extensions` denota as _extensões_ em `dist.libs`
----------------------------------------------------------------

## 2.7 DISTRIBUIÇÕES: BUILTINS

- sejam:
    - `dist` uma _distribuição_ Python
    - `lib` uma _biblioteca_ Python em `dist.python`

- dizemos que `lib` é _builtin_ em `dist` se:
    - `lib` pertence à `dist.libs`.

## 2.8 DISTRIBUIÇÕES: PSF

- como vimos:
    - Python é a _prescrição_ de uma linguagem de programação
    - `CPython` é uma _implementação_ Python

- ambos são mantidos:
    - pela `Python Software Foundation`.
    - com contribuições da comunidade

- ela também mantém:
    - uma _distribuição_ Python:
        - normalmente chamada de `CPython`
        - contendo _bibliotecas_ builtins

- dentre as bibliotecas, tem-se:
    - _módulos_ Python:
        - `os`
        - `io`
        - `re`
        - `logging`
        - `pathlib`
        - ...
    - _extensões_ Python:
        - `sys`
        - `asyncio`
        - `math`
        - `unicodedata`
        - `zlib`
        - ...

---------------------------------------------------------------------------
__OBSERVAÇÃO.__
----------------------------------------------------------------------------
- dadas duas distribuições Python `dist1` e `dist2`:
    - todo módulo em `dist1.modules` é válido em `dist2.python`
    - todo módulo em `dist2.modules` é válido em `dist1.python`

- de outro lado:
    - uma extensão em `dist1.extensions` pode não ser válida em `dist2.python`
    - uma extensão em `dist2.extensions` pode não ser válida em `dist1.python`
-------------------------------------------------------------------------------

======================================================================
3. ENVIRONMENT
======================================================================

- ambientes são:
    - como _distribuições_
    - mas associados a um diretório

## 3.1 ENVIRONMENT: DEFINIÇÃO

- seja:
    - `dir/` um diretório no file system

- um _ambiente_ Python para `dir/` é formado por:
    - uma implementação Python `python`
    - um conjunto `env_libs` de bibliotecas
    - tal que:
        - se:
            - `lib` é biblioteca em `env_libs`
            - `module.py` é um módulo Python em `dir/**/*`
        - então:
            - `lib` é _importável_ em `module.py`

`--------------------------------
env_libs = {lib1, lib2, ...}

dir/
  |-- module.py
  |-- ...
  |-- subdir/
  |     |-- other_module.py
  |     `-- ...
  `-- ...
---------------------------------`

```python
# in module.py
import lib1, lib2, ...

# in subdir/other_module.py
import lib1, lib2, ...
...

```

## 3.2 ENVIRONMENT: AFIRMAÇÃO

- sejam:
    - `python` uma implementação Python 
    - `libs` um conjunto de bibliotecas 
    - `dir/` um diretório 

---------------------------------------------------------------------
__AFIRMAÇÃO__
---------------------------------------------------------------------
- sejam quais forem:
    - `python`
    - `libs`
    - `dir/`
- é sempre possível tornar `(python, libs)` um ambiente para `dir/`
----------------------------------------------------------------------

## 3.3 ENVIRONMENT: CONSTRUÇÃO

- para que `python, libs` definam _environment_ para `dir/`, precisamos:
    1. conseguir _executar_ `python` em cada módulo em `dir/**/*`
    2. conseguir _importar_ cada `lib` de `libs` em cada módulo em `dir/**/*`


## 3.4 ENVIRONMENT: CONSTRUÇÃO, parte 1

- o que significa "executar" `python some_file` com o executável `python`?
    1. abrir _sessão_ com algum _shell_ por algum _usuário_.
        - carregar _ambiente_ associado à _sessão do shell_
            - variáveis de ambiente:
                - variáveis no _shell_ com valores pré-definidos
        - o arquivo `python` é _executável_:
            - o usuário que iniciou a sessão possui permissão de execução para `python`
        - sua execução:
            - pode ser _relativa_:
                - `python some_file`
                - se o diretório de `python` está no `PATH` da sessão do shell
            - pode ser _absoluta_:
                - `/path/to/python some_file`
    2. comunicar com _kernel_ via sessão do _shell_:
        - conversão do conteúdo de `some_file` em `bytecode`
        - leitura do `bytecode` como instruções pra máquina virtual
        - instruções pra máquina virtual executadas como instruções para o kernel

--------------------------------------------------------
__CONCLUSÃO 1.__
--------------------------------------------------------
- uma vez que um usuário possui permissão de execução do arquivo `python`:
    - para conseguir _executar_  `python` em um módulo de `dir/**/*`:
        - basta que `python` esteja no `PATH` da sessão.
        - ou que passemos seu caminho absoluto

-------------------------------------------------------

## 3.5 ENVIRONMENT: CONSTRUÇÃO, parte 2

- lembrete:
    - como parte da _prescrição_ do sistema de validação do Python:
        - para cada módulo `module.py` está associada lista `IMPORTABLES(module)`:
            - _determinada em tempo de execução_.
            - formada dos diretórios cujos módulos são importáveis em `module.py`

- mais precisamente:
    - `IMPORTABLES(module)` pode ser _modificada_ por uma variável de ambiente:
        - `PYTHONPATH=/path/to/dir1:/path/to/dir2:...`

---------------------------------------------------------------------
__CONCLUSÃO 2.__
----------------------------------------------------------------------
- para uma `lib` em `libs` seja importável em `module.py`
    - basta que seu diretório esteja no valor de `PYTHONPATH` da sessão.
-------------------------------------------------------------------------


## 3.6 ENVIRONMENT: CONSTRUÇÃO, parte 3

- da CONCLUSÃO 1:
    - executa-se `python` em `module.py` com:

```bash
# forma relativa
PATH=${PATH}:/path/to/python python module.py

# forma absoluta
/path/to/python module.py
```

- da CONCLUSÃO 2:
    - torna-se `lib1, lib2, ...` de `libs` importável em `module.py` com:

```bash
PATH=${PATH}:/path/to/python \
PYTHONPATH=${PYTHONPATH}:/path/to/dir1:/path/to/dir2 \
python module.py
```

## 3.7 ENVIRONMENT: PREFIXED

- para que possamos executar a construção anterior:
    - `PATH` pode ou não ser modificado
    - `PYTHONPATH` __precisa__ ser modificado

- existe maneira "pré-definida" de realizar tal construção:
    - sem alterar `PYTHONPATH`

--------------------------------------------------
__ALGORITMO__
--------------------------------------------------
1. crie diretório `/path/to/prefix/`:
2. crie subdiretórios:
    - `/path/to/prefix/bin/`
    - `/path/to/prefix/lib/`
3. colete uma implementação Python `python`
4. coloque tal implementação em `/path/to/prefix/bin/`
5. seja `<x.y.z>` a versão de `python`
6. crie o diretório `/path/to/prefix/python<x.y.z>/`
7. crie o diretório `/path/to/prefix/python<x.y.z>/site-packages/`
---------------------------------------------------

- feita a construção acima:
    - para todo módulo `module.py`
    - ao executar `/path/to/prefix/bin/python module.py`
    - a lista `IMPORTABLES(module)` contém `/path/to/prefix/python<x.y.z>/site-packages/`

-------------------------------------------
__OBSERVAÇÃO.__
-------------------------------------------
- tais ambientes são chamados de:
    - _prefixed environments_
- o diretório `/path/to/prefix/` é chamado de:
    - _prefixo_ do ambiente
-------------------------------------------

## 3.8 ENVIRONMENT: SYMLINKS

- ...

## 3.9 ENVIRONMENT: SYSTEM ENVIRONMENTS

- tipicamente:
    - instala-se uma distribuição Python em uma máquina
        - via _gerenciador de pacotes_
    - em tal distribuição
        - a implementação Python `python` fica em `/usr/bin/python`
        - as `libs` ficam em `/usr/lib/python<x.y.z>/`

- portanto:
    - tal distribuição define um ambiente:
        - cujo prefixo é `/usr`
    - ele é chamado de _system environment_

## 3.10: ENVIRONMENT: NECESSIDADE

- um ambiente Python é dito ser _virtual_ se:
    - é um _prefixed environment_
    - na máquina existe um _system environment_
    - o _prefixo_ do ambiente é diferente do prefixo do _system environment_

------------------------------------------------
__CONCLUSÃO.__
----------------------------------------------
- usa-se um ambiente (virtual ou customizado) quando:
    - o _system environment_ possui libs que:
        - não devem fazer parte do novo ambiente

-----------------------------------------------

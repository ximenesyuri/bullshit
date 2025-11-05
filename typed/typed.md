# OBJETIVO

- o que é um sistema de tipos?
- quais são suas características?
- o que é uma linguagem de programação?
- em que sentido uma linguagem de programação é um sistema de tipos?
- o que é um sistema de tipos com "type safety"?
- quais são as características do sistema de tipos do Python?
- o sistema de tipos do `typed`
- exemplos práticos

# SISTEMAS DE TIPOS

- origens
    - conjuntos nos fundamentos da matemática
    - conjunto de todos os conjuntos
        - paradoxo de Russell
    - 'tipo' dos conjuntos
    - 'tipo' dos 'tipos'
        - paradoxo de Giraud
    > _teoria de tipos (homotópicos) provê uma base formal para a matemática_

- linguagens formais
    - `L = (A, G(A))`
    - alfabeto: `A`
    - gramática: `G(A)`
    - sintaxe (regras gramaticais)
        - instruções sobre como construir `G(A)`
    - texto: uma parte de `G(A)`.

- sistemas de tipos
    - alfabeto: `A = Terms + Types + Rel`
        - termos `Term`
        - tipos `Type`
        - relações entre tipos `Rel`
    - notação
        termos e tipos
            - `x: X`
        - relações entre tipos (funções)
            - `f: X -> Y` (para todo `x: X`, existe `f(x): Y`)
            - `f(x: X) -> Y`

- classificação
    - funcionais: 
        - funções são termos
            - `f: Function`
            - `f: Function(X, Y)`
    - subtipos:
        - tipo cujos termos são de outro tipo
            - `X <= Y` (`x: X` implica `x: Y`)
    - meta-tipos:
        - tipo cujos termos são tipos
            - tipos `BLABLA` tais que se `T: BLABLA`, então `T` é um tipo
    - universo
        - tipo cujos termos são _precisamente_ os tipos
            - `TYPE` tal que `T: TYPE` se, _e somente se_, `T` é um tipo
        > OBS: se possui subtipos, então possui meta tipos:
            > `X < TYPE`
            > `META: TYPE`, `X: META` se, e somente se, `X < TYPE`
    - dependentes  ("tipos dinâmicos")
        - `x: X(y)`

- construção de tipos
    - tipos primitivos e derivados
    - exemplo:
        - funções que retornam tipos (type factories)
            - `F(x: X) -> TYPE` (caso com universo)
            - `F(x: X) -> Y`, com `Y: META` (caso com meta-tipos)

- relação entre linguagens formais
    - caso geral: `F: L1=(A1,G(A1)) -> L2=(A2,G(A2))`
        - alfabeto mapeado em alfabeto
            - `FA: A1 -> A2`
        - preservando regras gramaticais
            - texto mapeado em texto
                - `FT: T in G(A1) -> F(T) in G(A2)`
        
    - sistemas de tipos: `F: L1=(Term1, Type1) -> L2=(Term2, Type2)`
        - termos levados em termos
            - `FTerm: Term1 -> Term2`
        - tipos levados em tipos
            - `FType: Type1 -> Type2`
        - preservando relações de determinação do tipo
            - se `x in Term1`
            - se `X in Type1`
            - se `x: X` em `L1`
            - então `FTerm(x) in Term2`
            - então `FType(X) in Type2`
            - então `FTerm(x): FType(X)` em `L2`
    
# LINGUAGENS DE PROGRAMAÇÃO

- linguagens com validação
    - linguagens formais

- linguagem de máquina
    - 'baixo nível'
    - possível de ser "executada em uma máquina"
        - validação: `sucesso` ou `erro`
    - tempo de execução

- linguagens de programação:
    - linguagens formais que admitem compilação para linguagem de máquina
    - tempos:
        - de compilação
        - de execução
    - linguagens de 'alto nível'
        - alfabeto `A = V + F`
            - variáveis `V`
            - funções `F` (relações entre variáveis)
    - linguagens OOP:
        - variáveis possuem atributos (armazenam "objetos")

- máquinas de Turing 
    - linguagem de máquina "abstrata"
    - linguagens Turing completas
        - implementam máquinas de Turing
        - implementam qualquer algoritmo executável em uma máquina

- sistemas de tipos:
    - variáveis são termos
    - tipos primitivos são pré-construídos
    - construção de novos tipos:
        - classes (linguagens com OOP baseadas em classes)
        - algebraic data types (linguagens com FP)
    > _toda linguagem de programação moderna possui um sistema de tipos_

    - tipagem estática:
        - "linguagens tipadas"
        - validação do tipo das variáveis em tempo de _compilação_.
    - tipagem dinâmica:
        - "linguagens não tipadas"
        - validação do tipo das variáveis em tempo de _execução_.
- tipagem forte/fraca
- segurança de tipos
    - sistema de validação de tipos
    - construir tipos customizados
        - funcional
    - tipos customizáveis
        - tipos "dinâmicos" (??)
        - tipos dependentes

# PYTHON

- características:
    - interpretada
    - de alto nível
    - Turing completa
- sistemas de tipos:
- tipos primitivos
    - estáticos
    - em C
- tipagem forte e dinâmica
- funcional
- 

# TYPED

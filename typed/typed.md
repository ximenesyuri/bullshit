# OBJETIVO

- o que é um sistema de tipos?
- quais são suas características?
- o que é uma linguagem de programação?
- em que sentido uma linguagem de programação é um sistema de tipos?
- o que significa dizer que um sistema de tipos apresenta "type safety"?
- quais são as características do sistema de tipos do Python?
- o sistema de tipos do `typed`
- exemplos práticos (se der tempo [segunda apresentação?])

# SISTEMAS DE TIPOS

## origens

- conjuntos nos fundamentos da matemática
    - _conjunto_: é aquilo que possui elementos
    - _função_: é um mapeamento entre conjuntos, preservando elementos
- conjunto de todos os conjuntos
    - paradoxo de Russell
- tipo nos fundamentos da matemática
    - _tipo_: são símbolos fundamentais
    - _termo_: é aquilo que possui um tipo
    - _função_: é um mapeamento entre tipos, preservando termos
- 'tipo' dos conjuntos
- 'tipo' dos 'tipos'
    - paradoxo de Giraud

> _teoria de tipos (homotópicos) provê uma base formal para a matemática_
    - e também para a computação
    - assim como para a física

## linguagens formais

- `L = (A, G(A))`
    - alfabeto: `A`
    - gramática: `G(A)`
- sintaxe (regras gramaticais)
    - instruções sobre como construir `G(A)`
- texto `T(L)`: uma parte da gramática `G(A)`.

## sistemas de tipos

- são linguagens formais especiais
- alfabeto: `A = Terms + Types + Rel`
    - termos `Term`
    - tipos `Type`
    - relações entre tipos (funções) `Rel`

## notação

- termos e tipos
    - `x: X`
- relações entre tipos (funções)
    - `f: X -> Y` (para todo `x: X`, existe `f(x): Y`)
    - `f(x: X) -> Y`

## classificação

- funcionais
    - funções são termos
    - `f: Function` (minimamente funcional)
    - `(f: X -> Y): Function(X, Y)` (fortemente funcional)

- subtipos
    - tipo cujos termos são de outro tipo
    - `X <= Y` (`x: X` implica `x: Y`)

- meta-tipos
    - tipo cujos termos são tipos
    - tipos `BLABLA` tais que se `T: BLABLA`, então `T` é um tipo

- universo
    - tipo cujos termos são _precisamente_ os tipos
    - `TYPE` tal que `T: TYPE` se, _e somente se_, `T` é um tipo

> OBS: se possui subtipos e universo, então possui meta tipos:
>   - `X` é meta tipo se `X < TYPE`
>   - `META: TYPE`, `X: META` se, e somente se, `X < TYPE`

- dependentes
    - "tipos dinâmicos"
    - `x: X(y)`

## construção de tipos

- tipos primitivos e derivados
- funções que retornam tipos (type factories)
    - `F(x: X) -> TYPE` (caso com universo)
    - `F(x: X) -> Y`, tal que `Y: META` (caso com meta-tipos)
- se a linguagem é (fortemente) funcional:
    - tipo `Factory(X) = Function(X, TYPE)` das type factories 

## relações entre linguagens

- caso geral: 
    - `F: L1=(A1,G(A1)) -> L2=(A2,G(A2))`
    - alfabeto mapeado em alfabeto: `F: A1 -> A2`
    - preservando regras gramaticais `F: G(A1) -> G(A2)`
    - texto mapeado em texto `F: T(L1) -> T(L2)`
        
- sistemas de tipos: 
    - `F: L1=(Term1, Type1) -> L2=(Term2, Type2)`
    - termos levados em termos: `FTerm: Term1 -> Term2`
    - tipos levados em tipos: `FType: Type1 -> Type2`
    - preservando relações de determinação do tipo
    - se `x in Term1`, então `FTerm(x) in Term2`
    - se `X in Type1`, então `FType(X) in Type2`
    - se `x: X` em `L1`, então `FTerm(x): FType(X)` em `L2`
    
# LINGUAGENS DE PROGRAMAÇÃO

## linguagens com validação

- linguagem formal `L=(A, G(A))` dotada de:
    - `Status` (representando possíveis status da validação)
    - `eval: Textos -> Status` (representando o validador de textos)

- relação entre linguagens com validação:
    - relação entre linguagens formais `L1=(A1,G(A1)) -> L2=(A2,G(A2))`
    - que comuta com validador:
    - `eval1(t) = eval2(F(t))` para todo `t in T(L1)`.
    - costuma ser chamada de _compilação_.

## linguagens de máquina

- é aquela que pode de ser "executada em uma máquina"
- é uma linguagem com validação:
    - `Status = (Erro, Sucesso)`
    - `eval: Textos -> (Erro, Sucesso)`

- tempo de execução
- dita ser de "baixo nível"

- máquina de Turing:
    - linguagem formal com validação que:
        - representa uma linguagem de máquina "abstrata"
        - na qual todo algoritmo pode ser executado.

## linguagens de programação

- é um caso especial de linguagem formal `L=(A, G(A))` com validação:
    - _aquela que admite compilação para uma linguagem de máquina_

- tempos:
    - de compilação
    - de execução

- tipicamente `A = V + F + O`
    - variáveis `V`
    - funções `F` (relações entre variáveis)
    - operadores entre variáveis `O`

- linguagens Turing completas
    - admitem compilação para uma máquina de Turing
    - implementam qualquer algoritmo executável em uma máquina

## sistemas de tipos

- variáveis são termos:
    - variáveis possuem tipos
        - possui validador básico que checa se uma variável é de um dado tipo
    - se admite subtipos
        - possui validador que checa se um tipo é subtipo de outro tipo dado

- linguagem segue paradigma:
    - _funcional_ (FP) se é funcional enquanto linguagem formal
        - funções possuem tipo
    - _orientado a objetos_ (OOP) se "variáveis admitem atributos"
        - atributos são definidos no tipo da variável
        - tipos possem ser "decorados" com atributos

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

- tipagem forte
    - o tipo do retorno de uma função é "bem definido" em termos do tipo de seus argumentos

## type safety

- linguagem de programação apresenta _type safety_ se:
    - todo texto que apresenta um erro pode ser reconstruído de tal forma que o erro ocorre em seu validador básico

> _uma linguagem apresenta type safety se todo erro é reprodutível como um_ `TypeError`.
 
- sistema de tipos confiável
    - fortemente tipado
- sistema de tipos extremamente flexível
    - ser possível construir tipos de acordo com a necessidade
        - fortemente funcional
        - com meta tipos
        - com subtipos
        - tipos com atributos
        - tipos "dinâmicos"
        - tipos dependentes
        - type factories
        - ...
- checagem não só de variáveis, mas de funções
    - funções devem ter tipos de entrada e saída checados em algum tempo

# PYTHON

## características

- linguagem de programação:
    - de alto nível
    - interpretada (admite uma validação mais "direta")
    - Turing completa

## sistema de tipos

- fortemente tipada
    - (requisito mínimo para garantia de type safety)

- dinamicamente tipada
    - tipos de variáveis determinados em tempo de execução

- validação básica do tipo de um termo:
    - `isinstance`: foco em _instanciamento_

- _fracamente_ funcional:
    - existe o tipo `Function` das funções

- tipos possuem atributos
    - os atributos são dinâmicos...
        - ...exceto para tipos primitivos, construídos em C

- admite universo:
    - tipo primitivo (estático) `type`

- construção de tipos:
    - através de _classes_

- admite meta tipos:
    - através de _metaclasses_

- admite "versão fraca" de tipos:
    - `issubclass`: foco em _herança_
    - se `issubclass(X, Y) is True`, então, para todo `x` tal que `isinstance(x, X) is True`, tem-se `isinstance(x, Y) is True`
    - recíproca é falsa

- checagem to tipo de variáveis e retorno em funções:
    - sugestivo com `type annotations`
    - a ser implementado de forma estática por ferramentas externas

## garantia de type safety

- pontos de atenção:
    - checagem de tipos com foco em instanciamento
    - versão fraca de subtipos
    - fracamente funcional
    - tipos primitivos estáticos

- características faltantes:
    - não admite checagem de tipos para funções
    - não admite type factories
    - não admite tipos dependentes

# TYPED

## sobre

- framework que constrói novo sistema de tipos para Python
    - focado na garantia de type safety

- zero dependências (só Python >= 3.9)
    - leve e rápido

- customizável
    - usuário define quando utilizar checagens
    - e quais checagens quer utilizar

- cobre:
    - todos os pontos de atenção
    - todas as características faltantes

- fornece uma gama de type factories prontas pra uso
- incorpora sistema de "modelos" em seu sistema de tipos:
    - modelos com propriedades customizáveis

- permite ao usuário construir seu próprio sistema de tipos

## sistema de tipos



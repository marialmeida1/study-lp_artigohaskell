# Operações sobre Dados Quânticos em Haskell

## Definindo Operações com Haskell

Em Haskell, as operações fundamentais da computação quântica podem ser modeladas como **funções puras**, aproveitando-se das características funcionais da linguagem. Isso inclui, por exemplo, a modelagem da porta **Hadamard** ou da operação **NOT** como funções que recebem e retornam valores quânticos (`QV a`), manipulando diretamente as amplitudes de probabilidade.

Essas funções operam sobre estruturas do tipo `QV a`, que representam superposições de valores do tipo `a`, com amplitudes associadas a cada vetor unitário. O efeito de uma operação sobre os dados quânticos é representado por uma matriz (um mapeamento entre pares `(entrada, saída)`), implementada como `Qop a b`.

A aplicação de uma operação a um valor quântico é realizada por meio da função `qApp`, que realiza a multiplicação da matriz pela representação vetorial do estado quântico:

```haskell
qApp :: (Basis a, Basis b) ⇒ Qop a b → QV a → QV b
```

Operações comuns, como a porta **NOT** (`qnotop`) e a **Hadamard** (`hadamardop`), são definidas como matrizes explícitas. Isso permite uma implementação clara e matemática de operações quânticas usando apenas as ferramentas do Haskell.

## Composição de Funções para Quantum Gates

O Haskell permite **compor funções de forma modular**, o que se alinha perfeitamente com a natureza **modular dos circuitos quânticos**. Operações podem ser compostas e reutilizadas combinando-se `Qop`s com funções de ordem superior.

Uma abstração particularmente poderosa é a **operação controlada**, como `cop`, que permite aplicar uma transformação a um qubit **condicionalmente ao valor de outro**. Essa abstração modela operações como o **CNOT** e o **Toffoli** de forma genérica:

```haskell
cop :: (Basis a, Basis b) ⇒
       (a → Bool) → Qop b b → Qop (a, b) (a, b)
```

Com isso, é possível construir portas compostas e gerar **pares entrelaçados** (entangled), como demonstrado pela aplicação do `CNOT` sobre uma superposição e um valor fixo.

Além disso, funções clássicas reversíveis podem ser "elevadas" a operações quânticas com `opLift`, desde que mantenham a **unitariedade**, característica obrigatória em operações quânticas. Funções não reversíveis, como `and`, podem ser adaptadas para esse contexto adicionando variáveis auxiliares que preservam as entradas.

Esses mecanismos demonstram como Haskell oferece uma **estrutura expressiva e segura** para o desenvolvimento de algoritmos quânticos, mantendo as propriedades fundamentais da computação quântica.
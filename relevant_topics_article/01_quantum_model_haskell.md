# Modelagem de Computação Quântica em Haskell

## Abordagem Funcional para Computação Quântica

O artigo "Modeling Quantum Computing in Haskell", de Amr Sabry, apresenta uma proposta de modelagem da computação quântica utilizando a linguagem funcional Haskell. A proposta é fundamentada em dois objetivos principais:

1. **Explicar computação quântica** em um nível de abstração acessível à comunidade de programadores, sem exigir o aparato físico tradicional.
2. **Explorar a adequação das abstrações funcionais** à modelagem da computação quântica.

Haskell, sendo uma linguagem puramente funcional, se destaca por permitir uma representação natural dos conceitos matemáticos fundamentais da computação quântica, como espaços vetoriais e álgebra de matrizes.

### Superposição e Qubits em Haskell

Um qubit é modelado como uma superposição de estados, representado por uma estrutura de mapeamento (`QV a`) entre valores clássicos e amplitudes de probabilidade complexas:

```haskell
type PA = Complex Double
type QV a = FiniteMap a PA
```

Por exemplo:
```haskell
qFT = qv [(False, 1/√2), (True, 1/√2)]
```

Esse valor representa um qubit em superposição de `False` e `True`.

### Operações Quânticas como Funções Declarativas

Operações sobre qubits, como **negação (qnot)** e **transformada de Hadamard**, são representadas por funções puramente declarativas:

```haskell
qnotf :: QV Bool -> QV Bool
hadamardf :: QV Bool -> QV Bool
```

Tais operações correspondem a multiplicações de matrizes e vetores, reforçando o alinhamento entre computação quântica e programação funcional.

### Entrelaçamento e Estruturas Compostas

Além da superposição, o modelo suporta **entrelaçamento** (entanglement), por meio de estruturas compostas como `QV (a, b)`. Haskell permite representar entrelaçamento, essencial para a computação quântica, com clareza estrutural e sem a necessidade de efeitos colaterais iniciais.

---

## Diferença com Linguagens Imperativas

A modelagem de fenômenos quânticos como **superposição** e **entrelaçamento (entanglement)** é significativamente mais natural em Haskell do que em linguagens imperativas. O artigo destaca que **Haskell oferece vantagens significativas sobre linguagens imperativas** na modelagem da computação quântica:

- A representação de estados quânticos exige manipulações de vetores e matrizes — operações matemáticas bem integradas ao paradigma funcional.
- Em linguagens imperativas, modelar estados simultâneos (superposição) ou dependência entre valores (entrelaçamento) exige estruturas complexas, muitas vezes artificiais ou ineficientes.
- Já Haskell representa esses conceitos de forma **natural e declarativa**, utilizando tipos e funções puras.

O entrelaçamento, que torna impossível a separação de estados componentes, desafia o raciocínio composicional típico de linguagens imperativas. Em Haskell, esse problema é contornado com estruturas como *valores virtuais* e *adaptadores*, que permitem isolar e operar sobre partes entrelaçadas de uma estrutura de dados sem comprometer a consistência do sistema. Por exemplo, qubits entrelaçados podem ser manipulados com segurança em estruturas compostas sem necessidade de modificar diretamente o estado global — algo que seria extremamente difícil (ou propenso a erros) em linguagens imperativas.

Além disso, a observação (medição) de um valor quântico altera seu estado de forma irreversível. Esse efeito colateral é modelado em Haskell com referências (`IORef`) que armazenam e atualizam o estado colapsado após a medição, uma abordagem mais clara e segura do que em paradigmas imperativos puros.

Haskell também permite a composição de operações quânticas usando funções de ordem superior, como `qApp`, `cop` e `app`, facilitando a simulação de circuitos complexos como o operador Toffoli ou o algoritmo de Deutsch, sempre respeitando propriedades fundamentais das operações quânticas, como unitariedade e reversibilidade.

---

## Conclusão

A linguagem Haskell se mostra bem alinhada com os fundamentos matemáticos da computação quântica. Sua abordagem funcional facilita a representação de conceitos como superposição e entrelaçamento, oferecendo um ambiente claro, expressivo e declarativo para a construção de modelos e algoritmos quânticos. Essa vantagem não é facilmente replicável em linguagens imperativas, onde a natureza sequencial e mutável do estado representa um obstáculo à modelagem fiel do comportamento quântico.
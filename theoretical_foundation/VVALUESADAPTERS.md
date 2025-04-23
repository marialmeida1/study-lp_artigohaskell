# Valores Virtuais e Adaptadores para Entrelaçamento

Na computação quântica, frequentemente nos deparamos com estruturas de dados compostas por vários qubits interconectados por emaranhamento. A dualidade onda/partícula surge como um dilema na modelagem funcional desses sistemas: como manipular individualmente os qubits (como se fossem partículas) quando eles fazem parte de uma superposição global (como uma onda)?

Para ilustrar essa dificuldade, consideremos um circuito quântico composto por três qubits: `top`, `middle` e `bottom`, em um sistema potencialmente emaranhado. Deseja-se aplicar operações quânticas a pares distintos de qubits em sequência, como `hadamard` no `bottom`, seguida de operações controladas entre os pares `(middle, bottom)`, `(top, middle)` e `(top, bottom)`.

No entanto, dado que os três qubits compartilham um mesmo estado emaranhado, não é possível isolá-los diretamente sem comprometer a coerência do sistema global. A tentativa ingênua de acessar diretamente `bottom` para aplicar `hadamard`, por exemplo, pode falhar se não for respeitada a estrutura do emaranhamento.

---

### Valores Virtuais: A Solução

Para resolver esse problema, o artigo propõe o uso de **valores virtuais**, uma técnica inspirada no padrão de projeto **Fachada**. Um valor virtual permite que uma parte específica de uma estrutura de dados quântica seja acessada e manipulada como se estivesse isolada, mesmo estando profundamente aninhada e emaranhada com outras partes do sistema.

Um valor virtual é definido por:

- Uma **referência global** ao estado quântico completo.
- Um **adaptador** que mapeia entre a estrutura completa e a parte de interesse.

Por exemplo, dada a estrutura:

```
(((a, b, c), (d, e)), (f, g))
```

É possível extrair um valor virtual correspondente ao par `(d, g)`, mesmo que esses elementos não estejam diretamente agrupados. O adaptador fornece a lógica de "decomposição" (`dec`) e "reconstrução" (`cmp`) da estrutura.

---

## Adaptadores para Manipulação de Dados Quânticos Entrelaçados

Quando modelamos computações quânticas em uma linguagem funcional como Haskell, deparamo-nos com o desafio de manipular **dados entrelaçados**, como tuplas aninhadas que representam qubits emaranhados. A linguagem Haskell, por meio de seu sistema de tipos e suporte à abstração funcional, oferece uma solução elegante para esse problema: os **adaptadores**.

Um **adaptador** é um mapeamento bidirecional entre uma estrutura de dados global (potencialmente complexa e emaranhada) e uma subestrutura de interesse. Ele é composto por duas funções principais:

- `dec` (decomposição): extrai o valor-alvo da estrutura total.
- `cmp` (composição): reconstrói a estrutura total a partir do valor-alvo e de seus "vizinhos" no emaranhado.

Esses adaptadores são utilizados em conjunto com **valores virtuais** para fornecer um acesso seguro e localizado a componentes individuais, sem quebrar a integridade da estrutura global.

Por exemplo, se desejamos operar apenas sobre o par `(d, g)` da estrutura mencionada acima, criamos um adaptador que nos permite isolar `(d, g)` e tratá-lo como se fosse um valor independente. Isso é feito sem extrair fisicamente os valores da estrutura, mas apenas fornecendo uma visão **virtual** coerente, que respeita o estado emaranhado do sistema. Assim, cada componente pode ser manipulado por operações quânticas sem exigir reestruturações manuais ou cópias de dados.

Esse mecanismo se assemelha ao padrão de projeto **Fachada** da programação orientada a objetos, permitindo acesso simplificado a uma estrutura complexa por meio de uma interface enxuta e específica.

---

### Aplicando Operações com `app` e `app1`

Com valores virtuais definidos, é possível aplicar operações quânticas diretamente a eles, mesmo que estejam inseridos em estruturas maiores. A função `app` aplica uma operação quântica entre dois valores virtuais, cuidando para preservar as correlações com os demais qubits:

```haskell
app :: Qop a b -> Virt a nab ua -> Virt b nab ub -> IO ()
```

Se queremos apenas aplicar uma operação a um único valor virtual (como `hadamard`), utilizamos `app1`, que reaproveita a mesma referência para entrada e saída.

---

## Facilidade de Integração com Estruturas Complexas

Um dos grandes benefícios do uso de valores virtuais e adaptadores em Haskell é a **integração suave com estruturas de dados complexas**, como registros compostos ou múltiplos níveis de tuplas aninhadas. Diferentemente de linguagens imperativas, onde manipular componentes internos requer mutações explícitas e cuidados manuais com dependências, Haskell promove uma abordagem **modular, composicional e funcional**.

Ao modelar uma computação quântica com valores entrelaçados, frequentemente desejamos aplicar uma operação em um único componente de uma tupla, ou em um par de componentes não consecutivos. Com valores virtuais, isso se torna direto:

1. Utiliza-se um adaptador que identifica o subcomponente de interesse.
2. Esse subcomponente é encapsulado em um valor virtual (`Virt a na u`), onde `a` é o valor de interesse, `na` representa os "vizinhos emaranhados" e `u` é a estrutura total.
3. A operação desejada (como `hadamard`, `qnot`, `cop`, etc.) é aplicada ao valor virtual com `app1` ou `app`.

Por exemplo, para aplicar uma operação `Qop Bool Bool` a um qubit que está embutido em uma estrutura complexa com outros qubits, bastaria:

```haskell
let qubitVirt = Virt ref adaptador
app1 hadamard_op qubitVirt
```

Mesmo que esse qubit esteja emaranhado com outros, o mecanismo garante que as atualizações feitas respeitem a coerência global do sistema. Isso significa que o programador pode **trabalhar localmente, mas com segurança global** — um dos pilares da programação funcional aplicada à computação quântica.

Além disso, os adaptadores são **sistemáticos e composicionais**. Isso permite que sejam gerados automaticamente a partir dos tipos das estruturas envolvidas. Assim, estruturas como `(a, (b, c))` ou `((a, b), (c, d))` podem ter adaptadores gerados por convenção, como `ad_pair1`, `ad_triple12`, `ad_triple23`, etc., permitindo que programadores se concentrem na lógica quântica, sem se preocupar com a complexidade da estruturação de dados.

---

### Observando um Valor Virtual

A medição de um valor virtual é realizada com a função `observeVV`, uma generalização de `observeLeft`. Essa função calcula a distribuição de probabilidade marginal para o subcomponente de interesse (por exemplo, o qubit `bottom`), escolhe uma saída probabilisticamente, e então colapsa toda a estrutura global de forma consistente com essa observação.

Esse comportamento respeita a natureza do emaranhamento: embora se opere sobre um "subcomponente", a atualização afeta o sistema global inteiro de forma coerente, simulando o colapso não-local da função de onda.

---

### Conclusão

A abordagem de **valores virtuais e adaptadores** em Haskell não apenas resolve o problema técnico de acessar e operar sobre partes de estruturas emaranhadas, mas também revela uma poderosa metáfora de como a **localidade operacional e a globalidade física** coexistem na mecânica quântica. Essa arquitetura, embora inspirada em padrões de software clássicos, é especialmente eficaz para respeitar a **dualidade onda/partícula**, proporcionando um modelo funcional e seguro para computações quânticas complexas.

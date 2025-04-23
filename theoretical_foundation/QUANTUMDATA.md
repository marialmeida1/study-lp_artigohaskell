**Tipos de Dados Quânticos em Haskell: Como Haskell Modela Qubits**

A base da computação quântica está no conceito de **qubit** (quantum bit), que é a unidade fundamental de informação em sistemas quânticos. Diferente dos bits clássicos, que assumem apenas os valores 0 ou 1, um qubit pode estar em uma **superposição** de ambos os estados ao mesmo tempo. Isso significa que ele pode representar algo como "um pouco 0 e um pouco 1", com pesos diferentes (chamados de amplitudes de probabilidade).

Essas amplitudes são **números complexos**, e o quadrado de seus módulos representa a probabilidade de medir aquele estado. Por exemplo, se um qubit está no estado:

    α|False⟩ + β|True⟩

então, ao ser medido, ele será False com probabilidade |a|^2 e True com probabilidade |b|^2.

---

### Representação em Haskell

Haskell, por ser uma linguagem funcional fortemente tipada, permite representar qubits de forma abstrata e segura. Para isso, o artigo define:

```haskell
type PA = Complex Double  -- Probabilidade como número complexo
type QV a = FiniteMap a PA -- Um qubit é um mapeamento de construtores para amplitudes
```

Aqui, `QV a` é uma estrutura que representa uma superposição de todos os possíveis valores de um tipo `a`. Esse tipo `a` pode ser `Bool`, `Color`, `Move`, entre outros tipos enumerados. 

#### Exemplo com Bool (como em um qubit clássico):

```haskell
data Bool = False | True

qFalse :: QV Bool
qFalse = unitFM False 1

qFT :: QV Bool
qFT = qv [(False, 1 / sqrt 2), (True, 1 / sqrt 2)]
```

Neste exemplo:
- `qFalse` representa um qubit que está 100% no estado False.
- `qFT` representa um qubit em **superposição perfeita** entre False e True.

Haskell usa **listas e mapas** para manter essas representações explícitas e bem definidas.

---

### Generalização para Outros Tipos

Haskell permite aplicar a ideia de qubits não apenas a booleanos, mas a qualquer tipo com valores distintos. Para isso, define-se a **classe de tipos** `Basis`:

```haskell
class (Eq a, Ord a) => Basis a where
  basis :: [a]
```

Isso quer dizer que, para que um tipo `a` possa ser usado em um qubit (`QV a`), ele deve ter uma lista finita e ordenada de todos os seus valores possíveis. Exemplos:

```haskell
data Move = Vertical | Horizontal
instance Basis Move where
  basis = [Vertical, Horizontal]
```

Isso permite criar qubits como:
```haskell
qUp :: QV Move
qUp = unitFM Vertical 1
```

---

### Por que usar Mapas em vez de Funções?

Uma alternativa seria usar uma função para mapear construtores para amplitudes. No entanto, isso é **ineficiente**, pois cada chamada recalcula o valor e a composição de várias funções geraria uma **explosão exponencial** de cálculos. Por isso, o artigo opta por usar `FiniteMap` (um tipo de dicionário ordenado), que garante eficiência na busca e manipulação.

---

### Relação com as Características de Haskell

Essa modelagem de tipos quânticos em Haskell é possível e elegante graças a:

- **Tipos algébricos**: permitem definir tipos com construtores distintos (como `Bool`, `Move`, etc.).
- **Polimorfismo paramétrico**: `QV a` é genérico para qualquer tipo `a`.
- **Type classes**: `Basis` fornece uma interface comum para tipos que podem ser usados como qubits.
- **Imutabilidade**: cada qubit tem um estado fixo até que seja observada/modificada explicitamente (veremos isso em seções futuras do artigo).

---

### Conclusão da Seção

Haskell oferece uma maneira natural e segura de representar qubits e suas superposições. Ao usar abstrações como tipos genéricos, mapas finitos e classes de tipos, o autor consegue traduzir os princípios da mecânica quântica em estruturas computacionais funcionais. Além de educativo e introdutório essa representação também serve como base para simular algoritmos quânticos de forma precisa dentro do paradigma funcional.


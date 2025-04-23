# Tipos de Dados Quânticos e Haskell

## Representação de Qubits com Tipos de Dados

Em Haskell, os qubits (unidade básica da computação quântica) são representados usando o tipo `QV a`, que mapeia cada valor de um tipo base `a` para sua respectiva amplitude de probabilidade (um número complexo).

```haskell
type PA = Complex Double
type QV a = FiniteMap a PA
```

Por exemplo:
```haskell
qFalse = unitFM False 1
qTrue = unitFM True 1
qFT = qv [(False, 1/√2), (True, 1/√2)]
```

- `qFalse` representa um qubit que sempre retorna `False`.
- `qTrue` sempre retorna `True`.
- `qFT` representa um qubit em **superposição**, com 50% de chance de ser `False` e 50% de ser `True`.

As amplitudes de probabilidade (α, β) determinam a probabilidade de medição dos estados clássicos, sendo proporcional ao quadrado de seus módulos: `|α|²` e `|β|²`.

## Tipos Enumerados e Generalização

O modelo apresentado no artigo generaliza a construção de qubits para **qualquer tipo enumerado**, não apenas booleanos. Para isso, os tipos precisam implementar a classe `Basis`:

```haskell
class (Eq a, Ord a) ⇒ Basis a where
  basis :: [a]
```

Isso permite listar os **vetores unitários** associados a cada tipo. Por exemplo:

```haskell
data Move = Vertical | Horizontal
data Rotation = CtrClockwise | Clockwise
data Color = Red | Yellow | Blue

instance Basis Move where basis = [Vertical, Horizontal]
instance Basis Rotation where basis = [CtrClockwise, Clockwise]
instance Basis Color where basis = [Red, Yellow, Blue]
```

Cada valor de `QV a` é então um **mapeamento explícito** entre os construtores do tipo `a` e suas amplitudes de probabilidade. A representação explícita via `FiniteMap` é preferida sobre funções anônimas, pois estas introduzem grande penalidade de desempenho.

```haskell
qv :: Basis a ⇒ [(a, PA)] → QV a
qv = listToFM
```

## Vantagens da Abordagem com Tipos

- Permite **modelagem segura e expressiva** de valores quânticos.
- Facilita a **abstração** e a **generalização** para diferentes domínios (ex: cores, movimentos, rotações).
- Usa os recursos do sistema de tipos de Haskell para garantir que os dados quânticos estejam sempre bem formados.

## Tipos Infinitos

Em teoria, o modelo também suporta tipos infinitos, como inteiros (`Integer`):

```haskell
instance Basis Integer where basis = [0..]
qi = qv [ (i, 1 / i) | i ← basis, i ≠ 0]
```

Contudo, esses casos não são usados no restante do artigo, pois operações sobre eles exigiriam manipulação de séries convergentes — o que vai além do escopo do modelo proposto.

---

## Conclusão

A modelagem de tipos de dados quânticos em Haskell demonstra a força da linguagem funcional para representar conceitos como **superposição** e **entrelaçamento** de forma clara, segura e eficiente. A utilização de tipos enumerados e estruturas como `QV` permite construir representações quânticas generalizadas com alto grau de expressividade e controle.
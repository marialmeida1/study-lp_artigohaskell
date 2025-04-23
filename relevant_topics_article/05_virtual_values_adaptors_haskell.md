# Valores Virtuais e Adaptadores para Entrelaçamento

## Manipulação de Dados Quânticos Entrelaçados em Haskell

Na computação quântica modelada em Haskell, estruturas compostas frequentemente contêm qubits entrelaçados que não podem ser separados diretamente. Para possibilitar a manipulação funcional desses dados, o artigo propõe dois conceitos principais: **valores virtuais (virtual values)** e **adaptadores (adaptors)**.

Essas abstrações permitem acessar e operar sobre componentes individuais de uma estrutura entrelaçada de forma segura, preservando o entrelaçamento e a modularidade funcional do código.

---

## Adaptadores: Acesso Modular a Subestruturas

Um **adaptador** fornece uma forma de mapear entre uma estrutura global e uma parte específica dela. Ele atua como uma espécie de ponte entre a visão total e parcial dos dados, e é definido por duas funções:

```haskell
data Adaptor l g = Adaptor { dec :: g -> l, cmp :: l -> g }
```

- `dec`: extrai a subestrutura desejada `l` da estrutura global `g`.
- `cmp`: recompõe a estrutura global `g` a partir da subestrutura `l`.

Adaptadores são especialmente úteis quando os componentes desejados estão aninhados profundamente dentro de estruturas compostas e entrelaçadas. Eles permitem aplicar operações quânticas locais sem modificar diretamente o restante da estrutura.

---

## Valores Virtuais: Componentes Operáveis de Forma Isolada

Um **valor virtual** representa um componente específico de uma estrutura entrelaçada, permitindo que esse componente seja operado como se fosse um valor independente. Ele encapsula uma referência ao valor completo e o adaptador que define como acessar a parte desejada:

```haskell
data Virt a na u = Virt (QR u) (Adaptor (a, na) u)
```

- `a`: o valor quântico principal que se deseja manipular.
- `na`: os vizinhos entrelaçados (`entangled neighbors`) que compartilham dependência com `a`.
- `u`: o tipo da estrutura completa.

Isso permite isolar e operar sobre qualquer parte de uma estrutura quântica, mesmo que seus componentes estejam profundamente aninhados.

### Exemplo

Suponha uma estrutura do tipo:

```haskell
QV (((a, b, c), (d, e)), (f, g))
```

É possível isolar o par `(d, g)` por meio de diferentes agrupamentos usando adaptadores apropriados. Duas formas válidas são:

```haskell
mkVirt1 :: QR (((a,b,c),(d,e)),(f,g)) -> Virt (d,g) (a,b,c,e,f) (((a,b,c),(d,e)),(f,g))
mkVirt2 :: QR (((a,b,c),(d,e)),(f,g)) -> Virt (d,g) ((a,b,c),e,f) (((a,b,c),(d,e)),(f,g))
```

Cada uma dessas construções define maneiras diferentes de considerar os vizinhos entrelaçados, mas ambas permitem tratar `(d, g)` como um valor virtual manipulável.

---

## Integração com Estruturas Complexas

O uso de valores virtuais e adaptadores em Haskell oferece uma maneira poderosa e modular de manipular dados entrelaçados. Esse mecanismo evita a necessidade de “desentrelaçar” estruturas, respeitando a integridade quântica e o paradigma funcional da linguagem.

Esse padrão é inspirado no **Facade Pattern**, oferecendo uma interface simples para acessar partes complexas de uma estrutura aninhada.

---

## Aplicações e Operações

### Promoção de Operações Quânticas

As operações quânticas (`Qop a b`) podem ser aplicadas a valores virtuais usando as funções `app` e `app1`, que asseguram que os entrelaçamentos sejam respeitados:

```haskell
app  :: Qop a b -> Virt a na ua -> Virt b na ub -> IO ()
app1 :: Qop a a -> Virt a na ua -> IO ()
```

### Observação de Valores Virtuais

A medição de valores virtuais é feita por meio de `observeVV`, que calcula a distribuição de probabilidade localmente sobre o componente observado e colapsa a estrutura de forma consistente com a observação:

```haskell
observeVV :: Virt a na u -> IO a
```

---

## Uniformização: Tudo é um Valor Virtual

Para padronizar a manipulação de valores quânticos, todo valor é tratado como virtual. Isso inclui:
- Referências simples (`virtFromR`)
- Subcomponentes compostos (`virtFromV`)

Essas funções permitem construir valores virtuais a partir de referências diretas ou de outros valores virtuais, compondo adaptadores de forma flexível:

```haskell
virtFromR :: QR a -> Virt a () a
virtFromV :: Virt a na u -> Adaptor (a1, a2) a -> Virt a1 (a2, na) u
```

---

## Conclusão

A introdução dos **valores virtuais** e **adaptadores** no modelo de computação quântica em Haskell fornece uma solução elegante para lidar com estruturas entrelaçadas. Essa abordagem mantém a imutabilidade, tipagem forte e modularidade características da linguagem, ao mesmo tempo em que permite representar e manipular fenômenos quânticos complexos. Dessa forma, torna-se possível implementar algoritmos quânticos de maneira expressiva, segura e estruturada.
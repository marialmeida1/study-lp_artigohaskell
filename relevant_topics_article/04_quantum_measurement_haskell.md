# Medição e Observação em Haskell

## Implementação de Observação de Dados Quânticos

Na computação quântica, a **medição (ou observação)** de qubits é um processo essencial: ela colapsa a superposição de estados em um único valor observável. Em Haskell, esse processo é modelado de forma operacional usando **referências mutáveis** (`IORef`) para armazenar e atualizar o estado dos qubits após a observação.

A observação é feita com a função `observeR`, que:

1. Lê o valor quântico da referência.
2. Normaliza o valor (garantindo que as probabilidades somem 1).
3. Gera um número aleatório entre 0 e 1.
4. Com base nas probabilidades acumuladas, seleciona um dos estados possíveis.
5. Atualiza a referência para que, após a observação, o qubit esteja em estado puro (colapsado).

```haskell
observeR :: Basis a ⇒ QR a → IO a
```

Essa abordagem garante que, após a primeira observação, qualquer leitura subsequente do mesmo valor resultará no mesmo resultado, simulando o colapso da função de onda como esperado na mecânica quântica.

## Desafios de Compreensão com Estados e Efeitos Colaterais

Apesar da linguagem Haskell ser funcional e pura por padrão, a simulação da observação exige **efeitos colaterais globais**, pois a observação de um qubit pode influenciar automaticamente outros com os quais ele esteja entrelaçado (**entangled**).

Para isso, o artigo utiliza um mecanismo de **efeito colateral global via referência compartilhada**, o que permite que todos os componentes de um sistema entrelaçado sejam atualizados de forma coerente e instantânea após a medição de um deles. Isso é uma aproximação prática, embora não totalmente compatível com os princípios físicos — mas aceitável dentro de um **ambiente de execução sequencial** (sem múltiplas threads concorrentes).

Exemplo: ao medir um par quântico entrelaçado, como `QV (a, b)`, a função `observeLeft` permite medir apenas o componente da esquerda. Essa medição afeta automaticamente o componente da direita ao atualizar a estrutura de dados com um novo valor colapsado e normalizado, garantindo consistência do sistema.

```haskell
observeLeft :: (Basis a, Basis b) ⇒ QR (a, b) → IO a
```

Este tipo de modelagem exige cuidado especial, pois o uso de efeitos colaterais pode introduzir **complexidade conceitual** em uma linguagem funcional que, por natureza, evita mutações de estado.

## Conclusão

Embora a medição em sistemas quânticos seja naturalmente não-determinística e com efeitos colaterais, Haskell consegue modelar esse comportamento de maneira **compatível com sua estrutura funcional**, utilizando **referências mutáveis controladas** para representar o colapso do estado quântico de forma clara e operacional.
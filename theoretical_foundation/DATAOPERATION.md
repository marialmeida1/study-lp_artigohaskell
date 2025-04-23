# 3. Operações sobre Dados Quânticos em Haskell
## Definindo Operações com Haskell
Em Haskell, operações quânticas fundamentais, como o Hadamard gate ou operações de controle (como CNOT), podem ser modeladas de forma simples e elegante. O Haskell se destaca aqui por ser uma linguagem funcional, o que significa que tudo é feito por meio de funções que não alteram o estado do programa (funções puras).

- **Funções Puras:** Isso é importante porque, na computação quântica, os estados dos qubits são manipulados sem que o processo altere de forma permanente o estado do sistema até a medição. Assim como em Haskell, as operações quânticas podem ser tratadas como funções puras, ou seja, funções que recebem dados de entrada e retornam um resultado sem modificar nada fora delas. Isso ajuda a garantir que as operações sejam previsíveis e fáceis de controlar.

- **Exemplo de Operação:** Imagine uma função em Haskell que represente o Hadamard gate. Essa função pegaria um qubit e o transformaria em uma superposição de estados, representando de forma pura a operação quântica.

## Composição de Funções para Quantum Gates
Uma das maiores vantagens de Haskell no contexto de computação quântica é sua capacidade de criar operações compostas e reutilizáveis de maneira modular.

**Composição de Funções:** Haskell permite que você componha várias funções menores para formar uma operação maior. Por exemplo, se você quiser realizar uma sequência de operações quânticas em um qubit, pode definir cada operação como uma função e combiná-las de forma modular. Isso é muito semelhante à forma como circuitos quânticos funcionam, onde você conecta várias portas lógicas (como Hadamard, CNOT, etc.) para formar um circuito complexo.

**Exemplo de Modularidade:** Suponha que você tenha funções que representam diferentes operações, como hadamard, cnot (controle negado), e você precise de uma função que as combine para realizar uma tarefa específica. Haskell permite que você combine essas funções em uma nova função, sem precisar duplicar o código ou complicar o processo, refletindo a natureza modular dos circuitos quânticos.

Assim, Haskell, com sua abordagem funcional e de composição de funções, oferece uma maneira eficiente e clara de representar as operações quânticas, tornando o código mais limpo, reutilizável e fácil de entender para quem está começando.

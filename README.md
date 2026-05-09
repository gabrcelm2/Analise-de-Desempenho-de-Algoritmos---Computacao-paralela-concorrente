Este repositório contém uma suíte de testes (benchmark) desenvolvida em Java para analisar e comparar o desempenho computacional de algoritmos de ordenação em ambientes de processamento serial e concorrente.

## Objetivo
Demonstrar na prática as diferenças de complexidade assintótica - O(n2) e O(n log n) - e avaliar a viabilidade, os ganhos (Speedup) e os gargalos (Lei de Amdahl) da paralelização de algoritmos.

## Estrutura do Código e Lógica de Implementação

O projeto foi construído utilizando os princípios de Orientação a Objetos, garantindo modularidade. Cada algoritmo foi implementado em sua própria classe estática.

## 1. Algoritmos Quadráticos
* **Algoritmos:** `BubbleSort.java` e `InsertionSort.java`
* **Lógica:** Operam baseados na transposição e comparação de elementos vizinhos.
* **Por que não foram paralelizados?** Devido à altíssima dependência de dados adjacentes na memória. Tentar paralelizar esses métodos exigiria o uso excessivo de travas (*Locks*), gerando um custo (*overhead*) que corromperia o ganho de desempenho. Eles atuam como o pior cenário do benchmark.

### 2. Algoritmos Logarítmicos
* **Algoritmos:** `MergeSort.java` / `ParallelMergeSort.java` e `QuickSort.java` / `ParallelQuickSort.java`
* **Lógica:** Utilizam o paradigma de Divisão e Conquista. O array é quebrado em partes menores independentes, permitindo que múltiplas *threads* trabalhem ao mesmo tempo sem conflito de memória.
* **Como foram paralelizados?** Utilizando a moderna API de concorrência do Java: **`ForkJoinPool`** e a classe abstrata **`RecursiveAction`**. 

### 3. O Framework de Benchmark
A classe principal orquestra a execução dos testes variando os seguintes parâmetros de forma autônoma:
* **Tamanhos do Array:** 10.000, 50.000, 100.000 e 500.000 elementos.
* **Natureza dos Dados:** Aleatório, Ordenado e Inversamente Ordenado.
* **Núcleos de Processamento:** 1 *Thread* (Serial), 2, 4 e 8 *Threads* (Paralelo).
* **Amostragem:** 5 execuções sequenciais para cada configuração.

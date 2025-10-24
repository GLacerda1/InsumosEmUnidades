## Estruturas e Algoritmos Utilizados

Para o controle de estoque com Programação Dinâmica, foram implementadas duas abordagens principais e algumas estruturas auxiliares. Cada uma contribuiu para a solução do problema real das unidades de diagnóstico:

### 1. Programação Dinâmica Recursiva (Top-Down)
- **Como foi usada:** implementada como uma função recursiva que calcula o custo ótimo a partir do estoque atual e do dia atual.  
- **Contexto do problema:** permite modelar diretamente a equação de Bellman, considerando a demanda estocástica e os custos de pedido, armazenagem e falta.  
- **Benefício:** fácil de entender e validar; cada subproblema é resolvido de forma intuitiva.

### 2. Memorização (`@lru_cache`)
- **Como foi usada:** armazenou os resultados de subproblemas já resolvidos na versão recursiva.  
- **Contexto do problema:** evita recalcular o custo de estados/ações já analisados, acelerando significativamente a execução.  
- **Benefício:** mantém a implementação recursiva eficiente mesmo para horizontes maiores.

### 3. Programação Dinâmica Iterativa (Bottom-Up)
- **Como foi usada:** constrói uma tabela de valores de estoque para cada dia, calculando o custo mínimo de forma iterativa.  
- **Contexto do problema:** fornece os mesmos resultados da versão recursiva de forma mais eficiente.  
- **Benefício:** ideal para implementação prática e simulações em grande escala, sem risco de estouro de pilha.

### 4. Loops sobre ações e estados
- **Como foi usada:** para cada estado de estoque, testamos todas as quantidades de pedido possíveis.  
- **Contexto do problema:** permite identificar a **política ótima** de reposição para cada nível de estoque e dia.  
- **Benefício:** garante que o algoritmo explore todas as possibilidades e encontre a ação de menor custo.

### 5. Simulação Monte Carlo
- **Como foi usada:** aplicamos a política ótima em múltiplos cenários de demanda aleatória.  
- **Contexto do problema:** valida empiricamente o comportamento do sistema sob incerteza, estimando o custo médio esperado.  
- **Benefício:** permite avaliar robustez e risco operacional da política de estoque.

### 6. Uso de `numpy`
- **Como foi usada:** para armazenar tabelas de valores e calcular custos médios de forma vetorizada.  
- **Contexto do problema:** melhora desempenho em operações numéricas grandes, facilitando manipulação de arrays de estoque e ações.  
- **Benefício:** otimiza execução, principalmente na versão iterativa e na simulação Monte Carlo.

### Resultado
- Ambas as abordagens (recursiva e iterativa) **produzem a mesma política ótima**, garantindo consistência entre teoria e prática.  
- Cada estrutura foi escolhida para **equilibrar clareza, performance e robustez** no contexto de controle de estoques.


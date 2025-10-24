# Controle de Estoque com Programação Dinâmica

Este projeto aplica **Programação Dinâmica (PD)** para definir uma **política ótima de reposição de estoque** em unidades de diagnóstico médico, onde o consumo diário de insumos (reagentes e descartáveis) é incerto e pouco registrado.  

O objetivo é **minimizar o custo total esperado**, equilibrando custos de compra, armazenagem, pedidos e faltas, ao longo de um horizonte finito de tempo (ex: 30 dias).

---

## Estrutura do Modelo

| Elemento | Descrição |
|-----------|------------|
| **Estado (sₜ)** | Nível de estoque disponível no início do dia |
| **Ação (aₜ)** | Quantidade pedida (reposição) no início do dia |
| **Demanda (Dₜ)** | Consumo diário incerto, modelado como variável aleatória |
| **Transição de estado** | O estoque do próximo dia depende de `sₜ + aₜ - Dₜ` |
| **Custos considerados** | Pedido fixo (K), custo unitário (c), armazenagem (h), falta (p) |

**Objetivo:** encontrar a política de reposição que **minimiza o custo total esperado**, equilibrando **rupturas** (falta de insumos) e **desperdício** (estoque excessivo).

---

## Algoritmos Implementados

### 1. Programação Dinâmica Recursiva (Top-Down)

**Descrição:**  
Implementa a equação de Bellman de forma direta e intuitiva.  
A cada chamada, o algoritmo considera o custo imediato (pedido, armazenagem e falta) e o custo futuro esperado.  

**Como funciona:**
1. Para cada dia e nível de estoque, testa todas as possíveis quantidades a pedir (`aₜ`);
2. Calcula o custo total esperado de cada ação, considerando a distribuição da demanda;
3. Escolhe a ação que gera o **menor custo esperado**;
4. Armazena o resultado usando **memorização (`@lru_cache`)** para evitar recomputações.

**Aplicação no problema:**  
Essa abordagem é útil para **entender e validar o modelo teórico** — reproduz fielmente a formulação matemática da decisão ótima.

---

### 2. Programação Dinâmica Iterativa (Bottom-Up)

**Descrição:**  
Implementa a mesma lógica, mas de forma iterativa — calculando os custos **de trás para frente no tempo**.  
Ao invés de recursão, constrói uma **tabela `V[t][s]`** que guarda o custo mínimo esperado para cada estado (dia e estoque).

**Como funciona:**
1. Inicializa o custo no último dia como zero (sem horizonte futuro);
2. Para cada dia `t` (em ordem reversa) e nível de estoque `s`:
   - Avalia todas as decisões possíveis (`aₜ`);
   - Calcula o custo médio esperado após a demanda;
   - Atualiza `V[t][s]` com o menor custo encontrado.
3. Retorna a **política ótima** e o **custo total mínimo**.

**Aplicação no problema:**  
A versão iterativa é **mais eficiente e escalável**, ideal para cenários reais (como 30+ dias e dezenas de níveis de estoque).

---

### 3. Validação da Equivalência

Para garantir a consistência, o código compara os resultados das duas abordagens:
- Máxima diferença entre V_rec e V_iter: 0.0000000...
- Desacordos na política: 0


As duas versões produzem **políticas idênticas** e **custos iguais**, confirmando a correção matemática do modelo.

---

## Interpretação dos Resultados

- Aumentar **p (custo de falta)** → reduz rupturas, eleva estoques de segurança.  
- Aumentar **h (custo de armazenagem)** → reduz estoques, evita desperdícios.  
- Aumentar **K (custo fixo de pedido)** → reduz frequência de pedidos.  

Esses parâmetros permitem **personalizar o comportamento do sistema** conforme o perfil de cada unidade de diagnóstico.

---

## Tecnologias Utilizadas

- **NumPy** (operações vetorizadas e tabelas de custos)  
- **functools.lru_cache** (otimização da recursão)  
- **Matplotlib** (visualização dos resultados e políticas)  

---

## Conclusão
  
O modelo desenvolvido possibilita:

- Melhor **visibilidade do consumo real**;  
- **Redução de custos** com falta e excesso de insumos;  
- **Automatização da decisão de reposição** de forma ótima e previsível.  

O método é flexível e pode ser ajustado conforme os custos, políticas e perfis de consumo de cada laboratório ou unidade hospitalar.




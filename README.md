# 📊 Análise de Anomalias no Tempo de Entrega com SQL e Z-Score

Este projeto apresenta um script SQL para a detecção de anomalias (outliers) no tempo de entrega de pedidos. Utilizando a técnica estatística do **Z-score**, o código identifica entregas que estão significativamente acima ou abaixo da média, permitindo ações proativas para melhorar a operação logística e a satisfação do cliente.

---

## 🎯 O Problema de Negócio

Analisar apenas a **média** do tempo de entrega pode mascarar problemas críticos. Uma média geral "boa" pode esconder um pequeno número de entregas extremamente atrasadas que geram alta insatisfação, ou entregas muito rápidas que poderiam revelar eficiências no processo.

Este script visa expor esses extremos, respondendo a perguntas como:
- Quais foram as entregas mais problemáticas que precisam de investigação?
- Quais foram as entregas excepcionalmente rápidas e o que podemos aprender com elas?

---

## 🛠️ A Solução Técnica: Z-Score em SQL

O **Z-score** é uma medida estatística que nos diz quantos desvios padrão um ponto de dado está distante da média do conjunto. É uma "régua" universal para medir o quão "normal" ou "excepcional" é cada entrega, independentemente do valor absoluto.

O script implementa o cálculo da seguinte forma:

1.  **Cálculo das Estatísticas Globais:** Primeiro, usamos uma CTE (Common Table Expression) chamada `ESTATISTICAS` para calcular a média (`AVG`) e o desvio padrão (`STDEV`) de todo o histórico de tempo de entrega.

2.  **Aplicação a Cada Pedido:** Em seguida, utilizamos um `CROSS JOIN` para disponibilizar esses dois valores (média e desvio) em cada uma das linhas da tabela `PEDIDOS`. Esta é uma técnica eficiente para aplicar uma estatística global a cada registro individual.

3.  **Cálculo do Z-Score:** Finalmente, aplicamos a fórmula do Z-score `(valor - média) / desvio` para cada pedido. O uso de `NULLIF(E.DESVIO, 0)` é uma salvaguarda para evitar erros de divisão por zero caso o desvio padrão seja nulo ou zero.

---

## 🚀 Análise dos Resultados e Ações de Negócio

A interpretação do Z-score permite gerar insights acionáveis:

* **Z-Score Alto e Positivo (ex: > 2 ou 3):** Indica que a entrega demorou muito mais que a média. Estes são os principais candidatos para investigação.
    * **Ação:** Analisar a rota, a transportadora ou o processo de separação desses pedidos para identificar gargalos. Entrar em contato proativamente com o cliente para mitigar a insatisfação.

* **Z-Score Alto e Negativo (ex: < -2 ou -3):** Indica que a entrega foi excepcionalmente rápida.
    * **Ação:** Estudar esses casos de sucesso. Foi uma rota mais eficiente? Um processo de empacotamento mais rápido? Essas "anomalias positivas" podem se tornar o novo padrão de eficiência.

* **Z-Score Próximo de 0:** Indica entregas dentro do comportamento esperado.

---

## ⚙️ Como Utilizar

1.  **Pré-requisitos:** Acesso a um banco de dados SQL (SQL Server, PostgreSQL, etc.) e uma tabela de `PEDIDOS`.
2.  **Estrutura da Tabela:** A tabela `PEDIDOS` deve conter, no mínimo, as colunas: `ID_PEDIDO`, `CLIENTE`, `TEMPO_ENTREGA` (em dias ou horas) e `DATA_ENTREGA`.
3.  **Execução:** Adapte o nome da tabela (se necessário) e execute o script em seu cliente SQL de preferência.

---

### 📫 Contato

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/gustavo-barbosa-868976-236/) [![Email](https://img.shields.io/badge/Email-gustavobarbosa7744@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:gustavobarbosa7744@gmail.com)

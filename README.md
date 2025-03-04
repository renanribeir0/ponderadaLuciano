# **Modelagem de Dados: Oficina de Mecânica**

## **Objetivo**

Este repositório tem como objetivo apresentar a modelagem de dados de um sistema de oficina mecânica com foco em duas relações importantes: **1:N (um-para-muitos)** e **N:N (muitos-para-muitos)**. A modelagem abrange três entidades principais: **Mecânicos**, **Carros** e **Peças**, além de uma tabela de junção entre **Carros** e **Peças**.

## **Relações no Modelo**

### **Relação 1:N (Mecânicos e Carros)**

- **Entidades envolvidas**: **Mecanicos** e **Carros**.
- **Descrição**: Um mecânico pode consertar vários carros, mas cada carro é consertado por apenas um mecânico. Essa é uma relação **um-para-muitos**.
- **Chave Estrangeira**: A tabela **Carros** possui uma chave estrangeira **MecanicoID**, que se refere ao **ID** da tabela **Mecanicos**.

### **Relação N:N (Carros e Peças)**

- **Entidades envolvidas**: **Carros** e **Peças**.
- **Descrição**: Um carro pode ter várias peças, e uma peça pode ser utilizada em vários carros. Essa é uma relação **muitos-para-muitos**, e é representada pela tabela de junção **Carros_Pecas**.
- **Tabela de Junção**: **Carros_Pecas**, que mapeia a relação entre os **IDs** de **Carros** e **Peças**, além de incluir o atributo **Quantidade** para representar a quantidade de peças utilizadas em cada carro.

## **Estrutura do Banco de Dados**

![image](https://github.com/user-attachments/assets/0bc2ab34-de0e-4e3c-a546-3ff85354c04b)

![image](https://github.com/user-attachments/assets/6cf7f7cd-8002-4b80-a25a-6a9b46d6e86f)

![image](https://github.com/user-attachments/assets/7110497f-1ad0-4e92-a07d-440f40de25d7)

### **Tabelas**

1. **Mecanicos**
   - **ID**: Identificador único do mecânico (inteiro, chave primária).
   - **Nome**: Nome do mecânico (varchar).
   - **Nivel**: Nível do mecânico (varchar), com valores como "Júnior", "Pleno", "Sênior".

2. **Carros**
   - **ID**: Identificador único do carro (inteiro, chave primária).
   - **Modelo**: Modelo do carro (varchar).
   - **Categoria**: Categoria do carro (varchar), com valores como "Compacto", "Sedan", etc.
   - **MecanicoID**: Referência ao mecânico responsável pelo conserto do carro (inteiro, chave estrangeira).

3. **Pecas**
   - **ID**: Identificador único da peça (inteiro, chave primária).
   - **Nome**: Nome da peça (varchar).
   - **Preco**: Preço da peça (decimal).

4. **Carros_Pecas**
   - **CarroID**: Identificador do carro (inteiro, chave estrangeira).
   - **PecaID**: Identificador da peça (inteiro, chave estrangeira).
   - **Quantidade**: Quantidade de peças usadas no carro (inteiro).

## **Exemplo de Consulta SQL**

![image](https://github.com/user-attachments/assets/45aa2fd2-0a74-4ff3-93b3-f71677728419)

A consulta SQL proposta tem como objetivo listar quantas peças os mecânicos de nível **"Júnior"** consertaram em carros da categoria **"Compacto"**, onde o preço das peças estava entre **100 e 200**.

```sql
SELECT m.Nome AS Mecanico, COUNT(cp.PecaID) AS Total_Pecas
FROM Mecanicos m
JOIN Carros c ON m.ID = c.MecanicoID
JOIN Carros_Pecas cp ON c.ID = cp.CarroID
JOIN Pecas p ON cp.PecaID = p.ID
WHERE m.Nivel = 'Júnior'
AND c.Categoria = 'Compacto'
AND p.Preco BETWEEN 100 AND 200
GROUP BY m.Nome;
```

### **Explicação da Consulta:**
- **JOIN** entre as tabelas **Mecanicos**, **Carros**, **Carros_Pecas** e **Pecas**.
- **WHERE** filtra os mecânicos de nível **"Júnior"**, carros da categoria **"Compacto"** e peças com preço entre **100 e 200**.
- **COUNT** conta o número de peças utilizadas e **GROUP BY** agrupa os resultados pelo nome do mecânico.

## **Conversão para Álgebra Relacional**

A conversão da consulta SQL para álgebra relacional seria representada da seguinte forma:

```
π Nome, COUNT(PecaID) (σ Nivel = 'Júnior' ∧ Categoria = 'Compacto' ∧ Preco BETWEEN 100 AND 200 (Mecanicos ⨝ Carros ⨝ Carros_Pecas ⨝ Pecas))
```

### **Equação Otimizada de Álgebra Relacional**

A equação otimizada, para melhorar a eficiência da consulta, seria:

```
π Nome, COUNT(PecaID) ( (σ Nivel = 'Júnior' (Mecanicos)) ⨝ (σ Categoria = 'Compacto' (Carros)) ⨝ (σ Preco BETWEEN 100 AND 200 (Pecas)) ⨝ Carros_Pecas)
```

### **Explicação da Equação de Álgebra Relacional**
- **π (Projeção)**: Seleciona os atributos **Nome** e **COUNT(PecaID)**.
- **σ (Seleção)**: Aplica os filtros para **Nivel**, **Categoria** e **Preco**.
- **⨝ (Junção Natural)**: Realiza as junções entre as tabelas relevantes.

## **Execução no Banco de Dados**

Abaixo estão as etapas executadas para criação do banco de dados e das tabelas, além da inserção de dados.

1. **Criação do Banco de Dados**:  
   A criação foi realizada com sucesso, e o banco de dados foi nomeado **Oficina**.

2. **Criação das Tabelas**:  
   Foram criadas as tabelas **Mecanicos**, **Carros**, **Pecas** e **Carros_Pecas**, com as chaves primárias e estrangeiras apropriadas.

3. **Inserção de Dados**:  
   Dados de exemplo foram inseridos nas tabelas **Mecanicos**, **Carros** e **Pecas**, e as relações entre carros e peças foram mapeadas na tabela **Carros_Pecas**.

## **Execução da Consulta e Resultados Esperados**

Quando a consulta SQL é executada, espera-se que o resultado mostre o nome dos mecânicos de nível **Júnior** e o número de peças que eles consertaram em carros da categoria **Compacto**, com preço entre **100 e 200**.


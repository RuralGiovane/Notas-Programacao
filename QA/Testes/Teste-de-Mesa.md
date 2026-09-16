# Teste de Mesa
## ↩ Voltar [[_Testes_|Testes]]

Tags: #QA 

---
# Sumário
- [Teoria](#teoria)
	- [Conceitos Básicos](#conceitos-básicos)
	- [Objetivos](#objetivos)
	- [Vantagens e Desvantagens](#vantagens-e-desvantagens)
- [Exemplo Prático](#exemplo-prático)


---
## Teoria
### Conceitos Básicos
Teste de mesa, também é conhecido como "dry run" ou "walkthroughs", é uma técnica de teste realizada **manualmente**, sem necessidade de executar o código. Essa técnica envolve **simulação de comportamento do software**, percorrendo passo a passo o código e seu fluxo (Input e Output).

---
### Objetivos
O Teste de Mesa possui 2 objetivos:
1. **Encontrar falhas e pontos "cegos" no código:**
Os testes de mesa podem ajudar a identificar falhas lógicas, erros de sintaxe, 
problemas de desempenho e outras falhas que podem ocorrer durante a 
execução do software

2. **Aprimorar a qualidade do código:**
Ao encontrar e corrigir falhas no início do ciclo de desenvolvimento, os testes
de mesa podem ajudar a melhorar a qualidade do código e evitar problemas mais tarde

---
### Vantagens e Desvantagens
#### Vantagens
- **Barato e fácil de realizar**
Os testes de mesa não exigem nenhum equipamento
especial ou software caro.

- **Realizado em qualquer fase do SDLC**
Os testes de mesa podem ser realizados em qualquer
fase do ciclo de desenvolvimento, desde o início até a
fase de testes.

- **Análise mais profunda**
Os testes de mesa permitem que a equipe analise o
código mais profundamente do que seria possível 
com testes automatizados.

- **Aumento do conhecimento técnico**
O processo de realizar testes de mesa exige que os membros da
equipe utilizem seus conhecimentos técnicos para analisar o código e identificar falhas

#### Desvantagens
- **Demorado para executar**
Os testes de mesa podem ser demorados, especialmente 
para sistemas complexos.

- **Subjetividade**
Os resultados dos testes de mesa podem ser subjetivos, pois 
dependem da experiência e do conhecimento da equipe que os
realiza.

- **Baixa eficácia**
Os testes de mesa podem não ser tão eficazes quanto os testes
automatizados para encontrar alguns tipos de falhas.

- **Conhecimento técnico**
Testes de mesa exigem um profissional com profundo 
conhecimento analítico e técnico.

---
## Exemplo prático
Código de exemplo: 

``` 
total = 0

for(x = 0; x < 3; x++){
	total = total + x + 1
}

imprima total;
imprima x
```

| #   | Instrução                                  | Total | X   |
| --- | ------------------------------------------ | ----- | --- |
| 1   | total = 0                                  | 0     |     |
| 2   | for: x = 0                                 | 0     | 0   |
| 3   | total = total + x + 1<br>total = 0 + 0 + 1 | 1     | 0   |
| 4   | for: x++                                   | 1     | 1   |
| 5   | total = total + x + 1<br>total = 1 + 1 + 1 | 3     | 1   |
| 6   | for: x++                                   | 3     | 2   |
| 7   | total = total + x + 1<br>total = 3 + 2 + 1 | 6     | 2   |
| 8   | x++                                        | 6     | 3   |
| 9   | total = 6<br>x = 3                         |       |     |
|     |                                            |       |     |


# Rest Client
## ↩ Voltar [[_Java_|Java]]

Tags: #Java  

---
Perguntas

Qual é a principal vantagem de usar o Spring HTTP Interface Client (baseado em interfaces anotadas) para consumir uma API externa, em vez de escrever manualmente as chamadas com um cliente HTTP tradicional?

- Ele permite declarar a comunicação com a API externa como uma interface Java, deixando para o Spring a responsabilidade de gerar a implementação da chamada HTTP.
--- 
No padrão apresentado em aula, qual é a função de registrar a interface de serviço HTTP na configuração da aplicação (importando-a como serviço)?

- Fazer com que o Spring gere um proxy/implementação da interface e a disponibilize como um componente, pronto para ser usado onde for necessário.
---

Ao mapear a resposta de uma API externa para um objeto que representa apenas alguns dos campos retornados, qual é o principal motivo para configurar esse mapeamento de forma que campos desconhecidos sejam ignorados, em vez de gerar erro?


- Porque a resposta de APIs externas costuma trazer muitos campos que não interessam à aplicação, e o objetivo é modelar apenas o que será usado nas regras de negócio.
---
Por que faz sentido manter a lógica que decide a resposta ao usuário (as regras de negócio baseadas nos dados vindos da API externa) em uma camada separada do componente responsável apenas por buscar esses dados?

- Porque separar essas responsabilidades facilita testar e alterar as regras de negócio sem depender diretamente de como os dados são obtidos da API externa.
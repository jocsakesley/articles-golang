
Olá pessoal, sou o Jocsã e sou desenvolvedor backend. Se quiserem saber um pouco mais sobre a minha experiência podem acessar [essa carta de apresentação](./cover-letter.md) que demonstra um pouco sobre a minha trajetória e/ou acessar o meu [perfil o linkedin](https://www.linkedin.com/in/jocsa-kesley-oliveira/).


# Sobre esta série de artigos
Depois de 3 anos desenvolvendo com golang percebi que eu não tinha a minha trajetória documentada, primeiro por estar focado na entrega dos projetos, e depois por parecer estar bem confortável em um ambiente corporativo com várias ferramentas prontas e funcionais à minha disposição.

Sei que ao longo desse tempo desenvolvi bastante coisa, mas é difícil mensurar a profundidade do conhecimento adquirido em cada entrega, apesar de sempre fazermos as nossas cerimônias de <i>review</i> apontando os aprendizados. O fato é que ter ferramentas ágeis é importante para o fluxo contínuo de entrega, porém é importante também ter o cuidado para não se tornar apenas um montador de peças, sem entender com profundidade a solução que está sendo construída e o problema que está sendo resolvido.

Durante a minha jornada tive a oportunidade de pensar e discutir soluções com a minha equipe, e tenho percebido que não existe uma única resposta certa. O mesmo problema pode ser resolvido de diferentes formas, com diferentes arquiteturas e diferentes tecnologias. No final das contas o que importa é que os requisitos de negócios sejam atendidos com o melhor custo/benefício de acordo com as capacidades do time de tecnologia.

Porém, pensar apenas em custo/benefício sem pensar em qualidade pode se tornar um problema quando falamos em manutenção e código compartilhado. Uma simples alteração pode se tornar um <i>deploy</i> problemático lá na frente.

Por isso que a entrega de um software vai além de apenas conhecer tecnicamente muito bem uma linguagem, é necessário também, além do conhecimento de negócio, a aplicação das boas práticas de desenvolvimento, que abrangem o que já é tão falando em nosso meio: Clean Code com SOLID, Clean Archtecture, TDD, DDD, BDD, Design Patterns e por aí vai. 

Mas note que dominar tudo isso na ponta dos dedos pode ser uma tarefa que pode durar anos de dedicação e aplicação prática, porém a boa notícia é que dá para tirar proveito dos principais conceitos tendo em mente alguns princípios base, que aplicados de forma correta farão com que o próprio software grite para você o que ele precisa.

Dessa forma, proponho uma série de artigos que irão documentar a implementação de um cenário comum no meio corporativo, considerando que nem tudo que está proposto já foi implementado por mim, mas que me farão construir, a pequenos passos, um caminho de descobertas e aprendizados. 

O objetivo desta série de artigos é: 
 - Trazer código e ferramentas aplicadas de maneira prática em pequenos contextos com a devida explicação
 - Desenvolver uma arquitetura robusta em nuvem e de baixo custo para mim.
 - Apresentar e entregar uma solução que integre todos os pequenos contextos apresentados
 - Provisionar todo o ambiente de infraestrutura de forma automatizada.

Para tanto devo considerar as minhas devidas qualificações e limitações (tentei ordenar pelo nível de confiança e prioridade neste projeto específico):
- Proficiente na linguagem escolhida para desenvolvimento (golang)
- Boa experiência com Docker
- Boa experiência com IaC usando Terraform
- Bom conhecimento prático com vários recursos da AWS, porém para um contexto de exposição externa preciso validar e aperfeiçoar a minha capacidade prática de execução
- Conhecimento do funcionamento do Kubernetes, porém como usamos uma camada de abstração desenvolvida pelo pessoal DevOps, muitos detalhes de implementação se tornam transparentes.
- Conhecimento em SQL (Postgresql e Mysql) e NoSQL (DynamoDB e MongoDB)
- Conhecimento do funcionamento das pipelines CI/CD do github, mas nunca criei uma pipeline completa com verificação de qualidade e deploy na AWS
- Conhecimento de frameworks de frontend como ReactJS e AngularJS, apesar de não ser minha atividade principal e nem atuar com frequência nesse tipo de demanda

Obs.: Não tenho a intenção de ensinar as bases da linguagem ou das ferramentas, para isso a documentação é sempre o melhor ponto de partida, mas pretendo expôr um modo de pensar que me ajuda a evoluir um projeto conforme a necessidade.

[<div style="text-align: right">Criando um servidor web</div>](./web-api.md)


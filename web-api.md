# Criando um servidor Web

Quando falamos de boas práticas de desenvolvimento, alguns nomes influentes e suas obras vêm logo em mente como o Ucle Bob, o Martin Flower e o Eric Evans. Porém quando buscamos a implementação prática dos conceitos abordados por eles em outras fontes, acabamos caindo no meio de uma salada de frutas, onde não há um caminho objetivo para ser seguido, mas cada um aplica de forma subjetiva dependendo do contexto e entendimento. Isso não está necessariamente errado, dependendo da complexidade do sistema, não deveríamos ser obrigados a usar tudo, muitas vezes só pra atender as nossas próprias expectativas e mostrar domínio. Na prática isso é valido para demonstrar conhecimento, mas em questões de entrega pode tornar muito complexo um sistema que deveria ser simples. Por outro lado, há conceitos que não deveríamos abrir mão independente do tamanho do projeto, que o torna mais flúido, testável e de fácil evolução.

Para a criação de um servidor web, por exemplo, poderíamos escolher um _framework_, acessar o _quickstart_ da documentação e seguir a vida implementando os nossos _controllers_ e demais lógicas da nossa aplicação. Novamente, isso para um projeto pequeno, atenderia, e ouso arriscar que até mesmo para projetos grandes, porque dificilmente eu precisaria mudar o _framework_ depois de escolhido. Mas existe um princípio de boas práticas que me incomoda cada vez que abro o código e vejo ele violado, em que uma classe não deveria depender diretamente de uma implementação e sim de uma interface que define um contrato que qualquer implementação deve ter. Nesse caso podemos considerar que não deveríamos depender da implementação de qualquer lib externa de _framework_ web, e sim de uma interface que define os métodos de uma nova implementação que faz a adaptação das libs externas. Pode parecer confuso em um primeiro momento, e nem sempre é tão intuitivo de aplicar, mas com esse princípio consigo garantir que o meu sistema não esteja dependente fortemente de uma implementação externa e dar o _check_ em mais alguns dos muitos conceitos de boas práticas de desenvolvimento.

A seguir irei demonstrar como executar isso na prática e os passos que fiz para chegar no resultado alcançado.

## Estrutura do projeto

![image](https://github.com/user-attachments/assets/8405451c-98a0-4041-9107-0a47e34484ee)

Existem muitas formas de estruturar um projeto, desde o padrão MVC até arquiteturas mais elaboradas como a Clean Archtecture, que apresentam mais camadas e suas relações. O que propus na imagem acima é baseado na Clean Archtecture (mas por quanto não todas as camadas), que pode ter o seu fluxo bem exemplificado naquela [imagem com círculos coloridos e concêntricos com setas seguindo uma direção](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). 

Não há um modelo de estrutura de pastas nessa arquitetura, mas sim a aplicação dos conceitos de forma distribuída pelo código que dá a liberdade de organizar da forma que haja uma melhor separação de cada contexto. Já vi a implementação com varias estruturas de pastas e vários nomes diferentes, mas no final das contas o que importa é se o fluxo de relacionamento entre as camadas está sendo seguido.

Na imagem faço a separação inicialmente da camada de infra, controllers e cmd, em que a camada de infra é tudo que se relaciona com o mundo externo, como as libs dos _frameworks_, e o _setup_ das rotas da API, os controllers são responsáveis por receber a requisição, passar os dados para uma camada de usecase (que ainda não será tratada neste artigo) e retornar uma resposta, e a pasta cmd é uma convenção do golang que contém apenas o arquivo que é o _entrypoint_ da aplicação, onde serão inicializados os objetos e dado o _start_ no servidor. Note que na pasta infra, criei uma subpasta http para separar o escopo dos _frameworks_ web, pois podemos ainda ter outros contextos como o _setup_ do banco de dados, e outras dependências externas.

## Entendendo cada arquivo

Em um projeto real poderíamos começar diretamente pela camada de usecase, que é uma camada que entrega valor e é onde estão as regras de negócio, a camada de infra é apenas um meio, com as peças técnicas, para se alcançar o fim desejado. Porém como ainda não tenho um domínio definido para esse projeto, decidi criar todo o ambiente necesário para injetar as regras de negócio, evitando ao máximo qualquer acoplamento entre as camadas. Dessa forma, o que será criado aqui deverá poder ser replicado para outros projetos com diferentes domínios.

Começando pelo arquivo `infra/http/routes/router.go`:

![image](https://github.com/user-attachments/assets/597acf01-e375-44ec-9691-841065b67a13)


Nesse arquivo fazemos o _setup_ das rotas da nossa API recebendo um _server_ como parâmetro, porém ao invés de usar um _framework_ de servidor web diretamente, o `SetupRoutes` recebe uma interface que contém os métodos HTTP necessários para uma rota associada a um controller, e retorna o próprio _server_ que contém o método `Run` para iniciar o servidor. 

Implementei inicialmente um método `Get` que recebe uma rota com um _path parameter_ `:name` e um controller que retorna um "Hello, \<name\>", e um `Post` que recebe também uma rota e um controller que recebe um _body_ e retorna o próprio _body_.

Apesar de ter uma lógica simples nos controllers, com o uso da interface posso garantir qualquer escolha de _framework_ com uma implementação dos métodos da interface. Porém para garantir o comportamento esperado ainda se faz necessário a implementação de testes funcionais, dado que posso implementar a interface, mas desviar o comportamento para fazer algo diferente do proposto. (Os testes funcionais serão tratados em outro artigo.)

No arquivo `infra/http/server/interface.go`:

![image](https://github.com/user-attachments/assets/0d5ba8cd-f2f9-4a7a-86be-7d9551e2d307)

Criei duas interfaces, `IServer` e `IContext`. Para definir os métodos da interface procurei fazer o _tradeoff_ entre o nome ser autoexplicativo com as limitações dos _frameworks_ utilizados.

No `IServer` tem o `Get` e o `Post` que recebem o path e o controller como `interface{}`, que é o tipo genérico do golang, já que para cada _framework_ espera-se um tipo de controller. E o `Run` que recebe a porta e executa o servidor. 

Eu poderia ter continuado com os demais métodos como o `Delete`, o `Put` e o `Patch`, mas eles seguem o mesmo padrão, e na medida que o projeto for evoluindo eles aparecerão naturamente.

O `ÌContext` é uma interface para o contexto do servidor web onde podemos interagir com os metadados da requisição. Normamente o contexto é passado no controller e nele podemos pegar _query_ e _path parameters_, o _body_ e os _headers_ entre outras coisas. Nesse caso o método `Param` espera a chave de um parâmetro definido na rota e retorna o valor passado na URL, o `JSON` espera um _status code_ e um objeto para ser retornado na requisição e o `BodyParser` pega os dados do _body_ e carrega em um objeto para ser manipulado.

Nos arquivos `infra/http/server/fiber/fiberServer.go` e `infra/http/server/gin/fiberGin.go`:

| `fiberServer.go`| `ginServer.go` |
|---|---|
|![image](https://github.com/user-attachments/assets/973f7e00-ce48-43e7-b224-0c61c37838e9)|![image](https://github.com/user-attachments/assets/1364458d-6621-4654-84b3-b98af8c7677f)|

Temos a inicialização dos _frameworks_ fiber e gin com uma `scruct` privada (iniciando com letra minúscula) para cada servidor, e um método de instanciação público (`NewFiberServer` e `NewGinServer`) que inicializa o servidor retornando ele mesmo com os métodos da interface `IServer`.  

| `fiberServer.go`| `ginServer.go` |
|---|---|
|![image](https://github.com/user-attachments/assets/ad74054e-4af5-4f80-98e5-5df54755982e)|![image](https://github.com/user-attachments/assets/4aa5b647-7dc7-43c9-b6a9-ac31226beb0e)|


[Início](./index.md)
<br>
[Criando API Rest](./rest-api.md)

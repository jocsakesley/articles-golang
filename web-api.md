# Criando um servidor Web

Quando falamos de boas práticas de desenvolvimento, alguns nomes influentes e suas obras vêm logo em mente como o Ucle Bob, o Martin Flower e o Eric Evans. Porém quando buscamos a implementação prática dos conceitos abordados por eles em outras fontes acabamos caindo no meio de uma salada de frutas, onde não há um caminho objetivo para ser seguido, mas cada um aplica de forma subjetiva dependendo do contexto e entendimento. Isso não está necessariamente errado, dependendo da complexidade do sistema, não deveríamos ser obrigados a usar tudo, muitas vezes, só pra atender as nossas próprias expectativas, e mostrar o domínio de tudo. Na prática isso é valido para demonstrar conhecimento, mas em questões de entrega pode tornar muito complexo um sistema que deveria ser simples. Por outro lado, há conceitos que não deveríamos abrir mão independente do tamanho do projeto, que o torna mais flúido, testável e de fácil evolução.

Para a criação de um servidor web, por exemplo, poderíamos escolher um _framework_, acessar o _quickstart_ da documentação e seguir a vida implementando os nossos _controllers_ e demais lógicas da nossa aplicação. Novamente, isso para um projeto pequeno, atenderia, e ouso arriscar que até mesmo para projetos grandes, porque dificilmente eu precisaria mudar o _framework_ depois de escolhido. Mas existe um princípio de boas práticas que me incomoda cada vez que abro o código e vejo ele violado, em que uma classe não deveria depender diretamente de uma implementação e sim de uma interface que define um contrato que qualquer implementação deve ter. Nesse caso podemos considerar que não deveríamos depender da implementação de qualquer lib externa de _framework_ web, e sim de uma interface que define os métodos de uma nova implementação que faz a adaptação das libs externas. Pode parecer confuso em um primeiro momento, e nem sempre é tão intuitivo de excutar, mas com esse princípio consigo garantir que o meu sistema não esteja dependente fortemente de uma implementação externa e dar o _check_ em mais alguns dos muitos conceitos de boas práticas.

A seguir irei demonstrar como executar isso na prática e os passos que fiz para chegar no resultado alcançado.

## Estrutura do projeto

![image](https://github.com/user-attachments/assets/8405451c-98a0-4041-9107-0a47e34484ee)

Há muitas formas de estruturar um projeto, desde o padrão MVC até arquiteturas mais elaboaradas como a Clean Archtecture que apresentam mais camadas e as relações entre elas. O que propus na imagem acima é baseado na Clean Archtecture, mas por quanto não todas as camadas, que pode ter o seu fluxo bem exemplificado naquela [imagem com círcuclos coloridos e concêntricos seguindo uma direção](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). Não há um modelo de estrutura de pastas nessa arquitetura, mas sim a aplicação dos conceitos de forma distribuída pelo código que dá a liberdade de organizar da forma que haja uma melhor separação dos contextos. Já vi a implementação com varias estruturas de pastas e vários nomes diferentes, mas no final das contas o que importa é se o fluxo de relacionamento entre as camadas está sendo seguido.

### 



[Início](./index.md)
<br>
[Criando API Rest](./rest-api.md)

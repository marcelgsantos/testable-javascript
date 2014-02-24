# Testable Front-end Javascript - Part 1

* O desenvolvimento front-end vem com um conjunto de desafios que raramente são discutidos em artigos sobre **testes unitários**.
* Conceitos como auto-inicialização, lógica encapsulada, manipuladores de eventos do DOM, requisições assíncronas e callbacks aninhados tornam os testes ainda **mais difíceis**.
* Escrever um código front-end testável é **possível**, mas requer um pouco de **conhecimento** e **pensamento** sobre o problema.
* Alguns **anti-patterns** que dificulta os testes de uma aplicação são:
    * *Javascript inline* - o Javascript incluído no arquivo HTML é impossivel de ser testado por ferramenta de testes unitários externa.
    * *Código inacessível* - mesmo se o Javascript estiver num arquivo externo, se o código não possuir uma interface pública ele é difícil de ser testado.
    * *Falta de construtor de objetos - os testes unitários operam de maneira isolada e individual. Ou seja, testar um singleton é dificil porque os resultados de um teste pode afetar os resultados de outro teste.
    * *Pyramid of doom* - funções callback profundamente aninhadas ocorre com frequência no desenvolvimento Javascript e nos despertam várias preocupações. A lógica dentro da **pyramid of doom** é dificil de ser testada de forma isolada e, ao longo do tempo, tende a se transformar num *código spaghetti* de difícil manutenção.
    * *Manipuladores de eventos mal elaborados* - quando a lógica de manipulação de eventos é misturada com a lógica de submissão de formulários (por exemplo), resulta numa mistura de responsabilidades.
    * *Requisições assíncronas* - as requisições assíncronas reais necessitam de um back-end disponível. Neste caso, depender de um desenvolvimento paralelo de front e backend para testes é inviável.
    * *Lógica assíncrona sem notificação* - é impossível saber quando uma função assíncrona foi completada se não houver algum tipo de notificação.
 * Porém, estes problemas podem ser resolvidos.
    * *Inclua os arquivos Javascript externos*
        * Os arquivos Javascript externos estão prontos para serem reutilizados quando necessário e podem ser usados em mais de um arquivo HTML.
    * *Forneça uma interface pública*
        * O código deve possuir uma **interface pública** para poder ser testado.
        * O design pattern **Module** é o mais utilizado para encapsular a lógica enquanto fornece uma interface pública.
        * O Addy Osmani possui várias literaturas sobre Design Patterns para o Javascript.
        * Evite encapsular toda a lógica dentro de uma função auto-executável.
        * **TODO** Escrever testes usando somente eventos sintéticos é difícil.
        * Um módulo pode ser usado para limitar o acesso a funções, reduzir a poluição no namespace global e fornecer uma interface pública para os testes.
        * O ponto fraco do Module pattern, segundo Addy Osmani, é a incapacidade de se criar testes unitários automatizados para membros privados. Um função que não é diretamente acessível não pode ser diretamente testada.
        * Marcações podem ser feitas no código informando que membros privados foram expostos publicamente para serem testados e que, posteriormente, devem ser retirados para a implantação em produção.
    * *Usar objetos instanciáveis*
        * Nas aplicações que não usam objetos instanciáveis, o código é projetado para rodar somente uma vez. Esta restrição dificulta reiniciar o estado e rodar testes unitários um independente do outro.
        * È mais fácil testar módulos que podem ser instanciados múltiplas vezes. Existem duas abordagens em Javascript que permite isso: funções construtoras e Object.create.
        * Considerando que a estrutura criada no Module pattern trata-se de um objeto, pode-se utilizar Object.create para duplicar este objeto.
        * Pode-se utilizar ainda um método para realizar a inicialização que normalmente é feita por um método construtor.
    * *Diminuir a pyramid of doom*
        * As funções de callback profundamente aninhadas ou **callback hell** são um problema para os desenvolvedores.
        * As funções de callback profundamente aninhadas são consideradas um 'code smell' indicando uma péssima estruturação das funções e indo no sentido contrário da separação de responsabilidades.
        * Quebrar a 'pyramid of doom' em componentes menores resulta num código com funções pequenas, coesivas e fáceis de testar.
    * *Separar manipuladores de eventos DOM da ação que ele realiza*
        * Geralmente utiliza-se um manipulador de evento de submit para processar o evento e realizar a submissão do formulário. Isso mistura as responsabilidades e não permite enviar o formulário de forma programática sem um evento de verdade.
        * Recomenda-se seperar o processamento do evento do que ele realmente realiza. Assim, o comportamento realizado pode ser executado sem a necessidade de se disparar um evento de verdade.
    * *Mock de requisições assíncronas*
        * As requisições assíncronas dependem de uma resposta do back-end. Para o caso de testes de requisições assíncronas recomenda-se a utilização de *mocks*.
        * *Mocks* são objetos que simulam o comportamento de objetos reais de forma controlada.
        * Os mocks são utéis em situações em que os objetos requeridos contém funcionalidades ainda indisponíveis, lentas, que não podem ser controladas entre outras.
        * Testar o front-end e o back-end é importante, porém, pode ser deixado para os *testes funcionais*. Os *testes unitários* devem ser feitos de forma isolada.
        * Um módulo que realiza uma requisição assíncrona deve aceitar um *mock* de um objeto XHR que deve ser injetado através de um construtor ou método init. Desse modo, pode-se utilizar o mock durante os testes e uma implementação real em produção.
    * *Requisição assíncrona necessita de notificações*
        * Um mecanismo de notificação indica quando um processamento foi completado numa aplicação.
        * Existem diversos mecanismos de notificação em Javascript: callbacks, observables, eventos entre outros.
    * *Limpeza de objetos já utilizados*
        * Testes unitários devem rodar de maneira isolada de outros testes unitários. Uma vez que o teste chega ao fim, todos os estados devem ser destruídos incluindo manipuladores de eventos do DOM.
        * Dois testes que manipulam eventos de um mesmo elemento do DOM tem grandes chances de interferirem entre si. Para eliminar esta interferência, um objeto inutilizado deve ter seus manipuladores de eventos removidos.
        * Apesar de parecer o contrário, este trabalho extra vem com uma grande vantagem adicional, os "memory leaks" são reduzidos nas aplicações que destroem seus vários objetos.

# Testable Front-end Javascript - Part 2

    * É importante criar aplicações Javascript que sejam fáceis de ler, de reutilizar e de testas. A refatoração é um grande aliado do desenvolvedor em casos contrários.
    * Algumas boas práticas podem ser seguidas para se ter um código fácil de ler, reutilizar e testar:
        * *Incluir os arquivos Javascript externos*
        * *Fornecer uma interface pública*
        * *Usar objetos instanciáveis*
        * *Diminuir a pyramid of doom*
        * *Separar manipuladores de eventos DOM da ação que ele realiza*
        * *Mock de requisições assíncronas*
        * *Requisição assíncrona necessita de notificações*
        * *Limpeza de objetos já utilizados*
        * *Separar a inicialização da aplicação num módulo próprio*
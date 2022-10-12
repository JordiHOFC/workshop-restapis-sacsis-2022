# Configurando o Banco de Dados e o Hibernate na Aplicação Spring Boot

O Spring Boot, como o nome já diz é um aplicativo de inicialização, ele quem é responsavel por fazer com que os demais frameworks spring trabalhem de maneira cooperativa com minino de configuração possivel.

Em outras palavras antes era necessária escrever muito codigo java e arquivos de configuração para poder iniciar a construção dos sistemas, e isso levava um alto tempo, o que é contra produtivo.
Então o Spring Boot veio para construir essa integração entre os frameworks, e disponibilizar de maneira simples maneiras de personalizar estas configurações. 

As configurações são feitas através de properties (propriedades) dentro do arquivo: **`application.properties`**, que esta localizado dentro de: `\src\main\java\resources\`.

## Configurando o Banco de dados

Para utilizar Banco de dados junto a linguagem de programação Java devemos utilizar um `driver` JDBC referente ao SGBD que iremos utilizar. Para que o driver JDBC estabeleça uma conexão com banco de dados é necessário informar:

- A URL com caminho onde o BD esta disponivel.
- Usuario que fará acesso ao Banco.
- Senha do Usuario que faz acesso.




## Referencias

- [Documentação Oficial Spring Boot](https://spring.io/projects/spring-boot)
- [O que é um Driver JDBC](https://pt.wikipedia.org/wiki/JDBC)
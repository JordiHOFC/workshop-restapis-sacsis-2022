# Gerenciador de Funcionarios

O Objetivo deste projeto é construir uma API REST que seja capaz de:

- Cadastrar um funcionario 
- Consultar um funcionario 
- Deletar um funcionario 

## Tecnologias

Para conquistar este objetivo utilizaremos linguagem de programação Java na versão 11, Banco de dados Relacional PostgreSQL, e os frameworks do ecossistema Spring Boot, dentre eles: 

1. Spring Web

    É um conjunto de ferramentas que nós permite criar codigo referente a camada da WEb, esta depêndencia traz consigo as capacidades de subir um servidor Tomcat, expor endpoints, serializar e desserializar Jsons e outros.

2. Spring Validation
    
    Habilita a capacidade de fazer validações em classes Java de maneira declarativa através da Bean Validation e Hibernate Validator.

3. Spring Data JPA 

    Habilita a capacidade de lidar com Banco de Dados relacional, através dela é que vamos mapear nossos objetos Java em Entidades do BD. Spring Data JPA segue a especificação JPA e tem Hibernate como implementação padrão.


## Execute seu projeto

Para executar o aplicativo, entre no diretorio e execute:

```shell
./mvnw spring-boot:run
```
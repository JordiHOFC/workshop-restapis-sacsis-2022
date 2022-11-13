Agora que já estamos com nossa API REST finalizada, vamos melhorar o que já criamos. Nossa principal fonte de melhoria sera na classa `Livro` e `CadastraLivroController`. 

## Refatorando a `CadastraLivroController`

No decorrer do workshop aprendemos que uma Resposta HTTP,  é composta com cabeçalhos (Headers), Status Code e o corpo (Body). Também aprendemos que em caso de sucesso devemos utilizar os Status da familia 2xx.

O Status 200 OK é um status generalista, e pode ser utilizado em diversas situações, como por exemplo, na leitura de informações, relacionados ao verbo HTTP GET.

Como nosso Controller esta criando um novo recurso no servidor, e esta operação atende o verbo POST, podemos utilizar um Status que traga mais significado a nossa operação, como é o caso do Status 201 CREATED, que deve ser utilizado quando uma nova instância de um recurso é criada no Servidor.

Para determinar uma ResponseHttp mais elaborada, o Spring fornece a classe `ResponseEntity`, e ela nos permite, definir informações para Headers, Body e Status de maneira programatica. Iremos utiliza-la como tipo de Retorno do metodo cadastrar para se aproveitar destes beneficios.


1. Altere assinatura do metodo cadastrar para retornar ResponseEntity, e adapte o retorno para informar o status. Veja como deve fica o Controller após as modificações.

```JAVA
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import javax.validation.Valid;

@RestController
public class CadastraLivroController {
    private final LivroRepository repository;

    public CadastraLivroController(LivroRepository repository) {
        this.repository = repository;
    }

    @PostMapping("/livros")
    public ResponseEntity<?> cadastrar(@RequestBody @Valid LivroRequest request) {

        System.out.println(request);

        Livro livro = request.toModel();
        repository.save(livro);

        return ResponseEntity.status(HttpStatus.CREATED).build();
    }
}
```


## Refatorando a classe `Livro`

Nesta classe precisaremos fazer duas mudanças, a primeira para favorecer a criação dos nosso objetos, aumentar o encapsulamento, permitindo que os valores não sejam alterados de maneira indevida em outros pontos do sistema. Então justifica a criação do construtor que recebe titulo, descricao, isbn e preco.

E a segunda por exigencia da JPA/Hibernate que por padrão necessita que uma classe contenha o construtor vazio, que é dado por padrão quando nenhum outro construtor é criado.

1. Criando o construtor cheio.

    1. Após entrar na classe livro, se direcione para linha após ultimo atributo, e utilize o atalho: `ALT + INSERT`, e selecione a opção construtor.
    2. Selecione os atributos: titulo, descricao, isbn e preco; e clique em ok.

2. Criando o construtor vazio (default)
    1. Utilize o atalho: `ALT + INSERT`, e selecione a opção construtor.
    2. Clique no botão **select none** 
    3. Para indicar que este construtor não deve ser utilizado no sistema, anote-o com @Deprecated.

3. Removendo todos os Setters
    1. Apague todos os setters

No fim sua classe devera ser igual o codigo abaixo:

```JAVA
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Positive;
import javax.validation.constraints.Size;
import java.math.BigDecimal;

@Entity
public class Livro {
    @Id
    @GeneratedValue
    private Long id;
    @NotBlank
    @Size(max = 35)
    private String titulo;
    @NotBlank
    @Size(min = 90)
    private String descricao;
    @NotBlank
    private String isbn;
    @NotNull
    @Positive
    private BigDecimal preco;

    public Livro(String titulo, String descricao, String isbn, BigDecimal preco) {
        this.titulo = titulo;
        this.descricao = descricao;
        this.isbn = isbn;
        this.preco = preco;
    }

    @Deprecated
    public Livro() {
    }

    public Long getId() {
        return id;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getDescricao() {
        return descricao;
    }

    public String getIsbn() {
        return isbn;
    }

    public BigDecimal getPreco() {
        return preco;
    }

}
```

## Adaptando o metodo toModel na classe `LivroRequest`

1. Abra a classe `LivroRequest`
2. No metodo `toModel()` apague a chamada de todos metodos Setters, já que não existem mais
3. No lugar do contrutor vazio, chame o construtor cheio. Ao fim sua classe deverá corresponder com o codigo abaixo.

```JAVA
public class LivroRequest {
    @NotBlank
    @Size(max = 35)
    private String titulo;
    @NotBlank
    @Size(min = 90)
    private String descricao;
    @NotBlank
    private String isbn;
    @NotNull
    @Positive
    private BigDecimal preco;


    public Livro toModel() {
        return new Livro(titulo, descricao, isbn, preco);
    }

    public String getTitulo() {
        return titulo;
    }

    public String getDescricao() {
        return descricao;
    }

    public String getIsbn() {
        return isbn;
    }

    public BigDecimal getPreco() {
        return preco;
    }
}
```
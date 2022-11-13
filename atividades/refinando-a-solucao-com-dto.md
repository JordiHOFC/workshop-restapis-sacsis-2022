Agora que nossa solução já esta quase pronta, iremos nos atentar a melhora-la, principalmente contra brechas de segurança. 

Se formos analisar o `CadastraLivroController` iremos perceber que estamos recebendo a entidade `Livro`, o que representa uma brecha para ataques de Atribuição em Massa.

Uma solução é criar uma nova classe que se resposabilize por receber essas informações e as valide, e caso respeitem as regras converteremos para a entidade `Livro`.

Quando criamos classes com objetivo de transferir informações de um ponto do sistema para outro, estamos implementando um padrão chamada de Data Transfer Object (DTO), ou Objeto de Transferencia de Dados em português.

Bora implementar?

1. Crie uma classe chamada `LivroRequest`.
    - Crie os atributos Titulo, Descricao, Isbn e Preco.
    - Defina as mesmas validações da classe `Livro` utilizando a BeanValidation.
    - Defina os Getters e Setters, atraves do atalho:  `ALT + INSERT`
    
    No fim você tera uma classe como esta:

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
        //Getters e Setters omitidos
    }
    ```

2. Crie um metodo de conversão de `LivroRequest` para `Livro`, coloque o nome de `toModel()`.

    1. Utilize o modificador de acesso **`public`**, defina o retorno para `Livro`
    2. Dentro do corpo do metodo instancie um `Livro`, e utilize os setters para atribuir os valores
    3. Retorne o livro utilizando o **`return`**

    Seu metodo deve ser igual ao codigo abaixo:

    ```JAVA
    public Livro toModel(){
        Livro novoLivro = new Livro();
        
        novoLivro.setTitulo(titulo);
        novoLivro.setDescricao(descricao);
        novoLivro.setIsbn(isbn);
        novoLivro.setPreco(preco);

        return novoLivro;
    }
    ```

## Adaptando o CadastraLivroController

1. Inicialmente é necessário que o parametro do metodo cadastrar, seja alterado de `Livro` para `LivroRequest`.

2. Agora é necessário que aconteça a conversão antes da chamada do metodo `save()`.
   - Para fazer a conversão utilize o metodo `toModel()`

Após as modificações seu codigo deve ficar igual o demostrado abaixo.


```JAVA
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
    public void cadastrar(@RequestBody @Valid LivroRequest request) {

        System.out.println(request);

        Livro livro = request.toModel();
        repository.save(livro);

    }
}
```



## Testando a API com Insomnia

1. Realize os teste de caso de sucesso, conforme demostrado anteriormente.
2. Realize o teste de caso de erro, conforme demostrado anteriormente.

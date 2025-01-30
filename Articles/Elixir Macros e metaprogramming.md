
meta programação é feita de vários jeitos diferentes em linguagens diferentes, mas no Elixir é alcançada por meio de macros. 
Antes de entender o que são os macros é legal ter em mente o que é e como funciona a metaprogramação porque no fim estamos no contexto de metaprogramação. 

Basicamente no contexto do Elixir, metaprogramação é escrever código que em tempo de compilação, vai gerar mais código

##### AST
abstract syntax tree
arvore sintática abstrata é uma estrutura de dados onde a função é representar a estrutura hierarquica de um programa em forma de árvore, com galhos e folhas. 

quando a gente escreve código em qualquer linguagem de programação, o compilador ou interpretador precisa entender a estrutura do código pra traduzir para bit code, a ast é a representação intermediaria desse código.

a ast representa o codigo fonte em nivel abstrato, facilitando a visualização 
##### Quote e Unquote
quote do desestrutura a minha função em truplas de funções que são executadas nessa expressão
por exemplo
`quote do: if val do: "truly", else do: "falsy"`  
o que acontece é que o iex vai desestruturar esse if e mostrar como ele funciona e sua ordem na arvore AST
### Metaprogramação

A metaprogramação é uma prática avançada que envolve a escrita de código capaz de manipular ou gerar outro código durante o tempo de compilação ou execução. Em Elixir, essa prática é predominantemente realizada por meio de macros. Isso transcende os paradigmas tradicionais de programação, permitindo que os desenvolvedores criem abstrações poderosas e automatizem tarefas complexas. Com a metaprogramação, os desenvolvedores não apenas escrevem software, mas também constroem as ferramentas que escrevem software.

##### O que são Macros e pra que servem?

Macros em Elixir são uma funcionalidade avançada que permite aos desenvolvedores criar blocos de código reutilizáveis que geram código Elixir em tempo de compilação. De cara uma das vantagens é facilitar a escrita de código. Ao permitir que os desenvolvedores escrevam código que escreve código, os macros desbloqueiam um vasto potencial para metaprogramação.
### Os Benefícios dos Macros 
Os macros em Elixir oferecem uma variedade de benefícios que transcendem a mera conveniência. Eles se tornam os pilares sobre os quais a criatividade e a eficácia do desenvolvedor são construídas:

1. **Redução da Repetição de Código**: Macros podem encapsular padrões de código repetitivos, reduzindo a quantidade de digitação necessária e tornando o código mais limpo e legível. Essa capacidade de abstração permite que os desenvolvedores se concentrem em conceitos de alto nível, sem se perderem em detalhes triviais.
    
2. **Criação de DSLs Personalizadas**: Com macros, os desenvolvedores podem criar suas próprias DSLs para expressar soluções para problemas específicos de forma mais concisa e intuitiva. Isso permite que o código se assemelhe mais a uma linguagem natural, tornando-o mais acessível e legível para outros desenvolvedores, mesmo aqueles sem conhecimento profundo da linguagem.
    
3. **Implementação de Funcionalidades Avançadas**: Os macros podem ser usados para implementar funcionalidades avançadas que não seriam possíveis apenas com as construções padrão da linguagem. Isso proporciona uma maior flexibilidade e poder de expressão, permitindo que os desenvolvedores estendam a linguagem de acordo com suas necessidades específicas.
    
4. **Otimização de Desempenho**: Em certos casos, os macros podem ser usados para otimizar o desempenho do código, gerando código mais eficiente em tempo de compilação. Essa otimização pode resultar em melhorias significativas no desempenho e na escalabilidade de sistemas Elixir, especialmente em cenários de alta carga e concorrência.
    

### Exemplo Prático

Vamos agora mergulhar em um exemplo prático para ilustrar a aplicação dos macros em Elixir. Consideremos a criação de um DSL de validação para objetos de domínio. O objetivo é fornecer uma sintaxe elegante e expressiva para definir regras de validação de forma declarativa.

```elixir
defmodule User do
  defmacro validates_presence(field) do
    quote do
      def validate(%__MODULE__{unquote(field) => nil} = _user) do
        {:error, unquote(field), "can't be blank"}
      end
      def validate(_), do: :ok
    end
  end
end

defmodule MyApp do
  require User

  User.validates_presence :name
end

```

Neste exemplo, estamos definindo um macro `validates_presence` que gera um código de validação para verificar se um determinado campo está presente. Este código é então utilizado dentro de um módulo `MyApp` para validar os usuários. Esta abordagem permite uma definição concisa e expressiva das regras de validação, melhorando a legibilidade e a manutenibilidade do código.

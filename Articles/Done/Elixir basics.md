---
gists:
  - id: 3c54331a0c50cc1f3593cdd61395bd32
    url: 'https://gist.github.com/starch0/3c54331a0c50cc1f3593cdd61395bd32'
    createdAt: '2024-04-06T14:58:45Z'
    updatedAt: '2024-04-07T18:38:12Z'
    filename: Elixir.md
    isPublic: false
---
#elixir #article #ongoing

## Embarcando no Elixir, Mix e Ecto. 

Escrevo este artigo supondo que você, leitor tem noções básicas de programação. Pretendo manter a leitura simples , mas se sua primeira linguagem de programação for Elixir, talvez fique confuso. 
### Elixir


Elixir é uma linguagem de programação funcional que roda em cima da máquina virtual do ERlang a BEAM. 
Elixir é mundialmente usado principalmente em outros países e grandes produtos como Brex, Discord, Pinterest e vários outros, mas é um orgulho nacional.

O diferencial do Elixir é que seu código é executado em processos isolados, esse isolamento permite que o retorno de cada processo possa ser coletado de forma independente, utilizando da melhor forma os recursos da maquina.

### Tipos de dados em Elixir

Em Elixir existem tipos de dados que são utilizados para representar diferentes tipos de informações, como:

`int` Inteiros que podem ser positivos ou negativos
`float` Pontos flutuantes que representam números decimais.
`string` Que representam uma sequencia de caracteres escritos entre aspas duplas
`boolean` Representa valores verdadeiros ou falsos / `true` `false`
`atom` Que são constantes onde o próprio valor é equivalente ao próprio nome, por exemplo `:true, :heyThere, :barco`
##### Sintaxe e Módulos
Vamos começar um Hello World pra dar boa sorte.
```elixir
defmodule helloModule do
	def hello do
	IO.puts("Hello World") #Output => Hello World
	end
end
```

Aqui já é possível notar algumas 'peculiaridades do Elixir' que são os `módulos`.
Módulos no Elixir são unidades de organização de código e são usados pra agrupar funcionalidades relacionadas. Módulos também podem conter valores, constantes e até outros módulos. 
Você também pode criar uma função sem encapsular com um módulo, mas em Elixir é uma prática recomendada encapsular funções em módulos para organizar e estruturar seu código de forma mais clara e modular.
Módulos em Elixir também são átomos
```elixir
iex> is_atom(helloModule) #Output => true
```

Como citei, em Elixir é possível criar módulos dentro de módulos, mas por convenção, é recomendado criar apenas um módulo por arquivo. (A não ser que situação peça o contrário) 

Além disso também temos duas extensões principais dentro do Elixir, sendo `.ex` e `.exs`.
os arquivos `.ex` são usados para código compilado que faz parte do aplicativo ou biblioteca, enquanto os arquivos `.exs` são usados para scripts Elixir executados diretamente pelo interpretador, principalmente para tarefas de automação, experimentação e prototipagem.

#### Supervisão e Supervisores

Os conceitos de supervisão e supervisores vem do sistema ERlang. Em Elixir os supervisores são responsáveis pelo monitoramento do ciclo de vida de um processo filho e reinicialização dos mesmos quando necessário. 
O supervisor garante que, se o processo falhar, ele seja reiniciado de acordo com a estratégia definida. Isso ajuda a criar sistemas tolerantes a falhas. 

Existe alguns tipos de supervisores:

- Supervisor Simples:  Tipo mais básico, supervisiona processos e os reinicia se falharem.
- Supervisor Dinâmico: Com este é possível adicionar e remover processos filhos dinamicamente durante a execução do sistema.
- Supervisor de Tarefa: Este supervisor supervisiona tarefas assíncronas, como operações de  E/S .(Entrada e Saída)
- Supervisor de Grupo: Permite agrupar processos filhos para gerenciar em conjunto.

Exemplo de um supervisor:
```elixir
defmodule GenericSupervisor do
	use Supervisor
	def start_link do
		Supervisor.start_link(__MODULE, [])
	end
	def init([]) do
	children = [
		worker(GenericWorker), []]
		Supervisor.init(children, strategy: :one_for_one)
	end
end
```
Nesse exemplo o `GenericSupervisor` supervisiona um único processo que, se falhar, será reiniciado, de acordo com a estratégia `:one_for_one`

### Mix

Mix é uma para ajudar no desenvolvimento de aplicações Elixir. Ela é responsável por criar, compilar e gerenciar projetos, bem como tarefas de automação e teste.

Com Mix, você pode facilmente iniciar um novo projeto Elixir usando o comando `mix new projeto`. Isso criará uma estrutura de diretórios padrão para o seu projeto, com arquivos de configuração, testes e o esqueleto básico do código.
```shell
├── README.md
├── lib
│   └── projeto.ex
├── mix.exs
└── test
    ├── projeto_test.exs
    └── test_helper.exs
```

###### Estrutura de um projeto Mix

`lib` É neste diretório onde fica o código fonte do seu projeto e os arquivos `.ex` aqui dentro serão compilados.

`mix.exs` Este é o arquivo de configuração principal do seu projeto. É aqui onde você define metadados do projeto, dependências, tarefas personalizadas e outras configurações.

`test` Neste diretório ficam os arquivos de testes do seu projeto. Os arquivos de teste são escritos usando o ExUnit e têm a extensão `.exs`

Além disso, Mix é usado para compilar e executar seu código. Por exemplo, você pode usar `mix compile` para compilar seu projeto e `mix run` para executá-lo.

Mix também suporta a criação de tarefas personalizadas, que podem ser usadas para automatizar tarefas comuns de desenvolvimento, como a execução de testes ou a geração de documentação.

### Ecto

Ecto é uma biblioteca de persistência de dados para o Elixir, projetada para trabalhar com bancos de dados relacionais. Ela fornece uma maneira limpa e elegante de interagir com o banco de dados, usando uma abordagem baseada em modelos e consultas.

(Persistência de dados é o conceito de armazenar informações de forma duradoura, de modo que elas possam ser recuperadas e utilizadas posteriormente, mesmo após o encerramento da aplicação que as gerou )


Com Ecto, você pode definir modelos para representar suas tabelas de banco de dados e usar esses modelos para criar, atualizar, excluir e consultar registros no banco de dados.

Além disso, Ecto fornece um mecanismo de migração que permite gerenciar o esquema do banco de dados ao longo do tempo. Com migrações, você pode criar e modificar tabelas, índices e restrições de chave estrangeira de forma controlada e reversível.

Ecto também suporta consultas usando a linguagem de consulta Ecto.Query. Com Ecto.Query, você pode construir consultas SQL de forma programática, tornando mais fácil e seguro gerar consultas dinâmicas com base em variáveis de entrada.

Ecto também oferece suporte a transações, permitindo que você execute várias operações de banco de dados como uma única unidade atômica. Isso garante que todas as operações sejam bem-sucedidas ou que nenhuma delas seja aplicada, mantendo a consistência dos dados.

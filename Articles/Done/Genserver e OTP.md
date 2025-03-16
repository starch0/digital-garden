---
gists:
  - id: 3990772313b8b0f0a80424c5f862053e
    url: 'https://gist.github.com/starch0/3990772313b8b0f0a80424c5f862053e'
    createdAt: '2024-04-13T13:36:59Z'
    updatedAt: '2024-05-02T21:53:07Z'
    filename: Genserver and OTP.md
    isPublic: false
  - id: 281242d8f4a1f632db4bcc18831d0784
    url: 'https://gist.github.com/starch0/281242d8f4a1f632db4bcc18831d0784'
    createdAt: '2024-09-12T12:16:24Z'
    updatedAt: '2024-10-16T20:25:58Z'
    filename: Genserver e OTP.md
    isPublic: true
---
### Elixir e seus processos

Elixir é uma linguagem de programação funcional que roda na Erlang VM, a BEAM, conhecida pela capacidade de construir sistemas distribuídos, concorrentes e bastante resilientes. Um dos conceitos em Elixir pra lidar com concorrência e estado é o GenServer. 

Nesse artigo vamos destrinchar o GenServer, entender por que ele é útil e como implementá-lo. 

Se você não sabe o que é [Programação funcional](https://dev.to/starch1/iniciando-na-programacao-funcional-1o5p) ou [Elixir](https://dev.to/starch1/embarcando-no-elixir-mix-e-ecto-2eg4), eu já escrevi um pouco sobre ambos.

#### Processos em Elixir

Antes de entrar em outros tópicos eu quero falar um pouco sobre os processos em Elixir a fim de facilitar a leitura sobre os outros tópicos. 

Processos em Elixir são parecidos com [threads virtuais](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html) onde a VM da linguagem é quem gerencia os processos, a BEAM cria e gerencia os processos Elixir, que são internamente mapeados para um thread do sistema operacional, mas os processos em Elixir não estão vinculados a nenhum thread do sistema operacional e são completamente gerenciados pela BEAM. Cada processo inicialmente ocupa um espaço pequeno  e com isso criar um processo é muito rápido porque não requer 'intervenção' direta do sistema operacional. 

isso significa que você pode criar milhares ou até mesmo milhões de processos simultaneamente sem sobrecarregar significativamente o sistema. 
### Genserver

Basicamente GenSever (Ou Generic Server) é um processo em Erlang/Elixir que executa em um loop infinito que pode receber, enviar mensagens e gerenciar estados.

Ele é construído em cima de vários conceitos relacionados ao primitivo de concorrência, os processos, como criação de processos, registro de processos, vinculação de processos, passagem de mensagens, envio de mensagens síncronas, gerenciamento de estado usando recursão do bloco de recebimento, etc. Algumas das principais aplicações do GenServer são a criação de processos de servidor de longa duração que gerenciam estado, respondem a solicitações de forma síncrona e assíncrona, suportam resiliência via supervisores, fornecem rastreamento, depuração, monitoramento, atualização de estado durante troca de código em execução e capacidades de relatório de erros.

Erlang tem um modulo chamado `:gen_server` que define um numero de funções que devem ser implementadas pelo módulo "generic server". Se você estiver familiarizado com outras linguagens de programação, `:gen_server` é como uma interface, em Erlang, um módulo define quais funções devem ser implementadas por outros módulos, e isso é chamado de *behavior* ou comportamento. 

Em Elixir temos o módulo `GenServer` que diferente do módulo`:gen_server` do Erlang, o `GenServer` é um módulo que você implementa através da macro `use` 

```elixir
defmodule SomeGenericModule do
	use GenServer
end
```

Basicamente quando você usa `use GenServer`, o Elixir trás por padrão várias implementações do módulo `:gen_server`, isso permite que você implementa apenas as funções necessárias no seu módulo, enquanto pode deixar o restante das funções retornar os padrões do `GenServer`

Algumas das características mais importantes do GenServer são:
###### Comunicação síncrona e assíncrona: 
Processos podem se comunicar através de mensagens síncronas, enviando uma mensagem e esperando a resposta, ou assíncrona enviando mensagens, independente de ter um retorno ou não. 
###### Supervisão e tolerância a erros:
Processos GenServer são frequentemente usados com árvores de supervisores, permitindo que eles possam ser supervisionados e reiniciados quando necessário.
###### Tratamento de mensagens:
Os processos recebem mensagens de forma assíncrona e as tratam usando correspondência de padrões no conteúdo da mensagem. O comportamento do servidor pode ser definido com base na mensagem recebida.
### OTP 

A sigla significa 'Open Telecom Platform', mas hoje em dia OTP não é mais algo exclusivo da Telecom.  OTP é basicamente uma coleção de bibliotecas padrões de projeto e diretrizes de arquitetura para o desenvolvimento de sistemas em Erlang e Elixir. OTP oferece uma abordagem estruturada para construir sistemas distribuídos e concorrentes, incorporando conceitos como processos leves (também conhecidos como "processos Erlang")
### Modelo de ator

Em Elixir, cada ator é um processo isolado(Nesse ponto você já deve ter notado que em Elixir tudo é função / processo) e independente que executa sua lógica de forma concorrente em relação a outros atores. Essa abordagem é altamente escalável, já que permite que centenas, milhares ou até mesmo milhões de atores coexistam e cooperem em um sistema, sem a necessidade de preocupação com a sincronização manual de threads ou locks.

Cada ator possui um identificador único, o PID (identificador de processo), que permite enviar mensagens para ele de forma direta. As mensagens são enfileiradas em uma 'mailbox' associada a cada ator, garantindo que as mensagens sejam processadas de forma sequencial e na ordem em que foram recebidas. Isso promove uma comunicação assíncrona entre os atores, evitando bloqueios desnecessários e melhorando a eficiência do sistema.

Além disso, o modelo de ator em Elixir é construído sobre o conceito de imutabilidade e isolamento de estado. Cada ator possui seu próprio estado interno, que só pode ser modificado através do processamento de mensagens. Isso simplifica a construção de sistemas concorrentes, reduzindo a necessidade de compartilhamento de estado e eliminando condições de corrida e problemas de concorrência.

Vamos brincar um pouco.

```elixir
defmodule GenericActor1 do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok, name: __MODULE__)
  end
  
  def init(state) do
  
    {:ok, pid} = GenServer.start_link(GenericActor2, self(), name: GenericActor2)
    schedule_message(pid)
    {:ok, state}

  end

  def handle_info(:send_ok, state) do
  
    io_pid = inspect(self())
    IO.puts("Actor 1 received from #{io_pid}")
    schedule_message(self())
    {:noreply, state}

  end

  defp schedule_message(pid) do
    Process.send_after(pid, :send_ok, 2000)
  end
end

defmodule GenericActor2 do
  use GenServer
  
  def start_link(pid) do
    GenServer.start_link(__MODULE__, pid, name: __MODULE__)
  end

  def init(pid) do
  
    schedule_message(pid)
    {:ok, nil}
  end

  def handle_info(:send_ok, state) do

    io_pid = inspect(self())
    IO.puts("Actor 2 received from #{io_pid}")
    schedule_message(self())
    {:noreply, state}
  end

  defp schedule_message(pid) do
    Process.send_after(pid, :send_ok, 2000)
  end
end
```

No código acima eu criei dois atores que dentro de um intervalo de tempo enviam `:ok`  e só respondem um ao outro após receber o `:ok`

"Tá, mas como eu vejo isso acontecendo?"

Inicia o iex interativo com `iex -S mix`
Depois inicie um dos atores com `iex> GenericActor1.start_link`
E veja a mágica acontecendo.
```shell
iex(1)> GenericActor1.start_link
{:ok, #PID<0.152.0>}
Actor 1 received from #PID<0.152.0>
Actor 2 received from #PID<0.153.0>
Actor 1 received from #PID<0.152.0>
Actor 2 received from #PID<0.153.0>
Actor 1 received from #PID<0.152.0>
Actor 2 received from #PID<0.153.0>
Actor 1 received from #PID<0.152.0>
Actor 2 received from #PID<0.153.0>
Actor 1 received from #PID<0.152.0>
Actor 2 received from #PID<0.153.0>
```
É um exemplo super básico, mas tendo processos independentes dessa forma torna as possibilidades infinitas. 


---
gists:
  - id: 5acdcdd4c9934f841e36fb3eedbd0442
    url: 'https://gist.github.com/starch0/5acdcdd4c9934f841e36fb3eedbd0442'
    createdAt: '2025-01-13T21:30:26Z'
    updatedAt: '2025-01-14T19:08:43Z'
    filename: Explicando peer-to-peer(P2P).md
    isPublic: true
    baseUrl: 'https://api.github.com'
---
## O que é peer-to-peer?

O modelo **peer-to-peer** (P2P) é uma arquitetura de rede que permite a troca direta de dados entre dispositivos, eliminando a necessidade de servidores centralizados. Essa abordagem descentralizada tem sido amplamente utilizada em diversas áreas, como compartilhamento de arquivos, sistemas financeiros, e até mesmo em aplicações de comunicação. 
Vale pontuar que em arquiteturas baseadas em servidores, o ciclo é: Cliente faz uma requisição > Servidor responde. E a internet precisa de um servidor principal. 
Enquanto em uma conexão P2P, todos os peers trabalham como clientes e servidores e a conexão é organizada de forma descentralizada. 

![[Pasted image 20250113182824.png]]
### Como Funciona o P2P?

Primeiro, vamos pontuar como uma conexão baseada em servidores trabalha.
Quando estamos acessando algum conteúdo, como uma stream na Twitch, por exemplo, o seu celular(Ou PC) trabalha como o Cliente, então você abre o app escolhe o streamer que quer assistir e o app faz uma requisição pro servidor, o servidor recebe esta requisição e responde com o conteúdo que você quer assistir no seu dispositivo. 


Em uma rede P2P, cada dispositivo conectado à rede  – conhecido como _peer_ (ou par) – pode atuar tanto como cliente quanto como servidor. Isso significa que os _peers_ podem tanto consumir quanto fornecer recursos, como dados, armazenamento e poder de processamento. Essa caracterização contrasta com o modelo tradicional cliente-servidor, em que o cliente solicita serviços de um servidor central.

Mas se nós quisermos fazer a mesma request em uma arquitetura P2P, ao invés de enviar a requisição pro servidor central, seu celular(Ou PC) vai simultaneamente enviar requisições para vários outros dispositivos (Outros peers) dentro da rede. 

Então, cada peer vai responder com parte do video. Isso significa que ao invés de fazer o download do video inteiro de uma única fonte você vai receber diferentes partes de várias fontes da rede

Ao mesmo tempo que o seu dispositivo recebe partes do vídeo, ele vai começar a compartilhar estas partes com outros clientes que estiverem buscando o mesmo conteúdo.

![[https___dev-to-uploads.s3.amazonaws.com_uploads_articles_2r66adm6ewigb9g79vpc.gif]]

### Tipos de rede P2P

#### Rede estruturada

As informações são guardadas em um DHT (Distributed Hash Table), permitindo pesquisas específicas e respostas baseadas em índices. Geralmente, essas redes são mais eficientes e escaláveis para ambientes com um grande número de nós.

#### Redes não estruturadas

As redes P2P não estruturadas não possuem uma organização pré-definida entre os nós. Qualquer nó pode atuar como cliente ou servidor, conectando-se de maneira arbitrária. Essas redes são mais simples de configurar, mas podem enfrentar dificuldades em localizar recursos específicos em grandes redes, especialmente sob alta carga de tráfego.

#### Redes híbridas

Redes híbridas combinam características de redes estruturadas e não estruturadas. Elas geralmente contam com nós superpoderosos (supernós) que desempenham papéis de coordenação, como indexação de conteúdo ou gerenciamento de conexões. Essa abordagem busca aproveitar a eficiência das redes estruturadas e a simplicidade das não estruturadas, oferecendo uma solução equilibrada entre desempenho e flexibilidade.

### Beleza, mas por que usar P2P, quais as vantagens?
Se eu ainda não te convenci, continua lendo.

###### Segurança
Como redes P2P não possuem um servidor principal(Na maioria dos casos), elas não podem ser hackeadas facilmente, mesmo se um dos peers for atacado, o impacto no sistema é minimo
###### Escalabilidade
Quanto mais peers interagirem com nossa rede, mais peers trabalham como servidores. Considerando isto, a capacidade de uma rede P2P é ilimitada.
###### Flexibilidade
Como mencionei, existem vários tipos de rede P2P, dependendo dos nossos requisitos nós podemos escolher e implementar uma estrutura que cabe melhor na nossa aplicação.

E eu acho que é isso :). Espero que eu tenho te ajudado a entender melhor sobre redes P2P. Se em algum momento eu errei, pode me corrigir, se precisar de ajuda para entender algo melhor, sinta-se livre para entrar em contato :)

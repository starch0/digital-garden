Um encurtador de links é um serviço na web, ou até mesmo uma ferramenta que recebe URLs (Uniform Resource Locator) terrivelmente grandes e converte em URLs menores, como o nome sugere.  A principal ideia de um encurtador de links é fazer URLs gigantes serem mais amigáveis para o usuário final. 

É importante ter em mente que enquanto encurtadores de URL oferecerem vários benefícios, eles são uma faca de dois gumes, porque eles também costumam 'ofuscar' o link 

Basicamente o fluxo do nosso encurtador vai ser:
Receber o URL em um input e retornar o URL encurtado;
Se receber uma URL, redirecionar o usuário para o URL original;
O URL gerado deve conter apenas alfabetos (a ~ z, A ~Z, a ~Z) e Números ( 0 ~9 )
Se o URL encurtado já existir para o input recebido, retornar o mesmo URL encurtado ao invés de criar um novo.

Por exemplo:
Recebemos o link: `https://www.ablablueblé.com/paginaGenerica-muito-longa-meuDeus-aaaaaaaaaaaaaaaa`
E então encurtamos para: `https://encurtadorGenerico/AbC312`


### O que são funções Hash?

Funções hash são algoritmos que transformam dados de tamanho variado em um valor de tamanho fixo. Por exemplo, ao aplicar uma função hash a um texto como `https://www.ablablueblé.com` ele retornará algo uma string única que representa os dados originais.

Elas são utilizadas em sistemas de segurança, como verificação de integridade e em aplicações como encurtadores de links(como vamos utilizar aqui).

### Mãos a massa, vamos desenvolver o encurtador de links. 

Vamos começar configurando o ambiente e criando a estrutura do projeto, para isto, a partir do seu terminal digite:
```bash
mkdir encurtador
cd encurtador
npm init -y #Isto vai iniciar o projeto e automaticamente confirmar todas as perguntas básicas do projeto.
```

##### Instale os pacotes ncessários:

```bash
npm install express body-parser dotenv
```

##### Crie a estrutura do projeto

Esta é a estrutura que iremos usar:
![[Pasted image 20250123202341.png]]
```
└── shortener
    ├── index.js
    ├── package-lock.json
    ├── package.json
    └── urls.json
```
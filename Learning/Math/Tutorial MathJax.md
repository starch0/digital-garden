---
gists:
  - id: 8f4144c66ad4eac15f31942da394f844
    url: 'https://gist.github.com/starch0/8f4144c66ad4eac15f31942da394f844'
    createdAt: '2025-04-04T01:47:08Z'
    updatedAt: '2025-04-04T01:47:08Z'
    filename: Tutorial MathJax.md
    isPublic: false
    baseUrl: 'https://api.github.com'
  - id: e6dccfbbdf5c651da24e2b561cab7612
    url: 'https://gist.github.com/starch0/e6dccfbbdf5c651da24e2b561cab7612'
    createdAt: '2025-04-04T01:47:17Z'
    updatedAt: '2025-04-04T01:47:18Z'
    filename: Tutorial MathJax.md
    isPublic: true
    baseUrl: 'https://api.github.com'
---


### 1. Introdução

MathJax é uma biblioteca JavaScript que renderiza expressões matemáticas escritas em LaTeX ou notação similar. Ela é amplamente utilizada em páginas web, editores de texto como o Obsidian e notebooks interativos como o Jupyter. O MathJax permite escrever equações de forma clara, utilizando duas notações principais: inline e display.

2. Modos de Renderização

- Inline: Envolva sua expressão com um cifrão simples. Exemplo:
  $a^2 + b^2 = c^2$
  
- Display: Envolva sua expressão com dois cifrões (ou com \[ e \]). Exemplo:
  $$
  a^2 + b^2 = c^2
  $$

3. Frações, Expoentes e Subscritos

- Fração:
  Sintaxe: ` \frac{numerador}{denominador}`
  Exemplo:
  $$
  \frac{1}{2}
  $$

- Expoente:
  Sintaxe: `a^{expoente}`
  Exemplo:
  $$
  e^{i\pi} + 1 = 0
  $$

- Subscrito:
  Sintaxe:` a_{subíndice}`
  Exemplo:
  $$
  x_{1}, x_{2}, \dots, x_{n}
  $$

4. Radiciação
- Raiz Quadrada:
  Sintaxe: \sqrt{expressão}
  Exemplo:
  $$
  \sqrt{x + y}
  $$

- Raiz Enésima:
  Sintaxe: `\sqrt[n]{expressão}`
  Exemplo:
  $$
  \sqrt[3]{8} = 2
  $$

5. Símbolos Gregos e Operadores

- Símbolos gregos comuns:
  Exemplo: `\alpha, \beta, \gamma, \Delta, \Omega`
  Renderização:
  $$
  \alpha + \beta = \gamma
  $$

- Operadores:
  - Soma (Somatório):
    Sintaxe: `\sum_{i=1}^{n} i`
    Exemplo:
    $$
    \sum_{i=1}^{n} i = \frac{n(n+1)}{2}
    $$

  - Produto (Produtório):
    Sintaxe: `\prod_{i=1}^{n} i`
    Exemplo:
    $$
    \prod_{i=1}^{n} i = n!
    $$

  - Integral:
    Sintaxe: `\int_{a}^{b} f(x) \, dx`
    Exemplo:
    $$
    \int_{0}^{1} x^2 \, dx = \frac{1}{3}
    $$

6. Delimitadores Ajustáveis
---------------------------
Para ajustar o tamanho dos parênteses, colchetes ou chaves conforme o conteúdo, use os comandos ` \left e \right`.
Exemplo:
$$
\left( \frac{a}{b} \right)
$$

7. Matrizes e Sistemas de Equações
----------------------------------
- Matriz simples:
  Sintaxe:
  ```\begin{matrix}`
  a & b \\
  c & d
  \end{matrix}```
  
  Exemplo:
  $$
  \begin{matrix}
  1 & 2 \\
  3 & 4
  \end{matrix}
  $$

- Matriz com colchetes:
  Sintaxe:
```\begin{bmatrix}
  a & b \\
  c & d
  \end{bmatrix}
  
  Exemplo:
  $$
  \begin{bmatrix}
  1 & 0 \\
  0 & 1
  \end{bmatrix}
  $$
  ```

- Sistemas de Equações (usando aligned ou array):
  Sintaxe:
```

  \begin{aligned}
  2x + 3y &= 6 \\
  4x - y &= 5
  \end{aligned}
  
  Exemplo:
  $$
  \begin{aligned}
  2x + 3y &= 6 \\
  4x - y &= 5
  \end{aligned}
  $$
  ```

8. Alinhamento de Equações
--------------------------
Para alinhar várias equações, use o ambiente align ou align* (sem numeração).
Exemplo:
```
\begin{align*}
f(x) &= x^2 + 2x + 1 \\
     &= (x + 1)^2
\end{align*}
```

9. Funções, Limites e Derivadas
- Função:
  Exemplo: `f(x) = ax^2 + bx + c`

- Limite:
  Sintaxe: `\lim_{x \to \infty} f(x)`
  Exemplo:
  $$
  \lim_{x \to \infty} \frac{1}{x} = 0
  $$

- Derivada:
  Sintaxe: `\frac{d}{dx} f(x)`
  Exemplo:
  $$
  \frac{d}{dx} \sin x = \cos x
  $$

10. Personalizando Espaços e Estilos

- Espaçamento:
  - \, gera um pequeno espaço.
  - `\quad` e `\qquad` geram espaços maiores.
  Exemplo:
  $$
  a \quad b \qquad c
  $$

- Texto em modo matemático:
  Use `\text{texto}` para incluir texto normal dentro de uma expressão matemática.
  Exemplo:
  $$
  \text{se } x > 0, \text{ então } \sqrt{x} \text{ é definido}
  $$

11. Comandos Avançados e Definições Personalizadas

Você pode definir novos comandos para simplificar expressões complexas.
Exemplo, para definir um comando para o conjunto dos reais:
`\newcommand{\R}{\mathbb{R}}`
Utilize-o assim:
$$ f: \R \to \R $$

Dicas Gerais

- Sempre teste suas expressões em um ambiente que suporte MathJax para garantir a renderização correta.
- Consulte a documentação oficial e exemplos online para aprofundar o conhecimento.
- Experimente combinar ambientes, como usar align dentro de blocos display para equações complexas.
- Para mais detalhes, consulte o tutorial completo em:
  https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference

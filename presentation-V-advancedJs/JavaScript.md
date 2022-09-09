# JavaScript - Advanced Concepts

### [ ] - Scope (global / local | function / block - var / let / const)

### [ ] - Closures

### [ ] - Hoisting?

### [ ] - Value vs Reference Assignment

### [ ] - Destructuring

### [ ] - Rest | Spread Operator

### [ ] - this

### [ ] - Currying

### [ ] - Prototype

### [ ] - async

<br/>
<br/>

# Scope

## Scope

A região dentro de um programa do qual a variável pode ser referenciada através de seu nome.

O Escopo determina a acessibilidade, **_visibilidade_** , das variaveis.

Ele é o contexto atual de execução onde valores e expressões estão visíveis ou podem ser executadas. Se a viariavel ou a expressão nao faz parte do escopoatual, ela não estará disponivel para uso.

### Visibility

O termo visibilidade está mais relacionado à Classes. Ele é aplicado aos membros da classe (atributos ou métodos) e determina um controle de acesso a partir de fora da classe onde eles são declarados.

A Visibilidade é definida por modificadores de acesso.

```js
const isScopeDifferentThanVisibilityInTheory = scope !== visibility;
// => true
```

## Scope in JS

JavaScript é uma linguagem de programação que tem escopo léxico, ou seja, é possivel acessar variavéis de um escopo externo a partir de um escopo interno, mas não vice-versa.

![Lexical Scope in JS](./assets/img/Scope.png)

```js
function outterScope() {
  const outter = `from outter scope`;
  console.log(outter);
  console.log(inner); // => ReferenceError: inner is not defined
  function innerScope() {
    const inner = `from inner scope`;
    console.log(inner);
    console.log(outter);
  }
}

outterScope();
```

Existem 3 escopos em JS:

> Global Scope - Escopo Global

> Function | Local Scope - Escopo de função | Escopo Local

> Block Scope - Escopo de bloco

<br>

> ### Global Scope

É o escopo padrão para todo o código. O mais externo.

Variáveis definidas no escopo global podem ser acessadas de qualquer lugar em um programa JS.

```js
let myName = `John`;
// myName é acessível no escopo global.

function sayMyName() {
  // myName é acessível aqui também.
}
```

Variáveis declaradas fora de uma função são declaradas no escopo global.

Seja ela declarada com var | let | const.

```js
// myName, herName, myDogsName são acessíveis no escopo global.
let myName = `John`;
var herName = `Julia`;
const myDogsName = `Cacau`;

function sayMyName() {
  function sayMyAge() {
    // myName, herName, myDogsName são acessíveis aqui também.
  }
}
```

Caso um valor seja atribuído a uma variável que não foi declarada, ela automaticamente se torna uma variável global.

```js
sayMyName();

// myName é acessível no escopo global mesmo sendo declarada dentro da função.
console.log(`escopo global`, myName); // => '`escopo global John'

function sayMyName() {
  myName = `John`;

  function sayMyAge() {
    console.log(`escopo sayMyAge`, myName); // => 'escopo sayMyAge John'
  }
  sayMyAge();
}
```

> ### Function | Local Scope

Uma declaração de função em JS cria um novo escopo.

```js
function sayMyName() {
  // Escopo da função sayMyName.
  // Escopo referente a este local.
}
```

Variáveis definidas dentro da função não são acessíveis, "visíveis", de fora dela.

Seja ela declarada com var | let | const.

```js
// myName, herName, myDogsName não são acessíveis no escopo global.
console.log(myName); // => ReferenceError: myName is not defined
console.log(herName); // => ReferenceError: herName is not defined
console.log(myDogsName); // => ReferenceError: myDogsName is not defined

function sayMyName() {
  let myName = `John`;
  var herName = `Julia`;
  const myDogsName = `Cacau`;

  // myAge, herAge, myDogsAge não são acessíveis aqui.
  console.log(myAge); // => ReferenceError: myAge is not defined
  console.log(herAge); // => ReferenceError: herAge is not defined
  console.log(myDogsAge); // => ReferenceError: myDogsAge is not defined

  function sayMyAge() {
    // myName, herName, myDogsName podem ser acessadas aqui.
    let myAge = 35;
    var herAge = 27;
    const myDogsAge = 4;
  }
}
```

> ### Block scope

Anteriormente ao ES6 (2015), existiam somente o escopo Global e o Escopo Local | Escopo de Função.

Com a introdução de let e const, foi definido mais um ipo de escopo, o escopo de bloco

Um bloco de código em Js é definido por código escrito dentro de **{...}**

```js
{
  let message = `from inside a block of code`;
  const otherMessase = `from inside the same block of code`;

  console.log(message); // => 'from inside a block of code'
  console.log(otherMessase); // => 'from inside the same block of code'
}

console.log(message); // => ReferenceError: message is not defined
console.log(otherMessase); // => ReferenceError: otherMessase is not defined
```

Variáveis declaradas com var não possuem escopo de bloco, e podem ser acessadas de fora do bloco de código.

```js
{
  var message = `from inside a block of code`;

  console.log(message); // => 'from inside a block of code'
}

console.log(message); // => 'from inside a block of code'
```

**const** e **let** resolvem o problema de vazamento de variaveis declaradas com var em escopo de bloco.

Outro exemplo desse vazamento é a declaração da variavel de inicialização do **_for loop_**

```js
for(var i = 0, i < 10, i++){}
console.log(i) // => 10

for(let i = 0, i < 10, i++){}
console.log(i) // => ReferenceError: i is not defined
```

> ## Sugestão de leitura

<br/>

> [Scope MDN](https://developer.mozilla.org/en-US/docs/Glossary/Scope)

> [Lexical Scope in JavaScript – Beginners Guide freeCodeCamp](https://www.freecodecamp.org/news/lexical-scope-in-javascript/)

> [Lexical Scope in JavaScript – What Exactly Is Scope in JS? - freeCodeCamp](https://www.freecodecamp.org/news/javascript-lexical-scope-tutorial/)

> [Lexical Scope in JavaScript - educative.io](https://www.educative.io/answers/lexical-scope-in-javascript)

> [JavaScript Variable Scopes - javascripttutorial.net](https://www.javascripttutorial.net/javascript-variable-scope/#)

<br/>
<br/>

# Closures

# Hoisting

# Value vs Reference Assignment

# Destructuring

# Rest | Spread Operator

# this

# Currying

# Prototype

# async

```

```

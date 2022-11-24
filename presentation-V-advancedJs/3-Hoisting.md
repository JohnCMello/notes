# 3. Hoisting

Quando um código JS é executado pela sua respectiva engine, ele cria um [contexto global de execução (global execution context)](https://www.javascripttutorial.net/javascript-execution-context/), e ele tem duas fases.

- Criação | Creation
- Execução | Execution

Durante a fase de criação, todas as declarações de variáveis e funções são 'içadas'. Esse içamento é conhecido como Hoisting.

Variáveis definidas com var são "movidas para o topo" e inicializadas com o valor de undefined por padrão.

Note que esse código não gera uma exceção.

```js
console.log(myName); // => undefined

var myName = `John`;

console.log(myName); // => 'John'
```

Tecnicamente esse é o código antes da fase de execução.

```js
var myName; // var myName = undefined

console.log(myName); // => undefined

myName = `John`;

console.log(myName); // => 'John'
```

A referência para a variável `myName` é criada em memória, o escopo é definido, e seu valor é inicializado como `undefined`.

Caso tentemos acessar uma variável inicializada que não foi declarada antes, obtemos um erro de referência.

```js
console.log(myName); // => ReferenceError, o interpretador não conhece myName.

myName = `John`;
```

Entretanto, a inicialização também causa a declaração de uma variável, ou seja, o código abaixo é executado, sem exceções, pois a variável é inicializada e declarada antes de ser utilizada

```js
myName = `John`; // inicialização de myName

console.log(myName); // => 'John'
```

Declarações de função também são 'içadas'.

```js
console.log(sayMyName(`John`));

function sayMyName(name) {
  return `Meu nome é ${name}`;
}
```

ou seja:

```js
function sayMyName(name) {
  return `Meu nome é ${name}`;
}

console.log(sayMyName(`John`));
```

## 3.1. let / const

<br>

Variáveis declaradas com `let` | `const` também são içadas, mas não são inicializadas com o valor padrão de `undefined`.

A leitura de variáveis declaradas com `let` e `const` antes da sua inicialização lança uma exceção.

```js
console.log(myName); // => ReferenceError

let myName = `John`;
```

```js
console.log(myAge); // => ReferenceError

const myAge = 35;
```

## 3.2. Classes

- Class declaration

Uma diferença importante entre class declaration e function declaration é que enquanto uma função pode ser executada antes da sua declaração, o mesmo não acontece com classes.

Declarações de classes sao içadas mas seus valores não são inicializados.

```js
const p = new Pessoa(); // => ReferenceError

class Pessoa {}
```

## 3.3. Function e Class expression

<br>

Function e class expressions tem o mesmo tratamento de declarações de variáveis. A função ou a classe será inicializada na fase de execução do contexto global de execução.

```js
console.log(myFunction); // => ReferenceError

const myFunction = function () {};
```

```js
console.log(myClass) // => ReferenceError

const myClass = class {
  constructor()
};
```

O mesmo acontece utilizando a sintaxe de Arrow functions

```js
console.log(myOtherFunction); // => ReferenceError

const myOtherFunction = () => {};
```

> ## Sugestão de leitura

<br/>

> [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

> [JavaScript Hoisting - W3Schools](https://www.w3schools.com/js/js_hoisting.asp)

> [JavaScript Hoisting - javascripttutorial.net](https://www.javascripttutorial.net/javascript-hoisting/)

> [O que é Hoisting em Javascript? - Medium](https://medium.com/devzera/o-que-%C3%A9-hoisting-em-javascript-b8c629178518)

> [JavaScript Hoisting - Programiz](https://www.programiz.com/javascript/hoisting)

**[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

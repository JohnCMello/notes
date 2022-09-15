# JavaScript - Advanced Concepts

### [X] - Scope (global / local | function / block - var / let / const)

### [X] - Closures in JS

### [X] - Hoisting

### [X] - Value vs Reference Assignment

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

O Escopo determina a acessibilidade, **_visibilidade_** , das variáveis.

Ele é o contexto atual de execução onde valores e expressões estão visíveis ou podem ser executadas. Se a variável ou a expressão nao faz parte do escopo atual, ela não estará disponível para uso.

## Visibility

O termo visibilidade está mais relacionado à Classes. Ele é aplicado aos membros da classe (atributos ou métodos) e determina um controle de acesso a partir de fora da classe onde eles são declarados.

A Visibilidade é definida por modificadores de acesso.

```js
const isScopeDifferentThanVisibilityInTheory = scope !== visibility;
// => true
```

## Scope in JS

JavaScript é uma linguagem de programação que tem escopo léxico, ou seja, é possível acessar variáveis de um escopo externo a partir de um escopo interno, mas não vice-versa.

![Lexical Scope in JS](./assets/img/Scope.png)

```js
function outerScope() {
  const outer = `from outter scope`;
  console.log(outer);
  console.log(inner); // => ReferenceError: inner is not defined

  function innerScope() {
    const inner = `from inner scope`;
    console.log(inner);
    console.log(outer);
  }
}

outerScope();
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

Com a introdução de let e const, foi definido mais um tipo de escopo, o escopo de bloco

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

**const** e **let** resolvem o problema de vazamento de variáveis declaradas com var em escopo de bloco.

Outro exemplo desse vazamento é a declaração da variável de inicialização do **_for loop_**

```js
for(var i = 0, i < 10, i++){}
console.log(i) // => 10

for(let i = 0, i < 10, i++){}
console.log(i) // => ReferenceError: i is not defined
```

> ## Sugestão de leitura

<br/>

> [Scope - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Scope)

> [Lexical Scope in JavaScript – Beginners Guide - freeCodeCamp](https://www.freecodecamp.org/news/lexical-scope-in-javascript/)

> [Lexical Scope in JavaScript – What Exactly Is Scope in JS? - freeCodeCamp](https://www.freecodecamp.org/news/javascript-lexical-scope-tutorial/)

> [Lexical Scope in JavaScript - educative.io](https://www.educative.io/answers/lexical-scope-in-javascript)

> [JavaScript Variable Scopes - javascripttutorial.net](https://www.javascripttutorial.net/javascript-variable-scope/#)

<br/>
<br/>

# Closures in JS

Closures são funções que fazem referência a variáveis declaradas no seu escopo léxico, transformando-as em variáveis privadas e persistindo seus valores.

Qualquer função em JS pode ser uma closure.

```js
let name = `John`;

function greetings() {
  let message = `Hi`;
  console.log(`${message} ${name}`);
}

greetings(); // => 'Hi John'
```

JavaScript usa o escopo para o gerenciamento de acesso a variáveis.

Mesmo sendo declarada fora do escopo de greetings, no escopo global neste caso, a função tem acesso a variável name declarada no escopo mais externo.

Isso traz um problema, qualquer código na pagina tem acesso a name e pode alterar o seu valor sem a necessidade de executar greetings.

A variável message é local e só pode ser acessada dentro de greetings. Se tentarmos acessar de fora, obteremos um erro.

```js
function birthday(years) {
  let age = years;

  function celebrate() {
    const birthdayMessage = `Hoje é meu aniversário de ${age} anos.`;
    console.log(birthdayMessage);
  }
  celebrate();
}

birthday(35); // => Hoje é meu aniversário de 35 anos.
birthday(27); // => Hoje é meu aniversário de 27 anos.
```

Neste trecho de código, a função birthday cria uma variável local, age, e também a função celebrate, que por sua vez cria uma variável local, birthdayMessage.

celebrate é a função mais interna e só está disponível no escopo da função birthday.

A função celebrate tem acesso a variável age, definida em birthday, mas birthday nao tem acesso a birthdayMessage.

- Vamos modificar a função birthday.

```js
function birthday(name, years) {
  let person = name;
  let age = years;

  function celebrate() {
    const birthdayMessage = `Meu nome é ${person} e hoje é meu aniversário de ${age} anos.`;
    age++;
    console.log(birthdayMessage);
  }
  return celebrate;
}

const johnsBday = birthday('John', 35);
const juliasBday = birthday('Julia', 27);
```

```js
johnsBday(); // => 'Meu nome é John e hoje é meu aniversário de 35 anos'
juliasBday(); // => 'Meu nome é Julia e hoje é meu aniversário de 27 anos'
```

<img src="assets/img/oneyearlater.jpg" alt="One year later" width="300" height="150">

```js
johnsBday(); // => 'Meu nome é John e hoje é meu aniversário de 36 anos'
juliasBday(); // => 'Meu nome é Julia e hoje é meu aniversário de 28 anos'
```

Agora, em vez de executarmos a função celebrate dentro de birthday, birthday retorna o objeto de função celebrate.

Como em JS as funções são First-Class Citizens, podemos retornar uma função a partir de outra função.

Fora da função birthday, nós atribuímos à johnsBday e juliasBday o retorno de birthday com os seus respectivos argumentos.

Depois executamos as funções johnsBday e juliasBday.

Como já sabemos, em JS, uma variável local só existe durante o tempo de execução da função, isto é, ao fim da execução de birthday, as variáveis person e age não existem mais.

Neste caso, nós executamos johnsBday | juliasBday que faz referência ao retorno de birthday, que é a função celebrate, e as variáveis ainda persistem.

Uma Closure é uma função que preserva o escopo externo a ela, dentro do seu escopo.

> ## Sugestão de leitura

<br/>

> [Closures - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

> [Private Members in JavaScript - www.crockford.com](http://www.crockford.com/javascript/private.html)

> [JavaScript Closures - www.tutorialsteacher.com](https://www.tutorialsteacher.com/javascript/closure-in-javascript)

> [Javascript: Mas afinal, o que são closures? - Medium](https://medium.com/@stephanowallace/javascript-mas-afinal-o-que-s%C3%A3o-closures-4d67863ca9fc)

> [How to Use Closures in JavaScript – A Beginner's Guide - freeCodeCamp](https://www.freecodecamp.org/news/closures-in-javascript/)

# Hoisting

Quando um código JS é executado pala sua respectiva engine, ele cria um [contexto global de execução (global execution context)](https://www.javascripttutorial.net/javascript-execution-context/), e ele tem duas fases.

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

A referência para a variável myName é criada em memória, o escopo é definido, e seu valor é inicializado como undefined.

Caso tentemos acessar uma variável inicializada sem ter sido declarada antes, obtemos um erro de referência.

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

> ### let / const

<br>
Variáveis declaradas com let | const também são içadas, mas não são inicializadas com o valor padrão de undefined.

A leitura de variáveis declaradas com let e const antes da sua inicialização lança uma exceção.

```js
console.log(myName); // => ReferenceError

let myName = `John`;
```

```js
console.log(myAge); // => ReferenceError

const myAge = 35;
```

> ### Classes

- Class declaration

Uma diferença importante entre class declaration e function declaration é que enquanto uma função pode ser invocada antes da sua declaração, o mesmo não acontece com classes.

Declarações de classes sao içadas mas seus valores nao são inicializados.

```js
const p = new Pessoa(); // => ReferenceError

class Pessoa {}
```

> ### Function e Class expression

<br>

Function e class expressions tem o mesmo tratamento de declarações de variáveis. A função ou a classe será inicializada na fase de execução do contexto global de execução.

```js
console.log(myFunction); // => ReferenceError

const myFunction = function () {};
```

O mesmo acontece utilizando a sintaxe de Arrow functions

```js
console.log(myOtherFunction); // => ReferenceError

const myOtherFunction = () => {};
```

```js
console.log(myClass) // => ReferenceError

const myClass = class {
  constructor()
};
```

> ## Sugestão de leitura

<br/>

> [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

> [JavaScript Hoisting - W3Schools](https://www.w3schools.com/js/js_hoisting.asp)

> [JavaScript Hoisting - www.javascripttutorial.net](https://www.javascripttutorial.net/javascript-hoisting/)

> [O que é Hoisting em Javascript? - Medium](https://medium.com/devzera/o-que-%C3%A9-hoisting-em-javascript-b8c629178518)

> [JavaScript Hoisting - Programiz](https://www.programiz.com/javascript/hoisting)

# Value vs Reference Assignment

Em Js existem tipos que são copiados por valor e tipos que são copiados por referencia.

- Copiados por valor:

  - null
  - undefined
  - Number
  - String
  - Boolean

- Copiados por referência:
  - Object
  - Array
  - Function

Tipos primitivos são copiados por valor e objetos por referência.

> Copiados por Valor

```js
const name = `John`;

let leftHandFingers = 5;
let rightHandFingers = leftHandFingers;

console.log(leftHandFingers); // => 5
console.log(rightHandFingers); // => 5

let developer = true;
let frontEndDev = developer;

console.log(developer); // => true
console.log(frontEndDev); // => true
```

![](assets/img/values-in-memory.png)

Valores primitivos são armazenados na Stack.

Apesar de atribuirmos as variáveis leftHandFingers à rightHandFingers e developer à frontEndDev, o que é atribuído é o valor da variável, neste caso 5 e true, respectivamente.

Vamos imaginar que algo acontece com rightHandFingers. (bata na madeira)

```js
rightHandFingers--;

console.log(rightHandFingers); // => 4
console.log(leftHandFingers); // => 5
```

> Copiados por Referência

```js
const name = 'John'
const skills = ['cooking', 'dancing']
const features = {
  age: 35
  hair: 'grey',
  tall: true,
}
```

![](assets/img/values-in-memory-2.png)

Objetos em JS são armazenados em uma segunda área de alocação de memória, Heap.

As variáveis que recebem um objeto, na verdade recebem uma referência para um endereço dentro da Heap.

```js
const likes = skills;

console.log(likes); // => ['cooking', 'dancing']
console.log(skills); // => ['cooking', 'dancing']
```

![](assets/img/values-in-memory-3.png)

Caso uma variável, que tem como valor um objeto, seja atribuída a outra variável, o valor passado é a referência do endereço na Heap.

```js
likes.push('music');

console.log(likes); // => ['cooking', 'dancing', 'music']
console.log(skills); // => ['cooking', 'dancing', 'music']
```

![](assets/img/values-in-memory-4.png)

Como as duas variáveis apontam para o mesmo endereço no Heap, se fizermos uma alteração em likes, skills também é alterada.

```js
const skills = ['cooking', 'dancing', 'music'];
const likes = ['cooking', 'dancing', 'music'];

console.log(skills === likes); // => false
```

![](assets/img/values-in-memory-5.png)

Embora o "mesmo" array seja atribuído, uma referência é independente da outra. As variáveis apontam para endereços diferente no Heap.

> Copiando Objetos

```js
const person = {
  species: 'human',
  name: 'John',
  age: 35,
};

const spreadPerson = {
  ...person,
};

const jsonParsePerson = JSON.parse(JSON.stringify(person));

const objectAssignPerson = Object.assign({}, person);

// Object.assign ainda pode receber um terceiro parâmetro (objeto) que permite adicionar ou sobrescrever propriedades
const julia = Object.assign({}, person, {
  name: 'Julia',
  age: 27,
  hair: 'blue',
});

console.log(person); // => {species: 'human', name: 'John', age: 35}

console.log(spreadPerson); // => {species: 'human', name: 'John', age: 35}

console.log(jsonParsePerson); // => {species: 'human', name: 'John', age: 35}

console.log(objectAssignPerson); // => {species: 'human', name: 'John', age: 35}

console.log(julia); // => {species: 'human', name: 'Julia', age: 27, hair: 'blue'}
```

> Copiando Arrays

```js
const numbersArray = [2, 4, 6, 8, 10];

const spreadArray = [...numbersArray];

const sliceArray = numbersArray.slice();

const concatArray = [].concat(numbersArray);

console.log(numbersArray); // => (5) [2, 4, 6, 8, 10]

console.log(spreadArray); // => (5) [2, 4, 6, 8, 10]

console.log(sliceArray); // => (5) [2, 4, 6, 8, 10]

console.log(concatArray); // => (5) [2, 4, 6, 8, 10]
```

> ## Sugestão de leitura

<br/>

> [Back to roots: JavaScript Value vs Reference - Medium](https://medium.com/dailyjs/back-to-roots-javascript-value-vs-reference-8fb69d587a18)

> [Reference Vs Value - Most People Don't Understand This - Web Dev Simplified Blog](https://blog.webdevsimplified.com/2021-03/js-reference-vs-value/)

> [Quick Tip: How JavaScript References Work - sitepoint](https://www.sitepoint.com/how-javascript-references-work/#:~:text=The%20Bottom%20Line%20on%20JavaScript%20References&text=On%20variable%20assignment%2C%20the%20scalar,at%20other%20variables%2C%20or%20references.)

> [The Difference Between Values and References in JavaScript - dmitripavlutin.com](https://dmitripavlutin.com/value-vs-reference-javascript/)

> [JavaScript Primitive vs. Reference Values - www.javascripttutorial.net](https://www.javascripttutorial.net/javascript-primitive-vs-reference-values/)

> [How to get a grip on reference vs value in JavaScript - freeCodeCamp](https://www.freecodecamp.org/news/how-to-get-a-grip-on-reference-vs-value-in-javascript-cba3f86da223/)

> [3 Ways to Copy Objects in JavaScript - www.javascripttutorial.net](https://www.javascripttutorial.net/object/3-ways-to-copy-objects-in-javascript/)

> [Copying Objects in JavaScript - DigitalOcean](https://www.digitalocean.com/community/tutorials/copying-objects-in-javascript)

> [3 Ways to Copy or Clone Array in Javascript - https://holycoders.com](https://holycoders.com/javscript-copy-array/)

> [ES6 Way to Clone an Array - https://holycoders.com](https://www.samanthaming.com/tidbits/35-es6-way-to-clone-an-array/)

> [How to clone an array in JavaScript - freeCodeCamp](https://www.freecodecamp.org/news/how-to-clone-an-array-in-javascript-1d3183468f6a/)

> [How to differentiate between deep and shallow copies in JavaScript - freeCodeCamp](https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/)

> [Shallow copy - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy)

> [Deep copy - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Deep_copy)

> [What is shallow copy and deep copy in JavaScript ? - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-shallow-copy-and-deep-copy-in-javascript/)

> [JavaScript's Memory Management Explained - https://felixgerschau.com](https://felixgerschau.com/javascript-memory-management/)

# Destructuring

# Rest | Spread Operator

# this

# Currying

# Prototype

# async

```

```

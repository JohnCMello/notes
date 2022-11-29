# 6. Destructuring

A sintaxe do Destructuring Assignment é uma expressão em Js que nos permite extrair valores de arrays e propriedades de objetos e armazena-los em variáveis.

## 6.1. Arrays

```js
const technologies = ['JavaScript', 'HTML', 'CSS'];

/*
  js = technologies[0]
  html = technologies[1]
  css = technologies[2]
*/

const [js, html, css] = technologies;

console.log(js); // => 'JavaScript'
console.log(html); // => 'HTML'
console.log(css); // => 'CSS'
```

## 6.2. Objetos

```js
const person = {
  name: 'John',
  age: 35,
  developer: true,
};

/*
  name = person.name
  age = person.age
  developer = person.developer
*/

const { name, age, developer } = person;

console.log(name); // => 'John'
console.log(age); // => 35
console.log(developer); // => true
```

<br>

## 6.3. Exemplos

Se tentarmos atribuir um valor inexistente a uma variável, ela recebe o valor de `undefined`.

- Arrays

```js
const technologies = ['JavaScript', 'HTML', 'CSS'];

const [js, html, css, react] = technologies;

console.log(react); // => undefined
```

- Objetos

```js
const person = {
  name: 'John',
  age: 35,
  developer: true,
};

const { name, age, developer, tall } = person;

console.log(tall); // => undefined
```

<br>

Podemos extrair valores singulares e armazenar os valores restantes.

- Arrays

```js
const technologies = ['JavaScript', 'HTML', 'CSS', 'React', 'Angular', 'Vue'];

const [js, html, css, ...frameworks] = technologies;

console.log(js); // => 'JavaScript'
console.log(html); // => 'HTML'
console.log(css); // => 'CSS'
console.log(frameworks); // => (3) ['React', 'Angular', 'Vue']
```

- Objetos

```js
const person = {
  name: 'John',
  age: 35,
  developer: true,
  frontEndDev: true,
  company: 'Valtech',
};

const { name, age, ...workInfo } = person;

console.log(name); // => 'John'
console.log(age); // =>35
console.log(workInfo); // => {developer: true, frontEndDev: true, company: 'Valtech'}
```

<br>

Alguns valores podem ser ignorados.

- Arrays

Com o uso do operador `,` (vírgula | comma) .

```js
const technologies = ['JavaScript', 'HTML', 'CSS', 'React', 'Angular', 'Vue'];

const [js, , css, react] = technologies;

console.log(js); // => 'JavaScript'
console.log(css); // => 'CSS'
console.log(react); // => 'React'
```

Neste trecho de código os indices `[1]`, `[4]` e `[5]` não foram atribuídos a variáveis.

- Objetos

```js
const person = {
  name: 'John',
  age: 35,
  developer: true,
  frontEndDev: true,
  company: 'Valtech',
};

const { name, age, company } = person;

console.log(name); // => 'John'
console.log(age); // =>35
console.log(company); // => 'Valtech'
```

<br>

Variáveis podem ser declaradas e inicializadas posteriormente.

- Arrays

```js
const js,html, css, react, angular, vue

function webDevTechStack(){
  const technologies = ['JavaScript','HTML','CSS','React','Angular','Vue'];

  return technologies
}

[js,html, , , angular, vue] = webDevTechStack();

console.log(js); // => 'JavaScript'
console.log(react); // => undefined
console.log(vue); // => 'Vue'
```

- Objetos

```js
const name, age, frontEndDev;

const person = {
  name: 'John',
  age: 35,
  developer: true,
  frontEndDev: true,
  company: 'Valtech'
};

// a expressão precisa ser executada dentro de ()
({name, age,frontEndDev } = person)

console.log(name); // => 'John'
console.log(age); // => 35
console.log(frontEndDev); // => true
```

Expressão não executada:

```js
const name, age, frontEndDev;

const person = {
  name: 'John',
  age: 35,
  developer: true,
  frontEndDev: true,
  company: 'Valtech'
};

{name, age,frontEndDev } = person; // => Uncaught SyntaxError: Unexpected token '='
```

<br>

Valores padrão podem ser atribuídos.

- Arrays

```js
function webDevTechStack() {
  const technologies = ['JavaScript', 'HTML', 'CSS', 'React', 'Angular', 'Vue'];

  return technologies;
}

const [js, html, css, react, angular, vue, docker = 'Docker'] =
  webDevTechStack();

console.log(js); // => 'JavaScript'
console.log(angular); // => 'Angular'
console.log(docker); // => 'Docker'
```

- Objetos

```js
function webDev() {
  const person = {
    name: 'John',
    age: 35,
    developer: true,
    frontEndDev: true,
    company: 'Valtech',
  };

  return person;
}

const { name, age, frontEndDev, dogsName = 'Cacau' } = webDev();

console.log(name); // => 'John'
console.log(developer); // => true
console.log(dogsName); // => 'Cacau'
```

Valores padrão só podem ser atribuídos caso a variável não exista ou o seu valor seja `undefined`.
Quaisquer outros valores incluindo `null`, `false` ou `0` são ignorados pelo Default assignment.

- Arrays

```js
function webDevTechStack() {
  const technologies = ['JavaScript', 'HTML', 'CSS'];

  return technologies;
}

const [js = 'java_script', html, css] = webDevTechStack();

console.log(js); // => 'JavaScript'
```

- Objetos

```js
function webDev() {
  const person = {
    name: 'John',
    age: 35,
    developer: true,
  };
  return person;
}

const { name = 'Mello', age, developer } = webDev();

console.log(name); // => 'John'
```

<br>

Se as funções `webDevTechStack` e `webDev` retornassem outro valor que não um array ou um objeto, neste cenário onde `array` & `objeto` são esperados, uma exceção seria lançada.

```js
function webDevTechStack() {
  const technologies = null;
  return technologies;
}

function webDev() {
  const person = null
  return person;


const [js, html, css] = webDevTechStack(); // => Uncaught TypeError: webDevTechStack is not a function or its return value is not iterable

const {name, age, developer} = webDev(); // => Uncaught TypeError: webDev is not a function or its return value is not iterable
```

A sintaxe de Destructuring também se aplica a arrays e objetos aninhados.

- Arrays

```js
const person = ['John', 'Mello', ['JavaScript', 'HTML', 'CSS']];

const [firstName, lastName, [...technologies]] = person;

console.log(firstName); // => 'John'
console.log(lastName); // => 'Mello'
console.log(technologies); // => (3) ['JavaScript', 'HTML', 'CSS']
```

- Objetos

```js
const person = {
  name: 'John',
  age: 35,
  workInfo: {
    developer: true,
    frontEndDev: true,
    company: 'Valtech',
  },
};

const {
  name,
  age,
  workInfo: { ...workInfo },
} = person;

console.log(name); // => 'John'
console.log(age); // => 'Mello'
console.log(workInfo); // => {developer: true, frontEndDev: true, company: 'Valtech'}
```

A sintaxe de Object Destructuring ainda nos permite definir aliases para os valores obtidos a partir do objeto original com o uso do operador `:` (dois pontos | colon).

```js
function webDev() {
  const person = {
    name: 'John',
    age: 35,
    developer: true,
  };
  return person;
}

const { name: firstName, age, developer } = webDev();

console.log(firstName); // => 'John'
```

> ## Sugestão de leitura

<br/>

> [Destructuring assignment - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

> [ES6 Destructuring Assignment - javascripttutorial.net](https://www.javascripttutorial.net/es6/destructuring/)

> [JavaScript Object Destructuring - javascripttutorial.net](https://www.javascripttutorial.net/es6/javascript-object-destructuring/)

> [How to Use Array and Object Destructuring in JavaScript - freeCodeCamp](https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/)

> [Destructuring in JavaScript – How to Destructure Arrays and Objects - freeCodeCamp](https://www.freecodecamp.org/news/destructuring-patterns-javascript-arrays-and-objects/)

> **[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

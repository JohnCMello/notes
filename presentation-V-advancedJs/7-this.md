# 7. this

A palavra chave `this` não é pertinente somente ao JavaScript. Outras linguagens de programação como Java, C#, e PHP utilizam deste artifício com o intuito de representar a instância atual da classe. Entretanto em JavaScript ela se comporta de maneira diferente.

Como `this` é determinado em Runtime, quando uma função é executada, a sua referência pode variar dependendo do que acontece no código.

Em geral, `this` referencia o objeto do qual a função é uma propriedade, o objeto que executa a função naquele determinado momento, e não necessariamente o objeto onde a função foi definida.

`this` sempre faz referência a um objeto e não a uma função. Embora ele seja definido no momento da execução da função, a sua referência é do objeto.

```js
const webDev = {
  name: 'John',
  age: 35,
  developer: true,
  birthday: function () {
    return ++this.age;
  },
};

webDev.birthday(); // => 36
```

Dentro da função `birthday`, `this` faz referência a propriedade `age` do objeto `webDev`.

<br>

## 7.1. this no contexto Global

No contexto Global `this` faz referência a `window` nos browsers, ou a `global` no Node.js

```js
console.log(this === window); // => true

console.log(this); // => Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

Caso uma propriedade seja atribuída a `this` em contexto Global, ela será atribuída ao objeto global.

```js
this.globalString = 'Global string';

console.log(window.globalString); // => Global string
```

<br>

## 7.2. this no contexto Funcional

Em Js podemos executar uma função de quatro maneiras, ou seja, quatro modos de vincularmos, "binding", `this`

- Function invocation | Execução de função (Default binding)
- Method invocation | Execução de método (Implicit binding)
- Indirect invocation | Execução indireta (Explicit binding )
- Constructor invocation | Execução de Construtor (Constructor Call Binding)

### 7.2.1. Default Binding

Caso um função que foi definida no escopo global contenha uma referência a `this` dentro do seu escopo, no momento da sua execução, `this` é vinculado ao objeto global.

```js
var walterWhite = 'Heisenberg';

function sayMyName() {
  console.log(`You're ${this.walterWhite}`);
}

sayMyName(); // => "You`re Heisenberg"
```

`sayMyName` é uma função simples definida e no escopo global, isso faz com que `this`, neste contexto, referencie o objeto global.

Lembrando que variáveis definidas com a palavra chave `var` são atribuídas ao objeto global.

```js
console.log(this.walterWhite); // => "Heisenberg"

console.log(window.walterWhite); // => "Heisenberg"
```

Variáveis definidas com `let` ou `const` não são atribuídas ao objeto global.

```js
let walterWhite = 'Heisenberg';
//const walterWhite = 'Heisenberg';

function sayMyName() {
  console.log(`You're ${this.walterWhite}`);
}

sayMyName(); // => "You`re undefined"
```

Caso o nosso código seja escrito em strict mode, o que é aconselhável sempre, utilizando a diretriz `'use strict'` no inicio do código ou dentro da função, `this` não é vinculado ao objeto global e o código lança uma exceção.

```js
// 'use strict'

var walterWhite = 'Heisenberg';

function sayMyName() {
  'use strict';
  console.log(`You're ${this.walterWhite}`);
}

sayMyName(); // => Uncaught TypeError: this is undefined
```

### 7.2.2. Implicit Binding

Quando invocamos um método de um objeto utilizando a notação de ponto (`.`), `this` é vinculado ao objeto que invoca o método no momento da execução.

```js
var walterWhite = {
  name: 'Walter White',
  alias: 'Heisenberg',
  sayMyName,
};

function sayMyName() {
  console.log(`You're ${this.alias}`);
}

walterWhite.sayMyName(); // => "You`re Heisenberg"
```

Neste trecho de código, quando executamos `walterWhite.sayMyName()`, `this` é vinculado ao objeto `walterWhite` e `sayMyName()` é capaz de resolver a propriedade `alias`.

```js
var walterWhite = {
  name: 'Walter White',
  alias: 'Heisenberg',
  sayMyName,
  partner: {
    name: 'Jesse',
    alias: `Cap 'n Cook`,
    sayMyName,
  },
};

function sayMyName() {
  console.log(`You're ${this.alias}`);
}

walterWhite.partner.sayMyName(); // => "You`re Cap 'n Cook"
```

No trecho de código acima o método `sayMyName` é executado a partir de `partner`, e `this` é implicitamente vinculado a `partner` ao invés de `walterWhite`.

Uma maneira simples de descobrir a qual objeto `this` é vinculado, é analisar qual objeto está a esquerda da notação de ponto (`.`).

```js
var walterWhite = {
  name: 'Walter White',
  alias: 'Heisenberg',
  sayMyName,
  partner: {
    name: 'Jesse',
    alias: `Cap 'n Cook`,
    sayMyName,
  },
};

function sayMyName() {
  console.log(`You're ${this.alias}`);
}

walterWhite.sayMyName(); // => "You`re Heisenberg" | this é vinculado a walterWhite
walterWhite.partner.sayMyName(); // => "You`re Cap 'n Cook" | this é vinculado a partner
```

### 7.2.3. Explicit Binding

Vamos fazer algumas alterações no nosso código:

```js
var walterWhite = {
  name: 'Walter White',
  alias: 'Heisenberg',
};

var jessePinkman = {
  name: 'Jesse Pinkman',
  alias: `Cap 'n Cook`,
};

function sayMyName() {
  if (this.alias !== 'Heisenberg') {
    console.log(`You're ${this.name}`);
    return;
  }
  console.log(`You're ${this.alias}`);
}
```

Criamos dois objetos independentes `walterWhite` e `jessePinkman`, isolamos a função `sayMyName` e adicionamos uma verificação.

Agora, se executarmos a função `sayMyName`, `this` está vinculado ao objeto global.

Para vincularmos `this` explicitamente, podemos utilizar os métodos `call()`, `apply()` e `bind()` existentes no `Function prototype`

### 7.2.3.1. call() e apply()

Os métodos `call()` e `apply()` tem basicamente a mesma implementação, a diferença são as suas assinaturas.

```js
Function.prototype.call(thisReference, arg1, ..., argN)

Function.prototype.apply(thisReference, [...args])
```

Tanto `call()` quanto `apply()` recebem como primeiro argumento o objeto que será a referência para `this`, mas `call()` necessita que os demais argumentos da função sejam passados individualmente, e `apply()` recebe um array de argumentos.

```js
// call()
var teacher = {
  firstName: 'Walter',
  lastName: 'White',
};

var student = {
  firstName: 'Jesse',
  lastName: 'Pinkman',
};

function sayCatchPhrase(aka, catchPhrase) {
  console.log(
    `${this.firstName} ${this.lastName}, AKA: ${aka}, says: ${catchPhrase}`
  );
}

sayCatchPhrase.call(teacher, 'Heisenberg', 'Say my name!'); // => Walter White, AKA: Heisenberg, says: Say my name!

sayCatchPhrase.call(student, "Cap'n Cook", 'Yeah Science, Bitch!'); // => Jesse Pinkman, AKA: Cap'n Cook, says: Yeah Science, Bitch!
```

```js
// apply()
var teacher = {
  firstName: 'Walter',
  lastName: 'White',
};

var heisenberg = ['Heisenberg', 'Say my name!'];

var student = {
  firstName: 'Jesse',
  lastName: 'Pinkman',
};

var capNCook = ["Cap'n Cook", 'Yeah Science, Bitch!'];

function sayCatchPhrase(aka, catchPhrase) {
  console.log(
    `${this.firstName} ${this.lastName}, AKA: ${aka}, says: ${catchPhrase}`
  );
}

sayCatchPhrase.apply(teacher, heisenberg); // => Walter White, AKA: Heisenberg, says: Say my name!

sayCatchPhrase.apply(student, capNCook); // => Jesse Pinkman, AKA: Cap'n Cook, says: Yeah Science, Bitch!
```

### 7.2.3.2. bind()

Os métodos `bind()` e `call()` tem assinaturas semelhantes - ambos recebem como primeiro argumento o objeto referencia para `this` e os demais argumentos pertinentes a função são passados individualmente. A diferença é que `bind()` retorna uma nova função que será executada.

```js
//bind()
var teacher = {
  firstName: 'Walter',
  lastName: 'White',
};

var student = {
  firstName: 'Jesse',
  lastName: 'Pinkman',
};

function sayCatchPhrase(aka, catchPhrase) {
  console.log(
    `${this.firstName} ${this.lastName}, AKA: ${aka}, says: ${catchPhrase}`
  );
}

var walterWhiteCatchPhrase = sayCatchPhrase.bind(
  teacher,
  'Heisenberg',
  'Say my name!'
);

var jessePinkmanCatchPhrase = sayCatchPhrase.bind(
  student,
  "Cap'n Cook",
  'Yeah Science, Bitch!'
);

walterWhiteCatchPhrase(); // => Walter White, AKA: Heisenberg, says: Say my name!

jessePinkmanCatchPhrase(); // => Jesse Pinkman, AKA: Cap'n Cook, says: Yeah Science, Bitch!
```

### 7.2.4. Constructor Call Binding

Quando executamos uma função com a palavra chave `new`, também conhecida como `constructor function`, um novo objeto é criado; Este novo objeto é a referência para `this`

```js
function Character(firstName, lastName) {
  (this.firstName = firstName), (this.lastName = lastName);
}

var walterWhite = new Character('Walter', 'White');

var jessePinkman = new Character('Jesse', 'Pinkman');

console.log(walterWhite); // => Character {firstName: 'Walter', lastName: 'White'}

console.log(walterWhite); // => Character {firstName: 'Jesse', lastName: 'Pinkman'}
```

## 7.3 this e eventos HTML

Em `handlers` de eventos HTML, `this` é vinculado ao elemento que recebe o evento.

```html
<button onclick="console.log(this)">Click Me!</button>
```

Resultado no console quando o botão é clicado:

```
"<button onclick='console.log(this)'>Click Me!</button>"
```

Podemos alterar alguma regra CSS do botão, por exemplo:

```html
<button onclick="this.style.color='teal'">Click Me!</button>
```

Cuidado ao atribuir uma função ao evento que contenha `this` dentro da função:

```html
<!-- index.html -->
<button onclick="changeColor()">Click Me!</button>
```

```js
// index.js
function changeColor() {
  this.style.color = 'teal';
}
```

O código acima não terá o resultado esperado pois `this`, dentro da função `changeColor`, foi vinculado (binding) ao objeto global `window`, no modo `'non-strict'`, e não ao elemento do evento HTML.

> ## Sugestão de leitura

<br/>

> [Binding in JS explained - Medium](https://medium.com/codex/binding-in-js-explained-4a2481a0b01a)

> [The JavaScript this Keyword + 5 Key Binding Rules Explained for JS Beginners - freeCodeCamp](https://www.freecodecamp.org/news/javascript-this-keyword-binding-rules/)

> [What Does 'this' Mean in JavaScript? The this Keyword Explained with Examples - freeCodeCamp](https://www.freecodecamp.org/news/what-is-this-in-javascript/)

> [Demystifying the JavaScript this Keyword - javascripttutorial.net](https://www.javascripttutorial.net/javascript-this/)

**[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

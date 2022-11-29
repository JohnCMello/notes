# 9. async

JS por natureza é assíncrono, ou seja, ele executa as declarações uma a uma, linha a linha, do inicio, parte superior, ao fim do código.

```js
console.log('First log');
console.log('Second log');
console.log('Third log');

// => 'First log'
// => 'Second log'
// => 'Third log'

function secondLog(fn) {
  console.log('Second log');
  fn();
}

function thirdLog() {
  console.log('Third log');
}

function allLogs() {
  console.log('First log');
  secondLog(thirdLog);
}

allLogs();

// => 'First log'
// => 'Second log'
// => 'Third log'
```

Quando executamos uma função, somente quando todo a função é executada e/ou um valou seja retornado, é que a proxima função na fila sera executada. Isso faz com que o programa pare o seu fluxo de execução e aguarde o resultado da função atual.

Um modelo assíncrono nos permite realizar varias atividades simultaneamente.

Alguns exemplos de `Asynchronous JavaScript` são: Data fetching, chamadas à APIs backend, carregamento de arquivos, timers e intervalos.

## 9.1. Callbacks

Uma das abordagens para a programação assíncrona em JS é o uso de `Callback Functions`.

Uma `Callback Function` é uma função que é passada como argumento para outra função, e quando alguma ação é finalizada, essa função é executada com o resultado da ação anterior.

um exemplo clássico de uma `Callback Function` é a função passada para `setTimeout` como primeiro argumento.

`Callback` é só uma convenção de nomenclatura para o uso de funções JS. Não existem uma propriedade ou um método de código nativo JS que seja chamado `callback`.

```js
function callback() {
  const message = 'From the callback function';
  console.log(message);
}

setTimeout(callback, 2000);

// Após 2 segundos...

// => 'From the callback function'

function showMessage() {
  const message = 'From the showMessage function';
  console.log(message);
}

setTimeout(showMessage, 2000);

// Após 2 segundos...

// => 'From the showMessage function'
```

A função `callback` é executada após uma ação finalizar. No exemplo acima, a ação é a espera de 2000 mili-segundos.

```js
function logs() {
  console.log('First log');

  setTimeout(function callback() {
    console.log('Second log');
  }, 3000);

  console.log('Third log');
}

logs();
```

Qual o resultado desta operação?

```js
// A

// => 'First log'
// => 'Second log'
// => 'Third log'

// B

// => 'First log'
// => 'Third log'
// => 'Second log'
```

Resposta: B

Como `setTimeout` é uma função assíncrona, o fluxo do programa nao é interrompido. `'First log'` e `'Third log'` são respectivamente mostrados no console, seguindo as diretrizes síncronas do programa, e logo que `setTimeout` executa a sua `Callback Function` , neste caso após 3 segundos, `'Second log'` é mostrado.

Outro exemplo comum de `Callback Functions` é o das funções passadas para os eventos HTML.

```html
<!-- index.html -->
<button id="button">Click me</button>
```

```js
// index.js
const button = document.querySelector('#button');

button.addEventListener('click', clickHandler, null);

function clickHandler() {
  console.log('Button clicked');
}
```

Neste contexto a função `clickHandler`, que foi passada como argumento para `addEventListener`, é a função que será executada quando a ação do click do botão for realizada.

## 9.2. Callback Hell / Callback Pyramid of Doom

Dentro do universo de `Callback Functions`, vamos imaginar que temos uma sequencia de funções , onde cada uma irá executar um determinado trecho de código, de acordo com o o resultado da execução da função anterior, e assim sucessivamente.

```js
function makeBelieveCallbackHell() {
  setTimeout(function outer() {
    console.log(
      'outer function, this function did something and finished after 3 seconds'
    );
    setTimeout(function middle() {
      console.log(
        'middle function, this function did something and finished after 2 seconds'
      );
      setTimeout(function inner() {
        console.log(
          'inner function, this function did something and finished after 1 second'
        );
        //the list could go on and on
      }, 1000);
    }, 2000);
  }, 3000);
}

makeBelieveCallbackHell();
```

Neste exemplo temos somente 3 níveis de `callbacks`, mas com um fluxo maior onde mais funções serão executadas, a leitura ou manutenção desse código poderia se tornar algo difícil.

## 9.3. Promises

A função construtora `Promise` foi introduzida no ES6.

Uma `Promise` tem três estados: `<pending>`, `<fulfilled>` e `<rejected>`

> `<pending>` é o estado inicial. A promise foi criada.

> `<fulfilled>` é o estado quando a operação é realizada com sucesso e contem um valor.

> `<rejected>` é o estado quando a operação falha por alguma razão

### 9.3.1. Criando uma Promise

Um objeto `Promise` é criado usando a palavra-chave `new` e a função construtora `Promise()`. Ela recebe por parâmetro uma função, `executor`, que será executada pelo construtor, que por sua vez recebe duas funções como parâmetro, que serão executadas nos casos de sucesso, `resolve`, e em casos de erro, `reject`.

```js
const promise = new Promise(function (resolve, reject) {
  if (success) {
    resolve(value);
  } else {
    reject(error);
  }
});
```

Uma vez que a `Promise` é `fulfilled` ou `rejected`, ela permanece nesse estado. Qualquer outra implementação dentro de uma `Promise` será ignorada.

### 9.3.2. Consumindo uma Promise

```js
const tripTicket = (arrivalTime) => {
  return new Promise((resolve, reject) => {
    const successTrip = {
      greetings: `You've made it! Have a great trip.`,
      departureTime: '08:15',
    };
    const failureTrip = {
      greetings: `Too bad! Maybe next time.`,
      departureTime: '08:15',
    };
    setTimeout(() => {
      if (arrivalTime <= '08:00') {
        resolve(successTrip);
      }
      reject(failureTrip);
    }, 2000);
  });
};
```

Fazendo uma analogia entre uma `Promise` e uma passeio na Carreta Furação, a `Promise` é o bilhete, se voce chegar a tempo, voce teve sucesso, a sua `Promise` foi cumprida, `<fulfilled>`, caso contrario, por alguma razão você não conseguiu chegar a tempo, a sua `Promise` é recusada, `<rejected>`.

`setTimeout` é o nosso figurante que permite a função `trip` ser assíncrona.

Neste cenário, nós chegamos até o local da Carreta Furação as `'07:45'`.

```js
tripTicket('07:45').then((message) => console.log(message));

// => {greetings: "You've made it! Have a great trip.", departureTime: '08:15'}
```

Os casos de sucesso de uma `Promise`, são tratado no método `.then()`.

Os casos de erro são tratados no `.catch()`

```js
tripTicket('08:05')
  .then((message) => console.log(message)) //refer to the resolve method
  .catch((error) => console.error(error)); //refer to the reject method

// => {greetings: 'Too bad! Maybe next time.', departureTime: '08:15'}
```

Um exemplo real de `Promise` é a API `fetch`

```js
fetch('https://www.breakingbadapi.com/api/characters/1')
  .then((response) => response.json()) // .json() retorna uma Promise
  .then((data) => console.log(data))
  .catch((err) => console.error(error));
```

Uma `Promise` pode ser encadeada em outra. O que é conhecido como `promise chaining`

## 9.4. async - await

As palavras chave `async` e `await` foram introduzidas no ECMAScript 2017. Elas são `syntactic sugar` para `Promise`.

Para definirmos uma função assíncrona, adicionamos `async` antes da palavra chave `function` ou de uma arrow function `()=>{}`

Uma função que retorne uma `Promise`, tem o seu retorno, seja ele `resolve` ou `reject`, 'aguardado' com `await`.

```js
// function declaration
async function asyncAction() {
  const asyncTrip = await tripTicket('08:00');
  console.log(asyncTrip);
}

// function expression
const asyncAction = async function () {
  const asyncTrip = await tripTicket('08:00');
  console.log(asyncTrip);
};

// arrow function
const asyncAction = async () => {
  const asyncTrip = await tripTicket('08:00');
  console.log(asyncTrip);
};

// The function tripTicket, from the previous example, returns a Promise
```

Anteriormente ao ES2020 só era possível utilizar `await` dentro de funções `async`, mas uma nova implementação desta funcionalidade permite o uso de `top-level await`.

```js
const asyncTrip = await tripTicket('08:00');
console.log(asyncTrip);
// => {greetings: "You've made it! Have a great trip.", departureTime: '08:15'}
```

Se uma `Promise` é resolvida, `await` retorna o resultado, caso contrario um(a) erro/exceção é lançado(a).

> ## Sugestão de leitura

<br/>

> [Introducing asynchronous JavaScript - MDN](hhttps://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)

> [Asynchronous JavaScript – Callbacks, Promises, and Async/Await Explained - freecodecamp.org](https://www.freecodecamp.org/news/asynchronous-javascript-explained/)
>
> [JavaScript Promises - www.javascripttutorial.net](https://www.javascripttutorial.net/es6/javascript-promises/)
>
> [async function - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

**[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

# 2. Closures in JS

Closures são funções que fazem referência a variáveis declaradas no seu escopo léxico, transformando-as em variáveis privadas e persistindo seus valores.

Qualquer função em JS pode ser uma closure.

JavaScript usa o escopo para o gerenciamento de acesso a variáveis.

```js
let name = `John`;

function greetings() {
  let message = `Hi`;
  console.log(`${message} ${name}`);
}

greetings(); // => 'Hi John'
```

Mesmo sendo declarada fora do escopo de greetings, no escopo global neste caso, a função tem acesso a variável `name` declarada no escopo mais externo.

Isso traz um problema, qualquer código na página tem acesso a `name` e pode alterar o seu valor sem a necessidade de executar `greetings`.

A variável `message` é local e só pode ser acessada dentro de `greetings`. Se tentarmos acessa-la de fora, obteremos um erro.

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

Neste trecho de código, a função `birthday` cria uma variável local, `age`, e também a função `celebrate`, que por sua vez cria uma variável local, `birthdayMessage`.

`celebrate` é a função mais interna e só está disponível no escopo da função `birthday`.

A função `celebrate` tem acesso a variável `age`, definida em `birthday`, mas `birthday` não tem acesso a `birthdayMessage`.

Vamos modificar a função `birthday`.

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

Agora, em vez de executarmos a função `celebrate` dentro de `birthday`, `birthday` retorna o objeto de função `celebrate`.

Como em JS as funções são First-Class Citizens, podemos retornar uma função a partir de outra função.

Fora da função `birthday`, nós atribuímos à `johnsBday` e `juliasBday` o retorno de `birthday` com os seus respectivos argumentos.

Depois executamos as funções `johnsBday` e `juliasBday`.

Como já sabemos, em JS, uma variável local só existe durante o tempo de execução da função, isto é, ao fim da execução de birthday, as variáveis `person` e `age` não existem mais.

Neste caso, nós executamos `johnsBday` | `juliasBday` que faz referência ao retorno de `birthday`, que é a função `celebrate`, e as variáveis ainda persistem.

Uma Closure é uma função que preserva o escopo externo a ela dentro do seu escopo.

> ## Sugestão de leitura

<br/>

> [Closures - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

> [Private Members in JavaScript - crockford.com](http://www.crockford.com/javascript/private.html)

> [JavaScript Closures - tutorialsteacher.com](https://www.tutorialsteacher.com/javascript/closure-in-javascript)

> [Javascript: Mas afinal, o que são closures? - Medium](https://medium.com/@stephanowallace/javascript-mas-afinal-o-que-s%C3%A3o-closures-4d67863ca9fc)

> [How to Use Closures in JavaScript – A Beginner's Guide - freeCodeCamp.org](https://www.freecodecamp.org/news/closures-in-javascript/)

**[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

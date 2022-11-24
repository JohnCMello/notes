# 5. Spread Operator | Rest Parameters

Os operadores `Spread` e `Rest` tem sintaxe idêntica,`...`, mas eles diferem em funcionalidade.

A principal diferença entre eles é que o Rest Parameters é utilizado para armazenar o resto de uma lista de valores ou argumentos de uma função em um Array e o Spread Operator é utilizado para expandir iteráveis em valores individuais.

## 5.1. Spread Operator

- Arrays

```js
const frameworks = ['React', 'Angular', 'Vue'];

const technologies = ['HTML', 'CSS', 'JavaScript', ...frameworks];

console.log(frameworks); // => (6) ['HTML', 'CSS', 'JavaScript','React', 'Angular', 'Vue']
```

- Objetos

```js
const workInfo = {
  developer: true,
  frontEndDev: true,
  company: 'Valtech',
};

const johnDev = {
  name: 'John',
  age: 35,
  ...workInfo,
};

console.log(johnDev); // => {name: 'John', age: 35, developer: true, frontEndDev: true, company: 'Valtech'}
```

- Strings

```js
const message = 'Hello World!';

const letters = [...message];

console.log(letters); // => ['H', 'e', 'l','l' ,'o' ,' ' ,'W', 'o', 'r' ,'l' ,'d' , '!']
```

## 5.2. Rest Parameters

```js
function sum(...numbers) {
  return numbers.reduce((acc, cur) => acc + cur, 0);
}

console.log(sum(2, 4, 6, 8, 10)); // => 30
```

```js
function petOwner(person, ...pets) {
  console.log(`Olá, meu nome é ${person}`);
  console.log(
    `Eu tenho ${pets.length} ${pets.length <= 1 ? 'pet.' : 'pets.'} `
  );
  console.log(
    `${pets.length <= 1 ? 'O nome dele(a) é:' : 'Os nomes deles(as) são:'} `
  );

  for (const pet of pets) {
    console.log(pet);
  }
}

petOwner('John', 'Cacau');
// => 'Olá, meu nome é John'
// => 'Eu tenho um pet.'
// => 'O nome dele(a) é:'
// => 'Cacau'
```

> ## Sugestão de leitura

<br/>

> [Rest parameters - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

> [JavaScript: Operadores Rest e Spread - devMedia.com.br](https://www.devmedia.com.br/javascript-operadores-rest-e-spread/41200)

> [JavaScript Rest vs Spread Operator – What’s the Difference? - freeCodeCamp](https://www.freecodecamp.org/news/javascript-rest-vs-spread-operators/)

> [Rest parameters and spread syntax - javascript.info](https://javascript.info/rest-parameters-spread)

> [An Easy Guide to Object Rest/Spread Properties in JavaScript - dmitripavlutin.com](https://dmitripavlutin.com/object-rest-spread-properties-javascript/)

> [Spread operator vs rest parameters - Medium](https://medium.com/trainingcenter/spread-operator-vs-rest-parameters-f8688d8e1761)

> [Como funcionam o Rest e o Spread Operator- horadecodar.com.br](https://www.horadecodar.com.br/2019/03/19/como-funcionam-o-rest-e-o-spread-operator/)

> [Spread e Rest operators em JavaScript - blog.cod3r.com.br](https://blog.cod3r.com.br/spread-rest/)

> [What is the rest parameter and spread operator in JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-the-rest-parameter-and-spread-operator-in-javascript/)

> [JavaScript Rest Parameters - javascripttutorial.net](https://www.javascripttutorial.net/es6/javascript-rest-parameters/)

> [JJavaScript Object Spread - javascripttutorial.net](https://www.javascripttutorial.net/es-next/javascript-object-spread/)

**[⬆ Voltar para o índice](./README.md#javascript---advanced-concepts)**

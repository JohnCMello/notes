Redux
=====
>Video

[Redux Tutorial - Learn Redux from Scratch](https://www.youtube.com/watch?v=poQXNp9ItL4)

>Artigo

[You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)
___
Functional Programing
=====================

Programação funcional é um paradigma de programação.
Foi criada nos anos 50.

A programaçÀo funcional se resume em decopor um problemas e pequenas funções reutilizáveis que recebem um input e retonam um output, um resultado. Elas não alteram dados.

Com essa estrutura podemos contruir funções mais complexas.

### Benefícios

* Mais conciso
* Debug mais fácil
* Fácil de testar
* Escalável

#### Algumas linguagens funcionais

* Clojure
* Haskell
* JavaScript (multi-paradigma)

Functions as First-class Citizens
=================================

Funções em JS sáo tratadas como qualquer outra variável.

Podem ser: 

* Atribuidas à variáveis
* Passadas como argumentos para outras funções.
* Retornadas de outras funções

```js
function sayHello(){
  return 'Hello'
}

const func = sayHello;

func() // => 'Hello'
sayHello() // => 'Hello'

function greet(funcMessage){
  console.log(funcMessage())
}

greet(sayHello) // => 'Hello'

function sayHelloAgain(){
  return function(){
    return 'Hello Again'
  }
}

const fn = sayHelloAgain() // => function()
const helloAgain = fn() // => 'Hello Again'
```

High-Order Functions
====================

Uma HOF é uma função que recebe uma função como argumento e/ou que retorna uma função.

```js
function sayHelloAgain(){
  return function(){
    return 'Hello Again'
  }
}

const fn = sayHelloAgain() // => function()
const helloAgain = fn() // => 'Hello Again'
```

>O método de Array .map() é uma HOF, por ela recebe uma função como argumento.
```js
let numbers = [1,2,3]
numbers.map((number)=>{ number + 1 }) // => [2,3,4] 
```

>SetTimeout() é outro exemplo de uma HOF.
```js
setTimeout(() => console.log('Hello HOF'), 2000)
```

Function Composition
====================

Imagine que temos o seguinte código:

```js
let input = ' JavaScript   ' 
let outout = '<div>' + input.trim() + '</div>'
```

Como resolver esse problema com programação funcional?

```js
// remover espaços da string
// envolver em uma div
let input = ' JavaScript   ' 

const trim = str => str.trim()
const wrapDiv = str => `<div>${str}</div>`
const toLowerCase = str => str.toLowerCase()

const firstResult = wrapDiv(trim(input)) // => <div>'JavaScript'</div>
const secondResult = wrapDiv(toLowerCase(trim(input))) // => <div>'javaScript'</div>
```

Composing and Piping
====================

### Lodash
>Uma biblioteca de utilitários JS

```js
import {compose, pipe} from 'lodash/fp' 

let input = ' JavaScript   ' 

const trim = str => str.trim()
const wrapDiv = str => `<div>${str}</div>`
const toLowerCase = str => str.toLowerCase()

const firstResult = wrapDiv(toLowerCase(trim(input))) // => <div>'javaScript'</div>

const firstTransform = compose(wrapDiv,toLowerCase,trim) 
firstTransform(input)// => <div>'javaScript'</div>

const secondTransform = pipe(trim, toLowerCase, wrapDiv)
secondTransform(input)// => <div>'javaScript'</div>
```

Currying
========

```js
import {compose, pipe} from 'lodash/fp' 

let input = ' JavaScript   ' 

const trim = str => str.trim()
const toLowerCase = str => str.toLowerCase()
const wrap = (type, str) => `<${type}>${str}</${type}>`

const transform = pipe(trim, toLowerCase, wrap)

// pipe constroi um pipeline onde output de cada função é o input da função seguinte.
// o output the toLowerCase é passado para wrap como primeiro parametro.
// wrap recebe o retorno de toLowerCase como type e str = undefined

transform(input)// => '<javaScript>undefined</javaScript>'

const transform = pipe(trim, toLowerCase, wrap('div'))
// caso wrap fosse executada recebendo 'div' como parametro, causaria erro.
// pois essa chamada retornaria uma string.
// não podemos passar strings para pipe
// só podemos passar funções para pipe
// não podemos contruir um pipeline com algumas funções e uma string
// não faz sentido
```

Como resolver este problema usando Currying?

```js

function add(a,b) {
  return a + b
}
```
Currying é uma técnica que nos permite converter uma função que tem N argumentos em uma função que contem um unico argumento.

(...args) => (arg) => (arg)

```js
function add(a){
  return function(b){
    return a + b
  };
}

add(1)(3) // => 4

const add2 = a => b => a + b;
const add2 = (a) => (b) => a + b;

add2(3)(2) // => 5
```

Voltando ao Problema anterior...

```js
import {compose, pipe} from 'lodash/fp' 

let input = ' JavaScript   ' 

const trim = str => str.trim()
const toLowerCase = str => str.toLowerCase()
const curryedWrap = (type) => (str) =>`<${type}>${str}</${type}>`

const transform = pipe(trim, toLowerCase, curryedWrap('div'))

// o retorno de toLowerCase é passado para dentro de curriedWrap
// const curryedWrap = ('div') => (toLowerCase()) =>`<${div}>${javascript}</${div}>`

transform(input)// => '<div>javascript</div>'
```

Pure Functions
==============

Dizemos que uma função é pura quando sempre que executamos esta função com os mesmos argumentos
obtemos sempre o mesmo resultado.

```js
function randomNumber(number){
  return number * Math.random()
}

// a função randomNumber nao é uma função pura.
// ela sempre vai retornar um resultado aleatorio

function doubleNumber(number) {
  return number * 2
}

// a função doubleNumber é uma função pura
// ela sempre vai retornar o mesmo resultado quando executada com o mesmo argumento
```

### O que no fazer com uma função pura:

* Passar valores aleatórios
* Passar data/horario atual
* Passar Estado Global ( DOM, arquivos, DB, etc...)
* Mutar os parametros
  
Na pratica nem tudo tem que ser puro.

Quando desenvolvendo com Redux, temos que nos certifcar que os nossos Reducers sejam puros. Outras funções podem ser impuras.

```js
function isEligible(age){
  return age > minAge
}

// minAge nao é definido na função, e é uma variavel externa.
// esse valor de minAge pode variar no futuro e o retorno da nossal função também


function isEligible(age, minAge){
  return age > minAge
}

// agora a função recebe minAge como parametro.
```

### Benefícios

* Auto-documentação
* Facil de testar
* Simultaneidade (Concurrency)
* Cacheable (armazenamendo de resultado computado)


Immutability
============

>Once created, cannot be changed

Uma vez que um objeto é criado, não podemos alterar este objeto.

Caso seja preciso fazer alguma alteração, criamos uma copia deste objeto e alteramos esta copia.

```js
let name = 'John'
let newName = name.toUpperCase()

// strings em JS, bem como em outras linguagens de programação são imutaveis.
// se temos uma string e tentamos trnaformar para uppercase, recebemos uma nova string.
// a string original nao é alterada.

const book = {}
book.title = 'Some title'

console.log(book) // => {title: 'Some title'}

// em contrapartida, em JS objetos podem ser alterados diretamente
// Objetos e Arrays em JS não são imutáveis
```

### Por que imutabilidade: 

* Previsibilidade
* Fácil detecção de mudanças
* Simultaneidade (Concurrency)

### Por que não:

* Performance
* Sobrecarga de memória

> Existem libs que amenizam a sobrecarga de memoria 
> 
> >Structural sharing.
> 
> >Se valores são comuns entre alguns objetos, eles nao são copiados, eles são compartilhados.
________
### Updating Objects
_________
```
```
Object.assign()

```js
const person = { name: 'John' }

console.log(person) // => { name: 'John' }

const updatedPerson = Object.assign({}, person, { name:'Peter', age: 30 })

console.log(updatedPerson) // => { name:'Peter', age: 30 }
```
```
```
Spread Operator
```js
const person = { name: 'John' }

console.log(person) // => { name: 'John' }

const updatedPerson = {...person, name:'Peter', age: 30}

console.log(updatedPerson) // => { name:'Peter', age: 30 }
```
Temos que tomar cuidado quando pois ambos os metodos ( Object.assign() | Spread Operator ) criam uma Shallow Copy do objeto.

```js
const person = { 
  name: 'John',
  address: {
    country: 'Brasil',
    city: 'Florianópolis'
  }
}

console.log(person) 

// => { 
//  name: 'John',
//  address: {
//    country: 'Brasil',
//    city: 'Florianópolis'
//  }
//}
```
Aqui vamos copiar todo o conteúdo de person, e reatribuir o valor da propriedade *name* para *Peter*

```js
const updatedPerson = {...person, name:'Peter'}

console.log(updatedPerson) 

// => { 
//  name: 'Pedro,
//  address: {
//    country: 'Brasil',
//    city: 'Florianópolis'
//  }
//}

updatedPerson.address.city = 'Macapá'

console.log(person) 

// => { 
//  name: 'John',
//  address: {
//    country: 'Brasil',
//    city: 'Macapá'
//  }
//}

console.log(updatedPerson) 

// => { 
//  name: 'Pedro,
//  address: {
//    country: 'Brasil',
//    city: 'Macapá'
//  }
//}
```

Tanto em ***person*** quanto em ***updatedPerson*** o valor de ***adress.city*** foi alterado pois foi feito uma ***Shallow Copy***, e os dois apontam para a mesma referencia de ***adress*** em memória.

```js
const person = { 
  name: 'John',
  address: {
    country: 'Brasil',
    city: 'Florianópolis'
  }
}

const updatedPerson = {
  ...person, 
  name:'Pedro',
  address: {
    ...person.address,
    city: 'Macapá'
  }
}

console.log(person) 

// => { 
//  name: 'John',
//  address: {
//    country: 'Brasil',
//    city: 'Florianópolis'
//  }
//}

console.log(updatedPerson) 

// => { 
//  name: 'Pedro,
//  address: {
//    country: 'Brasil',
//    city: 'Macapá'
//  }
//}
```

Desta forma fazemos uma copia de ***address***, alteramos a cópia e nao referência original. 

Updating Arrays
===============

```js 

//adicionando elementos

const numbers = [1,2,3]

const addedFourEnd = [...numbers, 4] 
console.log(addedFourEnd) // => [1,2,3,4]

const addedFourBeginning = [4,...numbers] 
console.log(addedFourEnd) // => [4,1,2,3]

//caso queira adicionar o numero em uma posição específica

const index = numbers.infexOf(2)
const addFourBeforeTwo = [
  ...numbers.slice(0, index),
  4,
  ...numbers.slice(index)
] 
console.log(addFourBeforeTwo) // => [1,4,2,3]


//removendo elementos

const removeTwo = numbers.filter(n => n !== 2) 
console.log(removeTwo) // => [1,2,3,4]

//alterando elementos

const doubleNumbers = numbers.map(n => n * 2)
console.log(doubleNumbers) // => [2,4,6]

const changeTwoForTwenty = numbers.map(n => n === 2 ? 20 : n)
console.log(changeTwoForTwenty) // => [1,20,3]
```

Enforcing Mutability
====================

JS não previne mutação de objetos pois nao é uma linguagem puramente funcional.


```js
let bookHP = {title:'Harry Potter'}

function published(book) {
  return book.isPublished = true
}

published(bookHP)

console.log(bookHP) // => {title:'Harry Potter', isPublished:true}
```

Criamos o obj ***book*** e adicionamos a propriedade ***title*** com o valor de ***Harry Potter***, criamos uma função que vai adicionar a propriedade ***isPublished*** com o valor de ***true*** a um livro que é recebido por parametro, executamos a função ***published***, e apresentamos o valor de ***bookHP*** no console
  

Para isso existem libs que oferencem estruturas de objetos imutáveis.
  
  * Immutable
  * Immer
  * Mori
_______________
_______________
_______________
## Immutable.js

```js
import { Map } from 'immutable'

let bookHP = Map({title:'Harry Potter'})

console.log(bookHP) // => Map { size: 1, _root:ArrayMapNode{...},...}

console.log(bookHP.get("title")) // => 'Harry Potter'

console.log(bookHP.toJS()) // => {title:'Harry Potter'}

function published(book) {
  return book.set(isPublished, true)
}

const bookHPIsPublished = published(bookHP)

console.log(bookHPIsPublished.toJS()) // => {title:'Harry Potter', isPublished:true}
```
__________

## Immer

```js
import { produce } from 'immer'

let bookHP = {title:'Harry Potter'}

function published(book) {
  return produce(book, (draftBook)=>{
    draftBook.isPublished = true
  })
}

const bookHPIsPublished = published(bookHP)

console.log(bookHPIsPublished) // => {title:'Harry Potter', isPublished:true}
```
_____
Redux Architecture
=====



























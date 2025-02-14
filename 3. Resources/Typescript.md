---
date: 2025-02-04
tags:
  - topic/webdev
---
## Sin escribir tipos

```ts
const a = 1
const b = 2

const c = a + b
```

## Con objetos

```ts
const obj = {
  name: "Joseph",
  age: 22,
};

obj.name = "Juan";
```

## Los tipos básicos de TS

- number
- string
- boolean
- null
- undefined
- symbol
- bigint

## Inferencia de datos

TS detecta automáticamente el tipo de dato

## Tipos especiales: any

> [!danger]
> Nunca usar, any es un tipo especial que simplemente  no tiene tipo. Desactiva el type-checking

## Funciones

```ts
function saludar(name: string) {
  console.log("Hola " + name.toUpperCase());
}
```

### Ejemplo con un objeto

```ts
function saludar(person: { name: string; age: number }) {
  console.log("Hola " + person.name.toUpperCase());
}
```

### Anotaciones de retorno

```ts
function saludar(name: string): string {
  return `Hola ${name}`;
}
```

### Funciones como parametros

```ts
const sayHiFromFunction = (fn) => {
	return fn('Miguel')
} 
```

> [!danger]
> No usar function es muy general

```ts
const sayHi = (name: string) => `Hola, ${name}`

const sayHiFromFunction = (fn: (name:string) => string) =>
{
	return fn('Miguel')
}
```

### Tipando arrow functions

```ts
const sumar: (a: number, b: number) => number = (a, b) => {
	return a + b;
};

const restar = (a: number, b: number): number => {
	return a - b;
};
```

### Funciones que no devuelven nada `void`: 

```ts
const logError = (errorMessage: string) => {
console.error(`ERROR: ${errorMessage}`)
}
```

### `never`, cuando nunca debe devolver nada

```ts
function throwError(mensaje: string): never 
{
	if(mensaje)
	{
		throw new Error(mensaje);
	}

	throw new Error('Error general');
}
```

### Funciones anónimas 

Tipado contextual. Por eso infiere los tipos.

```ts
const avengers = ['Spidey', 'Hulk', 'IronMan']

avengers.forEach(function (s){
	console.log(s.toUpperCase());
})

avengers.forEach((s) => {
	console.log(s.toUpperCase());
});
```

### La inferencia de los tipos no siempre funciona con los parámetros

```ts
function suma(a, b){
	return a + b
}

suma(2, 2)
```

## Objetos
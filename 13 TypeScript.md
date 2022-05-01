# TypeScript

http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/14-Typescript.pdf

---

`29/04/2022`

## Js strict mode
```js
"use strict"
```

- unica direttiva riconoscituta da js che deve essere messa a inizio del codice o della funzione e permette di <u>generare errori per rendere più rigida js</u>
- errori generati quando:
	- assegno
		- una prop non scrivibile
		- una prop getter-only
		- una prop che non esiste
		- una variabile che non esiste (cioè non specificata con `var` o `let`)
		- un oggetto che non esiste
	- faccio `delete` di variabili, funzioni o undeletable oggetti
	- uso parole vietate
	- duplicazione di parametri `f(p,p)`
	- uso di operatore `with`


## Polyfilling and Transpiling
- **Polyfilling**: pezzo di codice (o plugin) che permette a unvecchio browser di avere le caratteristiche di una nuova versione js che voglio usare
	- fornendo queste nuove caratteristiche come funzioni utilizzabili
- **<u>Transpiler</u>** : meccanismo che trasforma codice scritto con una nuova sintassi in codice vecchio equivalente
	- processo che permette di scrivere codice in Typescript e poi tradurlo in js che può essere eseguito dal browser

## TypeScript
- superinsieme di js che permette di scrivere codice in modo rigido e seguendo le caratteristiche dei linguaggi tradizionali e poi di traspilarlo in js
	- <u>codice in typescrip transpilato in codice js</u>
- <u>posso specificare la versione js in cui traspilarlo</u>
	- in questo modo posso anche usare costrutti come classi, interfacce, ... specificate da typescript e poi transpilarli nella prima versione js (3) in modo tale che siano compatibili con qualsiasi browser
- permette di
	- **specificare tipo dellle variabili**
	-  **definire classi e interfacce**
- possiamo sempre includere codice js in codice typescript
- quando faccio transpyler e c'è un errore per typescript ma non per js
	- transpyler segnala eventuale errore
	- ma la traspilazione viene fatta comunque e generata codice js
		- perché in js sarebbe corretto
- (diverso dal compilatore tradizionale che segnalerebbe l'errore ma non farebbe traduzione)
- ex
```typescript
var x:number = 5;
x = "pippo"

// transpilazione segnala l'errore
// ma genera comunque codice js
	// perché in js non sarebbe un errore (non ho tipizzazione)
```

## Variable typing
**name:type**

```typescript
var id:number = 221255;
var aname:string = "Dorothea";
var tall:boolean = true;
var names:string[] = ['pippo','pluto','minnie'];
```

## Function typing
**name:type**

```typescript
function sum(a:number, b:number):number
{
	var temp:numer = a+b;
	return temp;
}
```

## Number of params
- anche in js è possibile passare un numero di parametri che non matcha con quello della funzione
	- quelli che mancano sono sostituiti con NaN
- con typescript viene segnalato un errore

## Optional params

^8bda66

- possibile specificare parametri opzionali con `x?:number`
	- ?
- devono essere messi per ultimi
- ex
```typescript
function sum(a:number,b:number,c?:number):number
{
	var total = 0;
	if(c!==undefined) {
		total += c;
	}
	total += (a+b);
	return total;
}

var temp = sum(7,8);
document.write("temp="+temp);
```

## Default params
- specificando un valore di default, se quell'argomento non viene passato, prende quel valore
- ex ..

## Rest params
- possiamo definire una funzione con un numero infinito di param

```typescript
function sum(...numbers:number[]):number
{
	var tot:number = 0;
	for(var i=0; i< numbers.length; i++)
		tot += numbers[i];
	return tot;
}
```

- posso anche specificare prima dei paramentri normali (singoli)  e mettere alla fine il rest params, che prenderà tutti gli argomenti passati dopo quelli che fanno riferimento ai singoli e li metterà nell'array

## Dynamic type variables
- se voglio trattare una variabile in modo dinamico sui tipi (come avviene in js) : **any**

```typescript
var temp:any = 3;

temp = 'a';
temp = [23,5,23];
temp = true;
temp = new Object();
```

## Classes
```typescript
class Car {
	//field
	engine:string;

	//constructor
	constructor(engine:string) {
		this.engine = engine
	}
	
	//function
	disp():void {
		console.log("Engine is : "+this.engine)
	}
}
```

## Constructor
- la keyword è **constructor( ... )**
	- non il nome della classe
- di default viene creato il costruttore vuoto
- se ne definisco un altro questo viene sovrascritto
- **non c'è polimorfismo dei costruttori**
	- se voglio gestire un numero variabile di paramentri : [[#^8bda66|Optional params]]
- possiamo specificare i modificatori di accesso:
	- `public` accessibili a tutti
	- `protected` accessibili alla classe e sottoclassi
	- `private` accessibili a classe
- **nelle sottoclassi il super costruttore non è chiamato automaticamente**
	- necessario fare super(...)
	- (vedi dopo)


## Instance vars and methods
- variabili di solito dichiarate prima del costruttore
- metodi dichiarati <u>senza</u> keyword function
	- possiamo specificare un modificatore di accesso
	- possiamo specificare il valore di ritorno

```typescript
class Rectangle
    {
    private width:number;
    private height:number;
    
    constructor(width:number,height:number)
    {
        this.width = width;
        this.height = height;
    }

    protected area():number
    {
        return this.width*this.height;
    }
}
```

## Static vars
- possiamo definire variaibli e metodi statici con `static`
- accedere a questi è possibile usando nome della classe


## Class inheritance
- ereditarietà con `extends`

```typescript
class Shape {
  Area: number;
  constructor(a: number) {
    this.Area = a;
  }
}

class Circle extends Shape {
  disp(): void {
    console.log("Area of the circle: " + this.Area);
  }
}

var obj = new Circle(223);
obj.disp();

```

## Type assertions
- è come un cast ma senza check particolari
- `<cast>`

```typescript
class Person {
  id: number;
  name: string;
}
class Student extends Person {
  average: number;
}

var a: Person = new Student();
var b: Student = <Student>a; //<-

```

## Generics
```typescript
function identity<T>(arg: T): T { return arg; }
```

- uso:
	- forma esplicita
	```typescript
	let output = identity("myString"); // ^ = let output: string
	```
	- forma implicita
	```typescript
	let output = identity("myString"); // ^ = let output: string
	```

## Note sulle Classi
- **typescript non supporta ereditarietà multipla di classi**
- **nelle sottoclassi il super costruttore non è chiamato automaticamente**
	- necessario esplicitare `super()`
		- con i parametri che devono matchare
- **classi implementano le interfacce**
	```typescript
	class Student implements Iprintable {…}
	``` 


## Interfaces
- possiamo usare interfacce con definizione di tipo di dato
- <u>in js spariscono</u>

```typescript
interface IPerson
{
	name:string,
	surname:string,
	welcome: () => string
}

var client:IPerson = {
	name : "Marco",
	surname : "Rossi",
	welcome: ():string => {return "Hello"}
}
```

- **c'è ereditarietà multipla di interfacce**


## Duck inheritance
- metodo per verificare se oggetti hanno stesso comportamento
- e quindi possono essere usati uno al posto dell'altro
- ex
```typescript
class Vehicle {
  public run(): void {
    console.log("Vehicle.run");
  }
}
class Task {
  public run(): void {
    console.log("Task.run");
  }
}
function runTask(t: Task) {
  t.run();
}

runTask(new Task());
runTask(new Vehicle());
```
- oggetti di tipo Task e Vehicle hanno stesso comportamento
	- un veicolo è un task e viceversa

### Evitare Duck inheritance
```typescript
class Vehicle {
  private x:string = "A"
  public run(): void {console.log("Vehicle.run");}
}
class Task {
  private x:string = "A"
  public run(): void {console.log("Task.run");}
}
```

- ora non posso piu assegnare a paramentro di tipo Vehicle un oggetto di tipo Task o viceversa perché la variabile A si riferisce alla singola classe (è private)
- altro modo
	- uso variabili con nome diverso
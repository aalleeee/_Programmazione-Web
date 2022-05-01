# JavaScript Objects
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/12-JS-objects.pdf

---

## Javascript objects
- collezione di nomi valori e metodi
- di fatto sono delle **mappe**

```js
var person = {firstName:"Dorothea", lastName:"Wierer", birthyear:1990, fullName:function() {return this.firstName + " " + this.lastName;}};
```

## Accesso alle proprietà
- `person.firstName`
- `person["firstName"];`
- `var x="firstName"; person[x];`

## Dynamic management of objects
- possiamo aggiungere nuove proprietà ad oggetti eseistenti e rimuoverne

```js
var person = {firstName:"Dorothea", lastName:"Wierer", birthyear:1990}; 

person.birthplace="Brunico;

delete person.firstName;
```

## Other ways to create objects
```js
var object1 = new Object();
Object.defineProperty(
	object1, 'name', {
	value: "AA",
	writable: false
});
```

ora se faccio `object1.name = 77;` non ho errori ma non viene cambiato il valore perché writable è false

## Objects constructors
- template per creare oggetti di un certo tipo
- permette di avvicinarci al concetto di classe
- **funzione costruttore di js è versione di concetto di classe**

esempio 

```js
function Rectangle(w, h) {
	this.width=w;
	this.height=h;
	this.area=function(){return this.width*this.height}
}
```

ora posso fare:

```js
a=new Rectangle(3,4);
a.area()    // => 12
a.width     // => 3
```


Conseguenze:
- con questo approccio io posso riscrivere le funzioni che non sono variabili di istanza
- quindi ogni "oggetto" potrebbe avere la sua funzione riscritta
- questo porta al fatto che devo allocare memoria per la funzione per ogni oggetto
	- la funzione è nel codice ...
- dall'esempio:
```js
a=new Rectangle(2,3);
b=new Rectangle(2,3);
p(a.area(),b.area()); // <- 6:6
a.area=function(){return this.width*5} // <-----
p(a.area(),b.area()); // <- 10:6
```

Viene quindi introdotto il concetto di **prototypes** per avere un comportamento simile a `static`


## Prototypes
```js
Rectangle.prototype.area=function(){
	return this.w*this.h
}
```

- il Prototype è un **namespace separato condiviso tra tutte le istanze dello stesso costruttore/classe**
- esempio:
```js
function Person(first, last ) {
	this.firstname = first;
	this.lastname = last;
}
Person.prototype.fullname= function () {
	return this.firstname+" "+this.lastname
};
Person.prototype.nickname="The Best"; // <-
let person1 = new Person('Dorothea', 'Wierer');
let person2 = new Person('Valentino','Rossi');
console.log(person1.nickname); /// -> The Best
console.log(person2.nickname); /// -> The Best
Person.prototype.nickname="The Champion"; // <-
console.log(person1.nickname); /// -> The Champion
console.log(person2.nickname); /// -> The Champion
```

## Inspecting objects
- posso ciclare sulle proprietà di un oggetto

```js
x = new Rectangle(3,4);
for (e in x) { // loop over all properties
	console.log(e+" : "+x[e]); // e: nome propr - x[e] valore della prop
}
```

output:
```sh
-   "width : 3"
-   "height : 4"
-   "area : function(){ return this.width*this.height}"
```

- se vogliamo considerare solo prop della classe stessa e non protoptipi (che invece sono condivisi)
	- `getOwnPropertyNames()`


## Inheritance con prototipi
- `Object.create()`
	- **crea un nuovo oggetto usandone uno esistente come prototipo per il nuovo** 
- ESEMPIO SLIDE 19
- c'è una ricerca per livelli delle proprietà
	- `person1` -> `Person` -> `Object`
- sorta di ereditarietà
- SLIDE 21-22-23
1. "sottoclasse" chiama il costruttore della "superclasse" nel suo costruttore
```js
function Athlete(first, last, sport ) {
	Person.call(this,first,last); // <----
	this.sport=sport;
}
```
2. sottoclasse lega suo prototipo a quello della superclasse
```js
Athlete.prototype = Object.create(Person.prototype);
```

e cosi a cascata ...
- slide 24


**Importante**
- `Person.prototype.nickname="The Best";`
	- setta i nickname di tutte gli oggetti di tipo Person
- ma se faccio `person1.nickname="The Magic";`
	- allora person1 scrive nickname come nuova proprietà
	- quindi risalendo la scala delle proprietà ora il suo nickname viene trovato all'interno dell'oggetto stesso, non del prototipo
	- quindi il suo nickname cambia (quello degli altri oggetti Person no)
- se facessi `delete person1.nickname` tornerebbe ad avere valore del prototipo
- slide 25

Esempio **slide 26**

## Predefined objects
... 


## Classi
- javascript ha introdotto nel 2015 le classi come **zuccherro sintattico** dei meccanismi basati sui prototipi
- dichiarazione di classi NON è hoisted !

esempi
```js
class Person {
	constructor(first, last ) {
		this.firstname = first;
		this.lastname = last;
	}
	fullname() {
		return this.firstname+" "+this.lastname;
	}
}

class Athlete extends Person {
	constructor(first, last, sport ) {
		super(first,last);
		this.sport=sport;
	}
}
```


IMPORTANTE:
- le istanze di classe possono essere  definite **solo** dentro i metodi
- variabili di classe possono essere definite ma **non chiamate su istanze**

esempio
```js
Person.nickname="The Champion";
let person1 = new Athlete('Dorothea', 'Wierer','Biathlon');
let person2 = new Athlete('Valentino','Rossi','Moto GP');
console.log(person1.fullname()+" "+person1.sport+" "+Person.nickname);
console.log(person2.fullname()+" "+person2.sport+" "+person2.nickname); //<---
```
OUTPUT:
- Dorothea Wierer Biathlon The Champion
- Valentino Rossi Moto GP **undefined**

```js
Person.nickname="The Champion";
Person.prototype.nickname="The Best"; //<------
let person1 = new Athlete('Dorothea', 'Wierer','Biathlon');
let person2 = new Athlete('Valentino','Rossi','Moto GP');
console.log(person1.fullname()+" "+person1.sport+" "+Person.nickname);
console.log(person2.fullname()+" "+person2.sport+" "+person2.nickname);
```
OUTPUT:
- Dorothea Wierer Biathlon The Champion
- Valentino Rossi Moto GP **The Best**


Slide 35
- metodi di classe possono essere definiti ma non chiamate su istanze



## Array
vanno considerati come mappe
- SPARSI
- NON OMOGENEI
- ASSOCIATIVI

```js
a=[];

a[0]=3; a[1]=“hello”; a[10]=new Rectangle(2,2);

a.length(); // -> 11; questo perché prende l'indice numerico max, non occputa veramente 11 blocchi contigui di memoria ma è una mappa: è sparsa
```

```n
a[“ name ”]=“Jaric” ;
stessa cosa di
a.name="Jaric";
```

Classe Array permette di
- add/remove elemento all'inizio
- add/remove elemento alla fine
- add/remove elemento tramite/da indice
- fare copia di array
- trovare indice
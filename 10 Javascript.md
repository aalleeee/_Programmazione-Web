# Javascript
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/11-JS-base.pdf

---
## Javasript
- problema di base: non posso mandare eseguibili (bytecode) perché non so hardware che c'è sotto.
- JavaScript **permette di eseguire codice dal cliente indipendente da hw**
	- inviando eseguibili che sono interpretati dal cliente
		- basta avere interprete JavaScript in browser
	- scrivere codice sorgente che mandiamo al browser che lo eseguirà
- quindi è linguaggio interpretato
- problema: client può vedere codice sorgente
	- problemi di sicurezza
	- non usare js per sicurezza
- codice js generalemente non è toccato dal server, arriva al client
	- con jsp o php invece il server vede il codice e lo sostituisce con l'output
	- quindi client non vede codice in questi casi
- **JavaScript è event-based**
	- UiEvent


## Storia
...

## Comparison operators
- `===` equal value and equal type
- `==` equal value (trasforma tipo se necessario)
- esempio
	- `'7' === 7` no
	- `'7' == 7` si

## Data types
- non c'è dichiarazione di tipi
- tipi:
	- primitivi
		- number, string, boolean, undefined
	- complessi
	- loosely, dynamic typed
- operatori
	- `typeof()`
	- `instanceof()`
- javascript ha anche Object()
	- hanno variabili e metodi
- rappresentazione di un frammento di html come JavaScript object : **DOM**


---
Esempi da slide 17

## Output
esempio slide 20
- innerHTML
	- Full HTML content
- innerText
	- Text only, CSS aware
- textContent
	- Text only, CSS unaware


## String() and string
- stringe sia
	- oggetti della classe String
	- che dato primitivo string
- posso chiamare metodi su tipo dato primitivo string perché internamente viene fatta conversione in Object String
	- esempio 
	```js
	var s = "Ciao"
	var q = new String("Ciao")
	```
	```js
	s === q : no
	s == q  : si 
	```


## Operatore +
- funzionamento operatore +
	1. conversione
		1. ogni oggetto viene convertito in valore primitivo
		2. se uno dei due operatori è stringa -> operatore non stringa convertito in stringa
		3. se rimangono bool -> convertiti in interi (true = 0, false = 1)
		4. se rimangono null -> 0
		5. se rimangono undefined -> Nan
		(ogni operazione tra un numero e Nan restituisce Nan)
	2. esecuzione
		1. se sono entrambi stringa -> concatenazione
		2. se sono entrambi numeri -> somma

- **esempi slide 31**

## Operatore *
- converte in numero implicitamente
- **slide 32**

## + as unary operator
- Unary + can be used to convert string to number
```js
var y = "5";              // y is a string
var x = + y;              // x is a number (5)
var y = "Pippo";          // y is a string
var x = + y;              // x is a number (NaN)
```

## Funzioni
- non specificano data type per param
- non controllano numero di argomenti
	- quelli mancanti sono Nan
	- possono essere messi di default
- c'è hoisting delle dichiarazioni
- nomi delle funzioni possono essere usati come paramenti
```js
var x = function (a, b) {return a * b};
product=x(2,3);
```
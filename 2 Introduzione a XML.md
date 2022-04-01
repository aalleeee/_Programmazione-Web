# Introduzione a XML
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/01-XML.pdf

`09-03-22`

## Markup language
- XML è un markup language (no programming language)
	- permette di annotare documenti associando metadati del documento (tag associati a info)
	- sintatticamente distinguibile dal testo (info vera)
	- è human readable

## Tipi di markup languages 
1. presentation markup
	- usato da sistemi di procession parole: codice binario associato a documento di testo
		- ex documento Word
	- questa decrizione difficile da capire da umano
	- di solito non è visibile ne da lettore ne da autore
	- dicono come devono essere le cose ?
2. procedural markup
	- markup è inserito nel resto per fornire istruzioni a programmi per processare testo
	- si processa testo da inizio alla fine eseguendo istruzioni che si trovano
		- comandi eseguibili
	- ex PostScript
3. descriptive markup
	- etichette per annotare dati che passiamo
		- non servono per azioni o descrivere aspetto
		- ma per dire che cosa sono le parti del documento

## SGML 
SGML: standard per definire linguaggi di markup
documenti SGML devono avere 3 parti:
- contenuto: del documento (+metadati)
- grammar: 
	- definizione del linguaggio
	- grammatica del linguaggio
		- DTD: data type definition
- stylesheet:
	- come si deve rendendere il contenuto

## XML
- eXtensible Markup Language
- gruppo di tecnologie che si aggregano a idea di markup language definito da SGML
- idea originale: avere venditore indipendente da formattazione dei dati nel contesto
- gruppo:
	- le due grammatiche di XML sono DTD e XML Schema
	- XSL è styleshett
	- XSLT agente che applica XSL
	- XQL ling di query su domini XML
	- XHTML
	- ...

## Applicazioni XML
- semantic web
- web service
- file di configurazione

## Elementi, contenuti e attributi XML
- tag
	- xml permette di definire i nostri tag
- elemento
	- contenuto tra tag di apertura tag di chiusura
- contenuto 
	- testo tra i tag
- attributo
	- opzioni dell'elemento


## Documenti ben formati
- un doc XML deve essere well-formed
- regole
	- tag sono case sensitive
	- devono esserci tag di apertura e chiusura
	- elementi devono essere annidati correttamente
	- trattare bene elementi vuoti
	- elementi vuoti possono contenere attributi
	- no caratteri di markup 
	- tutti gli attributi vanno quotati ( " " o ' ' )
	- documento XML debe avere tag di root
		- **struttura ad albero**

## Struttura logica docuemnti XML
...

## Namespace
- evitare conflitti di tag
- dichiaro namespace come attributo dell'elemento di root

## Parser
- preprocessa un documento XML
- check se è well-formed
	- se lo è fornisce struttura dati (contenuto xml e dati come albero)
- API per fare parsing
	- tree-based API: lavora con DOM
	- event-based API: lavora su eventi

## DTD
- definire le grammatiche

## Documento valido
- doc XML è valido se è conforme a una data grammatica
	- doc non valido è comunque un doc XML
- ci sono validator parsing
	- oltre a parsing normale
	- check su validazione
	- 
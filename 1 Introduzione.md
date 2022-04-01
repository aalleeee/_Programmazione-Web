# Introduzione
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/00-IntroductionToCourse.pdf

---
`04/03/22`

## Protocollo
- sinonimo di etichetta
- insieme di regole che determinano come i dati vengono trasmessi nelle telecomunicazioni e nelle reti
- definisce
	- **formato** dei messaggi
	- **ordine** dei messaggi
	- **azioni** prese durante la trasmissione e/o ricezione

## Porta
- punto finale di comunicazione in un sistema operativo
- un processo associa canali di input/output, tramite socket Internet, con protocollo di trasporto, a numero della porta e a indirizzo Ip
	- **binding**

## HTTP
- protocollo web livello applicazione
- modello client/server
- usa TCP (la connessione viene chiusa alla fine)
- **stateless** (no info delle richieste passate)

## URI, URL, URN
- risorsa web, o semplicemente una risorsa, è qualsiasi cosa identificabile, digitale, fisica o astratta
- **Uniform Resource Identifier (URI)**: sequenza compatta di caratteri che identificano una risorsa astratta o fisica
- **Uniform Resource Locator (URL)**: si riferisce a sottoinsieme di URI che identificano risorse con *rappresentazione dei loro meccanismi di accesso primario* (ex location di rete)
- **Uniform Resource Name (URN)**: si riferisce a sottoinsieme di URI che *devono rimanere uniche globalmente e persistenti, anche quando le risorse smettono di esistere* o diventa indisponibile

I promemoria nella serie di documenti **RFC** contengono note tecniche e organizzative su Internet.

note:
- sia l'URL che l'URN sono URI.
- un URN identifica una risorsa
- un URL fornisce un metodo per trovarlo.
- un URN può essere paragonato al nome di una persona,
- un URL può essere paragonato al loro indirizzo.
- un URN può essere associato a più URL

esempi slide 30 - 31


## Struttura URI
- URI = scheme:[//authority]path[?query][#fragment]
- authority = [userinfo@]host[:port]

slide 33 - 34

## MIME
Multipurpose Internet Mail Extensions
- se URI ci danno info su nome e location, metadati per loro tipo di contenuto
- tipi: MEDIA_TYPE/SUBTYPE
	- text -> text/plain, text/html, text/richtext … 
	- image -> image/jpeg, image/png, image/svg+xml… 
	- audio -> audio/basic, audio/ogg, audio/x-wav… ù
	- video -> video/mp4, video/ogg… 
	- application -> application/x-apple-diskimage…
	- multipart

## Client Server
- **server**: macchina che apre una connessione *SocketServer* e aspetta chiamate in arrivo
- **client**: macchina che inizia una connessione (aprendo una Socket al server) e chiede un servizio
- **sono ruoli sw** (non concetti hd) 

## Il modello
evoluzione da statici a dinamici
- alcuni URL sono associati ad **azioni** e contengono **parametri** per l'azione
- web server 
	- capisce quali URL sono dinamici
	- parsa i paramentri
	- inizia processo corrispondente all'azione che serve
	- ottiene dati dal processo
	- passa dati al client



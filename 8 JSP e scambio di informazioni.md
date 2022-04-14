# 8 Scambio informazioni e JSP
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/08-JSP.pdf

---

`01/04/2022`

## Scambio informazioni WebApp
![webAppInformationSchema](webAppInformationSchema.png)
- le informazioni di ogni utente sono memorizzate tramite sessione
	- una sessione per un utente
- tutto quello che non riguarda un utente specifico è nelle servlet
	- ogni servlet ha sue funzioni e istanze
	- una servlet per tutti gli utenti
	- servlet possono comunicare con `forward` e `include`
	- cioè `request`
- poi c'è parte condivisa con tutti utenti e tutte servlet : `ServletContext`
	- tutti utenti e tutte servlet
	- `getServletConfig().getServletContext`


## Thread safe
- istanze devono essere sempre in blocco synchronized
	```java
	public synchronized void increase(){
		count++;
		timeStamp = Calendar.getInstance();
	}
	```
	oppure dentro la doGet()
	```java
	synchronized (this) { counter.increase(); }
	```
- anche variabili statiche
- request e response sono safe
- variabili locali sono safe


## JSP
- permette di usare in modo semplice html
- e inserire codice java con carattere speciale `<% %>`
- e poi generare automaticamente codice della servlet

Elementi sintattici di jsp
- `<%@ directives %>` : interagire con il container con comandi apposti (tomcat) 
	- `<%@ page language=java session=true %>` : specifica linguaggio e se la servlet userà sessioni, ci torneremo perché il true può dare problemi con creazione di sessioni prima creazione di servlet (isNew() darà sempre false)
		- se non c'è session = false nelle direttive allora viene sempre creata nuova sessione nella jsp
		- se è false in quella servlet non avrò session
	- `<%@ page import=java.awt.*,java.util.* %>` : importare librerie
	- `<%@ page errorPage=URL %>` : specifica l'url della PageError a cui andare in caso di errori
	- `<%@ page isErrorPage=true %>` : specifica che questa è la PageError
- `<%! declarations %>` : blocco usato per definire classi, variabili e metodi generali, tutto quello che è fuori dalla doGet()
- `<% scriptlets %>` : codice java in service
- `<%= expression %>` : calcola espressione
	- `<%= x %>` stampa la variabile x


## JSP Lifecycle
- problema delle pagine jsp è che devono essere compilate, poi generano codice servlet, poi si compila la servlet e poi vanno in memoria
	- processo lungo
- ma che deve essere fatto solo una volta
- slide 26


`06/04/2022`

## Jsp e Servlet
- molto codice: servlet
- molto html: jsp

## JSP pages
- dentro a cartella webapps ci sono tutte le app del server
	- quindi myApp
		- file jsp e file html
		- cartella WEB-INF
			- file web.xml : configurazione
	- non posso uscire da myApp (che è come htdocs)
	- è possibile evitare di accedere a una jsp link prima di passare per le homepage (cioè no link a pagina prima di passare da home) 
		- si nascondono jsp dentro la WEB-INF che è directory protetta (che contiene info della webapp)
		- quando tomcat vede url che contiene radice webinf, vieta accesso
			- cosi sono accessibili da dentro il codice ma non direttamente.

## JSP Predefined Objects
- `out` : Writer
- `response` : HttpServletRequest
- `request` : HttpServletResponse
- `session` : HttpSessopm
- ...
	- `application`
	- `page`
	- ...


Esempio slide 33 - 34

## JSP Standard actions
- include
	- `<jsp:include page=“URL” />`
		- inclusione statica (html) o dinamica (jsp o servlet) a run-time
- forward
	- `<jsp:forward page=“URL” />`
- useBean ([[#Javabean]])
	- `<jsp:useBean id= "instanceNamescope ="page|request|session|application" class="..." type="..." beanName="..."</jsp:useBean>`
	-  voglio caricare e usare una classe java esterna
	- ho parametri
		- nomeIstanza (id)
		- classe che voglio caricare (class = , type=)
		- scope: dove var vive (opzioni: page, request, session, application)

## Best practies JSP
- non troppo codice java
- scegliere il meccanismo giusto
	- dati statici in file separanti e non (ri)generati dinamicamente
	- contenuti statici con direttiva `<%@ include file... %>`
	- contenuti dinamici a run time con `<jsp include page ...>`
		- pagine jsp o servlet
- non mescolare logica e presentazione
- usare [[#Filters]] se necessario
- usare database per informazioni persistenti
	- pooling connection


## Filters
Importante distinguere pagine pubbliche e ad accesso limitato
- scrittura codice diverso
- nelle seconde devo verificare se cliente ha sessione e se è identificato
	- altrimenti devo mandarlo a pagina di login (pubblica) 
- quindi da utente che va a pagine accesso limitato c'è pagina di check che controlla se session è in essere
	- e li o login o pagina a cui vuole accedere

Quindi tutte pagine ad accesso limitato devono avere codice di controllo che è sempre lo stesso
- c'è un pattern per questo:

Filtro
- tecnologia per scrivere in modo compatto questa logica chiamata da tutte pagine limitare
	- meglio se c'è livello dichiarativo in cui specifico quali pagine devono passare attraverso filtro (nel web.xml) cosi ho un filtro solo e elenco pagine che devono chiamarlo


## MVC
![](http://www.setgetweb.com/p/rational/com.ibm.etools.struts.doc_6.0.0/images/cstrdoc001a.gif)

Modello di solito usato quando c'è grafica da trattare
- M odel: dati
- V iewer : presentation
- C ontroller : action business logic
Slide 38


## Javabean
- classe java che 
	- fornisce costruttore pubblico senza argomenti
	- implementa *Serializable*
	- se pattern design di JavaBenas
		- metodi Set / Get
	- thread safe
- es slide 39


## Standard actions involving beans
- `<jsp:useBean id=“name” class=“fully_qualified_pathname” scope=“{request|session|application}” />`

- `<jsp:setProperty name=“nome” property=“value” />`
- `<jsp:getProperty name=“nome” property=“value” />`

Pay attention to the scope!
- slide 47

ESEMPIO DA SLIDE 41
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
- useBean
	- `<jsp:useBean id= "instanceNamescope ="page|request|session|application" class="..." type="..." beanName="..."</jsp:useBean>`
	-  voglio caricare e usare una classe java esterna
	- ho parametri
		- nomeIstanza (id)
		- classe che voglio caricare (class = , type=)
		- scope: dove var vive (opzioni: page, request, session, application)
# 8 Scambio informazioni e JSP
http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/08-JSP.pdf

---
## Scambio informazioni WebApp
![webAppInformationSchema](/Content/webAppInformationSchema.png)
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

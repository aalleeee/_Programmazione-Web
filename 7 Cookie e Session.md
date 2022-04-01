`30/03/22`

http://latemar.science.unitn.it/segue_userFiles/2022ProgrammazioneWeb/07-JavaServlets.pdf

## Cookies
- i cookie vanno creati e definiti prima di iniziare a scrivere la response (prima di response.setContentType("text/html"); ...) 
- i cookie funzionano solo se l'utente non li disabilita
- mantenere stato di una sessione utente
- **piccola porzione di informazione che viene memorizzata** sul computer del client dal sito web (salvata **sul browser**) **e viene rimandata al server** ogni volta che viene richiesta una pagina
- valore del cookie di solito identifica un client
- intacca il discorso di caching, java class Cookie non supporta cache definita da http

Funzioni principali:
- `String getValue() / void setValue(String s)`
	- questa è la nostra funzione principale
- `int getMaxAge() / void setMaxAge(int i)`
	- specificare valore in secondi di validità del cookie
	- se non specificata vale fino a chiusura del browser da parte del client
- `boolean getSecure / setSecure(boolean b)`
	- cookie encrypted
- `String getName() / void setName(String s)`
- `String getPath() / void setPath(String s)`
	- utili ad esempio se io ho più macchine server dedicate a servizi diversi ma di un unica organizzazione e voglio che i cookie siano "condivisi"

Esempio base
```java
Cookie userCookie = new Cookie(“user” , ”uid1234”); userCookie.setMaxAge(60*60*24*365);
response.addCookie(userCookie);
```

**SLIDE 28**

(slide 29 form con più button submit)

Vedi codice da slide 29 !


## Session
- memorizzazione informazioni lato server
-  la sessione usa cookie lato server e uno lato client per memorizzare i dati
	-  il cookie lato client memorizza solo un riferimento ai dati corrispondenti memorizzati sul server
- quando l’utente visita il sito web, il cookie lato client (con un numero di riferimento) viene inviato al server e il server utilizza questo numero per caricare i dati dell’utente
- per associare una sessione a un utente 2 metodi per passare l'identificativo tra client e server
	- usare cookie lato client
	- includere l'identificativo in ogni url ritornato al client `encodeURL()`
- funzionano solo se l'utente non li disabilita

Funzioni principali:
- `getSession()`
	- torna la sessione corrente o se non c'è ne crea una nuova
	- specficiando il paramentro false non ne crea una nuova se la richiesta non ha una sessione
- `public void setAttribute(String name, Object value)`
- `public Object getAttribute(String name)`
- `public Enumeration getAttributeNames()`
- `public void removeAttribute(String name)`
- `String getId()` , `boolean isNew()` , `void invalidate()`

Sessioni sono mantenute fino a un timeout che specifichiamo
- `public void setMaxInactiveInterval(int interval)`
	- se la sessione non è attiva per il tempo max di secondi viene eliminata
	- se si attiva viene il timer viene fatto partire da quel momento
- meglio specificare questo a livello di deployment nel `web.xml`
	- che è il deployment descriptor file

Deployment descriptor file
- possiamo usare le annotation (livello di sviluppo) o scrivere sul web.xml (livello di deployment)
	- whatever is defined in web.xml overwrites annotations


Vedi codice da slide 60 !
- importante fare il `synchronized(session)` della sessione
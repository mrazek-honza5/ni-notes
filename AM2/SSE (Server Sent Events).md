- Představen v rámci specifikace HTML5
- Jedná se o API pro zpracování HTTP streamu pomocí DOM eventů
- Prakticky realizuje [[Pushing a polling#Long polling|long polling]]
# Definice
- Definuje **EventSource** interface, který obsahuje:
	- Handlery - `onopen`, `onmessage`, `onerror`
	- Konstruktor - `EventSource(url)`
	- Metoda - `close()`
	- Atribut `readyState`
		- `CONNECTING` - Spojení se navazuje, nebo obnovuje
		- `OPEN` - spojení je aktivní
		- `CLOSED` - spojení je zavřené a nebude se pokoušeno jeho obnovení
## Formát
- `Content-Type` odpovědi musí být `text/event-stream`
- Každý řádek začíná řetězcem `data:`, nebo jiným kontrolním řetězcem (`retry: 10000` pro nastavení intervalu na obnovení spojení)
- Zpráva je ukončena pomocí dvou `\n` znaků
## Obnovení spojení
- Při přerušení je po uplynutí určité doby znovunavázáno spojení
- Obnovení je automatické, klient specifikuje ID poslední zprávy, pomocí HTTP hlavičky `Last-Event-ID`
- Server potom odesílá data, která navazují (i když už je třeba odeslal do zavřeného spojení)

## Ukázka
### Inicializace
```js
if (window.EventSource != null) {
  var source = new EventSource('your_event_stream.php');
} else {
  // Result to xhr polling :(
}
```

### Definováni event handlerů
```js
source.addEventListener('message', function(e) {
  // fires when new event occurs, e.data contains the event data
}, false);
 
source.addEventListener('open', function(e) {
  // Connection was opened
}, false);
 
source.addEventListener('error', function(e) {
  if (e.readyState == EventSource.CLOSED) {
    // Connection was closed
  }
}, false);
```

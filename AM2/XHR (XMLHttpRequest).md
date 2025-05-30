- Starší, více košatější přístup k asynchronní komunikaci založené na callbacku
- Standardizované rozhraní na využití HTTP protokolu v JavaScriptu
- Poskytuje více údajů (dá se tam například dělat progress bar, kolik dat máme staženo)
- Funguje to takto: ![[Pasted image 20250529180512.png]]

# Formát
- Základem je `XMLHttpRequest` objekt, obsahuje řadu funkcí a atributů
	- `open` - otevře request s určenou metodou, url, údaji pro HTTP auth...
	- `onReadyStateChange` - funkce která je volána když se změní `readyState`
	- `send, abort`
	- `status, statusText`
	- `responseText, responseXML`
	- `onload` - listener pro podporu server push
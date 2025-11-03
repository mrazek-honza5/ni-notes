- Zkratka pro Extensible Markup Language
- Zkratka `.xml`, `Content-Type: text/xml`
- Syntax:
```xml
<\?xml version="1.1" encoding="UTF-8">
<movie year="2007">
	<actors />
	<title>Medvídek</title>
</movie>
```
- Dobře formovaný dokument
	- Začíná prologem (\<?xml version="1.1" encoding="UTF-8">)
	- Obsahuje kořenový element
- Konstrukty
	- Element
		- Skládá se z názvu, atributů a obsahu, obsahuje uzavírací tag po obsahu
		- Pokud je obsah prázdný, může se rovnou uzavřít a nepotřebuje párový tag
		- př:
			- `<title id="...">Title</title>`
			- `<actors for="movie" />`
- XML schéma
	- Popisuje strukturu dokumentu
		- Omezení hodnot
		- Datové typy elementů
		- Sémantika...
- Související technologie
	- [[XPath]]
	- [[XQuery]]
	- XSLT - transofmační jazyk, nebudeme probírat
- Podmnožina jazyka [[XQuery]]
- Umožňuje vybrat relevantní elementy z [[XML]] dokumentu

## Path expressions
- Umožňují navigaci [[XML]] stromem
- Skládá se z **navigačních kroků**
	- Absolutní cesty začínají /
	- Relativní cesty začínají v implicitně specifikovaném kontextovém uzlu
- Příklady:
	 ![[Pasted image 20251103164500.png]]
	- `/movies`
	- `/movies/movie/title/text()`
	- `/movies/movie/@year`
	- `actor/text()`
	- `@director`

### Navigační krok
- Skládá se z až 3 komponent
	- Osy
	- Node test
	- Predikáty
- Uvažujme dále nějaký uzel $u$
#### Osy
- Jedná se o vztahy mezi uzly v kontextu uzlu $u$
- Označují se pomocí 
- Mohou být:
	-  child
		- Vybere potomky uzlu $u$
		- Výchozí osa, pokud není specifikována jiná
	-  attribute
		- Vybírá atribut uzlu $u$
		- Jediná osa, umožňující výběr atributu
	-  self
		- Vybírá pouze aktuální uzel $u$
	-  descendant(-or-self)
		- Vybírá všechny neatributové uzly v podstromu
		- Varianta or-self zahrnuje také $u$
	-  parent
		- Vybírá rodičovský uzel uzlu $u$
	-  ancestor(-or-self)
		- Vybírá všechny předky
		- Varianta or-self zahrnuje také $u$
	-  preceding-sibling / following-sibling
	-  preceding / following
#### Node test
- Slouží k filtrování uzlů vybraných *osou* 
- Umožňuje testovat pouze podle typu uzlu, nebo názvu uzlu
- Příklady:
	- name:
		- `/movies`
		- `/movies/movie/attribute::year`
	- všechny elementy
		- `/movies/*`
		- `/movies/movie/attribute::*`
	- všechny textové uzly
		- `/movies/movie/title/text()`
	- všechny uzly
		- /movies/descendant-or-self::node()/actor``

### Predikáty
- Dodatečné filtrování
- Pravdivý, pokud je výsledná sekvence neprázdná
- Umožňuje:
	- Test existence
		- Vyhodnocuje existenci absolutní nebo relativní cesty
		- Příklady
			- `/movies/movie[actor]`
			- `/movies/movie[actor]/title/text()`
	- Porovnání
		- `/descendant::movie[@year > 2000]`
		- `/descendant::movie[count(actor) ge 3]/title`
	- Test pozice
		- Umožňuje filtrování založené na pozici v kontextu
		- Příklady:
			- `/descendant::movie/actor[position() = 1]`
			- `/descendant::movie[actor][position() = last()]`
	- Logické výrazy
		- `/movies/movie[@year > 2000 and @director]`
		- `/movies/movie[@director][@year > 2000]`


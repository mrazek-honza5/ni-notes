- Zkratka pro Structured Query Language
- Používá se pro práci s daty v relačních databázích
- Níže se zabýváme hlavně vyhodnocením dotazu a optimalizací
	- Konkrétně specifikovanou pro Oracle RDBMS

# Datové struktury
- Jedná se o datové struktury, do kterých se ukládají data
- Data se ukládají do bloků (zjistit více)
- Je potřeba, aby data byla pro danou strukturu vhodná, nejčastěji se používá Heap table s indexy
## Heap table
- Data jsou ukládána neseřazeně tak, jak určí RDBMS
## Heap table s indexy
- Zřejmě nejčastější datová struktura
- Nad neseřazenými daty (klasický heap table) vytváříme indexy
	- Pomocné struktury
	- Indexů může být kolik chceme
- Indexy nesou výhody
	- Snížení latence přístupu k datům (při dotazování)
- Nevýhody indexů
	- Zabírají místo
	- Při vkládání / mazání / jiných úpravách je třeba je přepočítat
## Indexem organizovaná tabulka (IOT)
- Jakoby se do indexu ukládal přímo data místo Row id
- Sice je rychlejší, ale může vést na hlubší strom
## Cluster
- Buď indexem organizovaný, nebo hashovaný

# Další struktury
## Databásový blok
- Základní struktura pro ukládání dat
- Jedná se o nejmenší jednotku
- Typicky má velikost 8kB
- ![[Pasted image 20251109194248.png]]
	- Na obrázku lze vidět free space - to je z důvodu, aby se při vkládání hodnot / updatu hodnot (například varcharů) nemusel přealokovávat blok. 

## RowID
- Vysvětlováno na Oraclu
- Jedná se o unikátní identifikátor řádku, používaný na interní úrovni
- Skládá se z několika částí (zakódovaných do B64)
	1. ID datového souboru (počínaje 1)
	2. Datový blok, kde se řádka nachází
	3. Pozice řádky v databásovém bloku (počínaje 0)
	4. Object ID (pro objektově-relační systémy)
- Používá se pro indexy
- Není vhodné jej využívat na aplikační úrovni (může se měnit)

## Indexy
### B-Tree index
- Užitečný, když má chtěný indexový sloupec vysokou kardinalitu (počet různých hodnot klíče a počet různých řádků je 1)
- Poměrně dobře vyvážený 
- Faktor větvení je typicky velmi vysoký (100 - 150)
- Hloubka je většinou velmi nízká (2-4)
- ![[Pasted image 20251109192216.png]]
- Snižuje cenu přístupu na hodnotu $cost = h(t) + 1$, kde $h$ je hloubka stromu $t$
	- Platí pro Heap table s indexy

### Bitmap index
- Bitmap index
	- Užitečný, když nejde b-tree
	- Je implementovaný jako tabulka podle ROWID, kde sloupce jsou unikátní hodnoty podle indexu. Každý řádek má potom hodnoty true false podle toho, jestli má danou hodnotu.

# Optimalizace dotazů
- Cost-based přístup
- Důležité jsou pro to statistiky 
	- Počet řádek
	- Variabilita atributu
	- Počet stránek
	- Block faktor (kolik řádků máme v bloku)
	- Podpůrné (pro b-strom)
		- Výška stromu
		- Počet listových bloku
		- Průměrný počet následníků
	- Obecné
		- Clustering factor


## Statistiky tabulek
### Heap table statistiky
- Základní statistiky:

| Značení  | Význam                                                              |
| -------- | ------------------------------------------------------------------- |
| $nR$     | # řádků relace $R$                                                  |
| $V(A,R)$ | **variabilita** = # unikátních hodnot $A$ v relaci $R$ ()           |
| $pR$     | # stránek pro uložení $R$ (kolik řádků se v průměru vejde do bloku) |
| $bR$     | blokovací faktor                                                    |
- Doplňující statistiky
	- Histogramy - popisují rozložení hodnot v rámci tabulky
	- Minimální a maximální hodnoty
### B-Tree statistiky
- Základní statistiky ($A$ je klíč indexu)

| Značení  | Význam                                                                         |
| -------- | ------------------------------------------------------------------------------ |
| $f(A,R)$ | průměrný počet následníků uzlu (~50 - 150 v reálných DB)                       |
| $I(A,R)$ | hloubka indexového stromu (~2 - 4 v reálných DB) ~ $log(V(A,R)) / log(f(A,R))$ |
| $p(A,R)$ | # listových bloků                                                              |
- Doplňující statistiky
	- Clustering factor - kolik bloků musím přečíst, abych data dostal seřazené podle indexu

### Triky
- Clusterované indexy - indexy nad tabulkou seřazenou podle indexovacího klíče
- Složený index
- Reverzní index
- Index založený na funkci
## Údržba statistik
- Neaktuální statistiky způsobují špatná rozhodnutí optimalizátoru
- Neznamená to ale, že se aktualizují online - to nesmrdí mi

## Cena různých přístupů
Příklad: `SELECT * FROM r WHERE a = 'x'`

### Cena bez indexu (full scan of heap table)
- $cost = \#r/2$, pokud `a` je unikátní
- $cost = \#r$, pokud `a` není unikátní

### Unikátní index na r(a)
- $cost = I(a,r) + 1$ v případě heap table a index
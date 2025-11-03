## Struktury
- Jedná se o datové struktury, do kterých se ukládají data
- Je potřeba, aby data byla pro danou strukturu vhodná, nejčastěji se používá Heap table s indexy
- Zmíněné datastruktury:
	- Heap table
		- Obsahuje nese
	- Heap table s indexy
	- Indexem organizovaná tabulka
	- Hash cluster

### RowID
- Jedná se o unikátní identifikátor řádku, používaný na interní úrovni
- Skládá se z několika částí 

## Indexy
- B-Tree 
	- Užitečný, když má chtěný indexový sloupec vysokou kardinalitu (počet různých hodnot klíče a počet různých řádků je 1)
	- 
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

## Údržba statistik
- Neaktuální statistiky způsobují špatná rozhodnutí optimalizátoru
- Neznamená to ale, že se aktualizují online - to ne

## Cena různých přístupů
Příklad: `SELECT * FROM r WHERE a = 'x'`

### Cena bez indexu (full scan of heap table)
- $cost = \#r/2$, pokud `a` je unikátní
- $cost = \#r$, pokud `a` není unikátní

### Unikátní index na r(a)
- $cost = I(a,r) + 1$ v případě heap table a index
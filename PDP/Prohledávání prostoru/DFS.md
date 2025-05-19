- Prohledávání do hloubky
- Používá zásobník

## Kritéria pro návrat a ukončení

1. DFS s jednoduchým návratem (SB-DFS)
	- Hledáme koncový stav bez optimalizace
	- Návrat se provádí z nepřípustných mezistavů, nebo nepřípustných koncových stavů
2. Metoda větví a řezů (BB-DFS)
	- Přidáme nějakou cenovou funkci
	- Když nacházíme potom přípustný mezistav, jehož cena je horší, tak rovnou provedeme návrat

## Omezení hloubky prohledávání

* Máme dva druhy - strom (omezená hloubka) a cyklický graf (možnost zacyklení)
	* Prevence zacyklení
		* Kontrola, zda byl kontrolovaný stav již generován - prakticky nemožné
		* Lze kontrolovat krátké cykly o délce N
		* Rozumné je ale stanovit horní mez
			* Po N krocích se vrátím
			* To vede na DFS s postupným prohledávání, kdy N postupně zvyšuji, pokud nenajdu řešení
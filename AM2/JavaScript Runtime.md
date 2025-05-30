- Zde vysvětlujeme na JavaScriptu

# Runtime
![[Pasted image 20250529162020.png]]
- Jsou zde 3 základní komponenty
- Jedná se o základ asynchronního I/O
- Může jich být i více, typicky najdeme vlastní runtime v prohlížeči i pro iframe a Web worker
	- Komunikují mezi sebou skrze zasílání zpráv (`postMessage`)
	- Příjemce potom musí naslouchat události, aby mohl zprávu přijmout

# Základní komponenty
## Stack - zásobník
- V zásobníku máme *frames*, které obsahují například argumenty funkce, lokální proměnné apod
## Heap - halda
- Na haldě jsou alokované objekty
## Queue - fronta
- Fronta obsahuje seznam zpráv, které se mají zpracovat
	- Zpráva obsahuje data a callback (funkce, co se má nad daty zavolat)
- Z fronty jsou zprávy vybírané do event loopu
## Event-loop - událostní smyčka
- Z fronty vybere zprávu a tu zpracuje
- Zpracování je sekvenční a tudíž by nemělo trvat dlouho, jinak dojde k zablokování
	- Řešením je využít Worker thread
- Technologie web serveru
- Event-driven I/O framework
	- Veškeré I/O je neblokující
- Jedno vlákno obsluhuje požadavky
- Více vláken poté obsluhuje I/O
	- Každá I/O úloha je událost, která obsahuje vlastní callback

## Event loop
![[Pasted image 20250529164004.png]]
- Umožňuje asynchronní I/O operace
- Má 6 fází:
### timers
- Obsluha callbacků, které jsou naplánovány přes `setTimeout()` a `setInterval()`
### I/O callbacks
- Obsluha callbacků v rámci zpracování vstupu a výstupu (volání API, query do DB...)
### idle/prepare
- Interní záležitost
### poll
- Kontrola příchozích I/O událostí
### check
- Vyvolání callbacků, které byly nastaveny jako `setImmediate`
### close callbacks
- Vyvolání callbacků uzavírajících I/O operace (například zavření souboru nebo spojení)
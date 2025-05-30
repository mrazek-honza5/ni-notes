- Každý požadavek obsluhuje jedno vlákno z thread poolu
	- Vlákna jsou předvytvořena při startu systému
	- A poté přiřazována požadavkům (je to vlastně asynchronní na inbound)
- Typicky nejdéle trvající operace jsou zápisy / čtení z databáze nebo jiného externího systému v rámci požadavku
 
# Zpracování komunikace a úskalí
- Při příchodu požadavku se mu přiřadí vlákno
- Vlákno je rezervováno pro jeden požadavek po celou dobu výpočtu
- Když vlákno přistupuje k jiným prostředkům (čtení z disku, jiné I/O operace), tak je zablokováno a čeká
- Když se potom sejde více požadavků, tak budou příchozí spojení čekat

## Velikost thread poolu
- Thread pool musí být rozumně omezen
- Když je jich příliš mnoho a nastane nějaký problém, tak může dojít k obrovské režii při přepínání vláken a systém se stane efektivně neresponzivním
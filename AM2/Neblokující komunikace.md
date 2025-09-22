- Jinak také **asynchronní I/O**
- Existuje v mnoha jazycích a frameworkách
- Jedná se o obecný princip, ale my se budeme bavit o JavaScriptu
- Je to styl konkurentního programování

## Výhoda oproti [[Blokující komunikace|blokující komunikaci]]
- Vlákno se při čekání na dlouhotrvající operaci uvolní 
- Toto uvolněné vlákno může mezitím zpracovat další úlohy, které trvají krátký čas a obsluhovat tak jiné požadavky

# Princip
- Mám jeden proces a jedno vlákno
- Využívám **kooperativní multitasking**
	- Záleží na úloze, kdy se vzdá vlákna (například v JS `await`)
	- Jakmile se vzdá vlákna, tak jej přebírá jiná úloha, která opět běží, dokud nedoběhne, nebo se nevzdá vlákna
- Úlohy by neměly blokovat vlákno dlouho
	- Na [[Concurrency (soubežnost) a typy úloh#I/O-bound úlohy|I/O-bound úlohy]] využít `async` a `await`
	- Na [[Concurrency (soubežnost) a typy úloh#CPU-bound úlohy|CPU-bound úlohy]] využít [[Worker|workera]]
- Úlohy se pohybují v tzv [[JavaScript Runtime#Event-loop - událostní smyčka|event loopu]]
- Zástupce [[Key-value databáze]]
- Open source AP databáze
	- Může docházet k nekonzistencím (nemáme C z CAP)
	- Dá se ale konzistence dosáhnout skrze nastavení r / w quor bucketů
- Implementovaná v erlangu
- Aggregate-aware
- Rozšíření [[Search 2.0]] pro vyhledávání v hodnotách
# Datový model
## Instance
- Kontejner pro buckety a bucket typy
## Bucket types
- Nepovinné logické rozdělení kolekce bucketů
- Pokud není uvedeno, použije se `default`
- Primárně se používá pro sdílení konfigurace
## Bucket
- Kolekce key-value párů
- Není zde možnost prohledat celou kolekci
- Má určité vlastnosti
	- **n_val** - replikační faktor (číslo)
	- **r / w** - read / write quorum
		- Částečná hodnota, může být `all`, nebo číslo
		- Určuje, kolik replik může selhat v dané operaci, aby stále prošla
		- Např pokud je u 3 replik w_quorum 2, tak může selhat write do jedné repliky a write bude pořád úspěšný
		- Pro silnou konzistenci
			- $w > n_{val}/2$ write quorum
			- $r > n_{val} − w$ read quorum
	- **search_index** - jméno asociovaného indexu
	- **datatype** - název datového typu (string)
		- Nespecifikuje string, číslo apod, ale **CRDT**
		- Jedná se o způsob, jak se pomocí zavedením typů a povolených operací dají řešit konflikty
	- **allow_mult** - bool příznak
## Object
- Jedná se o key-value pár
- Key je unikátní v rámci [[#Bucket]]
- Value může být cokoliv
- Dále obsahuje metadata
	- Content type 
		- Typicky se používá MIME formát
	- Lastmod
# Návrhy
## Multiple buckets design
- Rozdělení entit podle typů
## Single bucket design
- Do jednohu bucketu ukládám všechny entity, prefixuju třeba typem nebo tak...
# Funkcionality
- CRUD operace
- Linky
- Search 2.0 = integrace se SOLR
- MapReduce
# Práce s DB
- Probíhá přes jeden z následujících interfaců
	- HTTP API
	- Protobuf API
	- Erlang API
- Nebo přes knihovny
	- Java, Ruby, python...
# HTTP API
- Metody specifikují CRUD akci
	- Create - POST nebo PUT
		- POST je čistý insert
		- PUT je update or insert
	- Read - GET
	- Update - PUT
	- Delete - DELETE
- URL
	- ![[Pasted image 20251110163519.png]]

## Příklady
- Vložení dat
```bash
curl -i -X PUT \ 
	-H 'Content-Type: text/plain' \ 
	-d 'Ivan Trojan, 1964' \ 
	http://localhost:8098/buckets/actors/keys/trojan
```
- Získání hodnoty
```bash
curl -i -X GET \ 
	http://localhost:8098/buckets/actors/keys/trojan
	
Content-Type: text/plain 
Content-Length: 17 
X-Riak-Vclock: a85hYGBgzGDKBVI8XxW02dii9T4wMKgLZjAlMuWxMti+WXKHLwsA 
Last-Modified: Sun, 25 Sep 2022 15:14:05 GMT

Ivan Trojan, 1964
```

- Smazání (nemusí existovat)
```bash
curl -i -X DELETE \ 
	http://localhost:8098/buckets/actors/keys/trojan
```
## Operace s buckety
- Vypsání všech bucketů 
```bash
curl -i -X GET http://localhost:8098/buckets?buckets=true
```
- Vypsání všech klíčů bucketu 
```bash
curl -i -X GET http://localhost:8098/buckets/actors/keys?keys=true
```
### Změna vlastností bucketu
- Místo keys v path se udává props

# Konflikty
- Situace, kdy nemají repliky stejná data pro stejný klíč
- Riak jako AP systém musí konflikty řešit, jsou nevyhnutelné
- Řeší se přes
	- Generický koncept
	- [[#CRDTs]]
# CRDTs
- Zkratka pro Convergent Replicated Data Types
- Jedná se o definici
	- Datové struktury
	- Povolených operací
- Řeší konflikty
## Counter
- Jedná se o celočíselný čítač
- Povolené operace - inkrementace / dekrementace hodnoty
- Pravidlo konvergence
	- Aplikují se všechny konfliktní požadavky
## Množina
- Kolekce unikátních binárních hodnot
- Operace - přidání / odebrání prvku
- Pravidlo konvergence
	- Přidání vyhraje nad odebráním 
## Mapa
- Neseřazená kolekce name-value párů
	- Name je string
	- Value je cokoliv
- Operace - přidání, odebrání, update prvku
- Pravidlo konvergence
	- Přidání vyhraje nad odebráním

## Registry a příznaky
- Registr
	- Jakákoliv binární hodnota
	- Pravidlo konvergence - nejmladší hodnota vyhrává
- Příznaky
	- Boolovský příznak
	- Pravidlo konvergence - true vyhraje nad false
## Přístup k CRDTs
- Probíhá opět přes URL, akorát místo `keys` nebo `props` dám `<datatype>` (např. `counters`)
# Replikace
- Řídí se konzistentní hashovací funkcí (neměnná při změnách clusteru) 
	- Bere jméno bucketu a klíč objektu
	- Vrací 160 bitové číslo - [[#Riak Ring]] 
## Riak Ring
- Jedná se o prostor, jak se adresují replikace
- Jedná se o celá 160 bitová čísla
- Celý prostor je rozdělen do disjunktních partitions
	- Fyzické uzly mohou mít více virtuálních
	- Virtuální uzel je zoodpovědný za jednu partition
- Při selhávání virtuálního uzlu
	- Vezme se jeho soused
	- Soused dočasně převezme jeho agendu a po opětovném zprovoznění mu předá repliky
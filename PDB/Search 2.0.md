- Rozšíření pro [[Riak KV]]
- Implementuje full-textové vyhledávání nad hodnotami objektů
- Používá Apache Solr

# Princip fungování
## Indexace
- Spouští se vždy když se změní objekt Riaku
- Riak objekt -- extractor --> Solr dokument -- schema --> Solr index

## Vyhledávání
- Nejdříve se Riak search query převede do Solr, ten vyhledá a potom vrátí identifikace vyhovujících objektů

# Extractor
- Definice pro práci s objekty
- Vybírá se automaticky podle content type
- Jsou již implementované
	- Plain text, XML, JSON
	- Pro datové typy:
		- counter
		- set
		- map
- Ale dají se implementovat i vlastní v Erlangu
## Příklad
- Vstup (Riak hodnota) - do hodnot ukládám XML
- Výstup - Index
![[Pasted image 20251110170246.png]]

# Indexovací schéma
## Solr dokument
- Extrahovaná pole
- Dodatková pole
	- Type bucketu **_yz_rt**
	- Bucket **_yz_rb**
	- Klíč pro záznam **_yz_rk**

## Solr schéma
- Popisuje, jak jsou hodnoty indexované
	- Hodnoty jsou analyzovány, tokenizovány a filtrovány
	- Trojice **hodnota tokenu field name document id** jsou zaindexované
- Existuje výchozí schéma **_yz_default**

# Práce se search 2.0
1. Vytvoření indexu
```bash
curl -i -X PUT \ 
	-H 'Content-Type: application/json' \ 
	-d '{ "schema" : "_yz_default" }' \
	http://localhost:8098/search/index/imovies
```
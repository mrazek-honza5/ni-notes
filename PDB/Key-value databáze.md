# Datový model
- Jednoduchý - páry **klíč - hodnota**
- Key je jediný způsob přístupu - **primární klíč**
	- Bez přidaného SOLR nelze například vyhledat podle hodnoty
- Value je víceméně cokoliv

## Klíč
- Často reálný identifikátor (email, session id,...)
- Nebo např. generované řetězce

# Operace
- Základní CRUD - je nutnost znát klíč
- Nadstavby
	- Mapreduce

# Další funkcionality
- Expirace key-value páru
	- Po expiraci se záznam automaticky smaže
- Linky mezi key-value páry
	- Není to 
- Kolekce hodnot
	- Místo hodnoty můžeme uložit nějakou kolekci (set, pole...)


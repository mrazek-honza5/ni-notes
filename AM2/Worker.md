- Kód, který běží ve speciálním vlákně, které neblokuje jiné
- Každý worker běží v event loopu a komunikuje skrze zasílání zpráv
- Může dělat cokoliv, ale nemanipuluje s DOM
	- Může založit další workery

# Typy
## Dedikovaný worker 
- Přístupný je pouze skriptu, který ho vytvořil
## Sdílený worker
- Přístupný více skripty (ifram)
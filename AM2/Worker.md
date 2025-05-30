- Umožňuje spustí kód, který běží ve speciálním vlákně a neblokuje jiné
- Každý worker běží ve vlastním event loopu a komunikuje skrze zasílání zpráv
- Může dělat cokoliv, ale nemanipuluje s DOM
	- Může založit další workery
# Typy
## Dedikovaný worker 
- Přístupný je pouze skriptu, který ho vytvořil
## Sdílený worker
- Přístupný více skripty (`iframe`)
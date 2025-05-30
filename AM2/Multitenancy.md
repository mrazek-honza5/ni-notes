- Přístup k návrhu architektury
- Různí uživatelé sdílejí výpočetní prostředky
- To přináší různé výhody
	- Můžeme centralizovat infrastrukturu do levnější lokality
	- Zvýší se nejvyšší možná kapacita 
	- Optimalizuje se využití systému
- Hlavní předpoklad je, že všichni uživatelé využívají veškeré prostředky ve stejný čas
	- Když se to stane, tak je to kontrolováno mechanismy, které například omezí zdroje pro uživatele na krátkou dobu
	- Je zde teda jistý overcommitment

# Možnosti sdílení
## Sdílené všechno
- Veškeré zdroje jsou sdíleny mezi všemi tenanty
- Běžné pro SaaS model
- Aplikace poskytuje pouze izolaci tenantů, ale jejich data jsou uložena ve stejné databázi

## Sdílená infrastruktura: Virtuální stroje
- Infrastruktura je sdílená skrze virtuální stroje
- Každý tenant má svůj virtuální stroj
- Izolaci mezi tenanty obstarává hypervizor

## Sdílená infrastruktura: Virtualizace OS
- Infrastruktura je sdílená skrze virtualizaci v OS
- Každý tenant má svojí processing zónu a izolace je zajištěna operačním systémem
- Není zde abstrakční vrstva mezi aplikací a OS, narozdíl od sdílení skrze virtuální stroje
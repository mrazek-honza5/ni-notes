- Jedná se o prostředí umožňující správů obrazů pro využití v [[Kontejner|kontejnerech]]
- Staví na Linux namespaces a filesystému OverlayFS
	- OverlayFS poskytuje vrstvený filesystém, kde se vrstvy nabalují na sebe 
	- Poslední vrstva je R/W a někdy se označuje jako *Upper Dir*
- Dělí se na dvě éry - před 1.11.0 a po 1.11.0 
	![[Pasted image 20250601103904.png]]
	- Rozdělení na:
		- Container runtime => *containerd*
			- Abstrakce syscallů a jiné OS specifické funkcionality => umožňuje spouštět kontejnery na různých OS
			- Komunikuje s kernelem pro spuštění kontejnerizovaného procesu
			- Dále obsahuje:
				- *runc* => komunikace s Linux namespaces při vytváření kontejnerů
				- *shim* => parent jednotlivých procesů běžících v kontejneru
		- Container engine => *Docker engine*
			- Přijíma vstupy od uživatele, stahuje images a připravuje metadata pro container runtime

# Pojmy
## Image
- Soubor vrstev filesystému stavěné na sobě
- Vrstvy jsou neměnné
	- Další vrstva nemůže měnit to, co bylo v předchozí přímo. Pokud teda v další vrstvě něco smažu, tak se to označí jako smazané, ale soubor tam fyzicky stále bude
## Kontejner
- Jeden nebo více procesů běžících na izolovaném namespacu ve filesystému, poskytnutým [[#Image|obrazem]]
- Umožňuje změny za běhu díky poslední vrstvě filesystému
- Instance jsou tzv. *ephemeral*
## Klient
- Aplikace komunikující s container enginem skrze API
- Běžně třeba Docker CLI
## Registr
- Služba obsahující [[#Image]]
- Umožňuje také přidávání vlastních obrazů
## Swarm
- Klastr jednoho či více docker enginů

## Dockerfile
- Sada instrukcí, která vytváří nový [[#Image]]

## Stavový diagram
![[Pasted image 20250601122530.png]]

# Síťování a propojení
- Docker má by default 3 sítě
	- bridge - defaultní, umožňuje přístup do sítě hostitele 
	- host - všechny síťová rozhraní hostitele jsou dostupná kontejneru
	- none - kontejner je ve své vlastní síti a nemá dostupné žádné rozhraní
# Volumes a data
- Volume je složka, který obchází vrstvení
- Je uchována i po smazání kontejneru
- Je možné jí namapovat do hostitelského filesystému

- Privátní síť v rámci [[Region|jednoho regionu]], ve kterém mám instance
- Jedná se o souvislý blok IPv4 CIDR (classless) adres
- Tenant si poté může v rámci jeho VCN vytvářet další subnety
	- Subnety musí být uvnitř VCN, ale mohou být rozprostřeny mezi více [[Datové centrum|datových center]]
# Směrování a bezpečnost
- Subnety mají specifická pravidla, například
	- Public subnet => může komunikovat s internetem (oboustraně). Přístup z internetu je zajištěn pomocí **internet gateway**
		-  Využívají se principy **SNAT** (komunikace ze sítě do internetu) a **DNAT** (komunikace do sítě)
	- Private subnet => může být kompletně izolovaná, nebo komunikuje do internetu
		- Využívá se zde pouze **SNAT**
- Komunikace mezi sítěmi je obecně založena na komponentě **gateway**
- Směrování je zajištěno klasicky routovacími tabulkami
- Bezpečnost je u každého subnetu zajištěno firewallem
## Bastion
- Jedná se o komponentu, která slouží pro vstup do systému 
- Je to jediný "server", na který se mohu dostat zvenčí
- Může mít různé podoby
	- **RDP** - Remote Desktop Protocol (dnes docela běžné)
	- **SSH** 
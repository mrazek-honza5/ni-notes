- Jedná se o soubor výpočetních prostředků, který se řídí určitými koncepty:
	- On-demand a samoobsluha
		- Zdroje jsou poskytovány na vyžádání, když jsou potřeba
		- Možnost automatizace, bez zásahu člověka
	- Širokopásmé připojení na síť
	- Resource pooling
		- Zdroje jsou využívány několika tenanty
		- Také jsou dynamicky přidělovány podle potřeby
	- Škálovatelnost a elasticita
		- Infrastruktura se zvětšuje a zmenšuje dle potřeby
	- Měřené služby
		- Využití zdrojů je měřeno a může být kontrolováno
		- Mohou z toho být dělané reporty
	- Pay-per-use
		- Uživatel cloudu platí za skutečně využité prostředky
- V cloudu se také občas využívá overcommitment v rámci multitenancy, kdy klientům v součtu prodáváme větší kapacitu, než máme. Statisticky se totiž mockrát nestane, že to všichni využívají naplno ve stejný čas
# Modely nasazení
- Veřejný cloud
	- Dostupný za peníze pro veřejnost
	- Zakoupené zdroje jsou dostupné v rámci tenancy
- Privátní cloud
	- Cloudy organizací, které provozují sami (např. CloudFIT)
- Hybridní cloud
	- Cloud kombinovaný z veřejného a hybridního
	- Například v případě nedostatečného výkonu využiju veřejný cloud
# Vrstvení služeb v cloudu
## Infrastructure as a Service (IaaS)
- Nejširší záběr s největší režií
- Typicky se jedná o virtuální PC s předdefinovanými parametry instance (paměť, CPU...)
	- Disk je zde většinou rozdělen na 
		- Block volume - privátní pro instanci, rozdělen na boot volume a data volume
		- NFS volume - síťový disk (pomalejší, ale sdílený pro více instancí)
- Příklady
	- Load balancer
	- Autoscaling
	- Monitorování zdrojů
	- Propojení s on-premise sítí
## Platform as a Service (PaaS)
- Mám k dispozici runtime pro nasazení vlastní aplikace (vyberu si runtime, ve kterém je napsaná)
- Pod touto vrstvou je skryt většinou nějaký kontejner, nebo dnes Kubernetes
### Function as a Service (FaaS)
- Je to jakási podmnožina [[#Platform as a Service (PaaS)]]
- Jedná se o provedení funkčního volání a je dobrá pro realizaci serverless architektur
## Software as a Service (SaaS)
- Dostávám k dispozici hotovou službu
- Může jít například o databázi
## Porovnání služeb
![[Pasted image 20250530145835.png]]
# Nasazení aplikace do cloudu
## Lift-and-Shift
- Aplikace se vezme tak, jak je a hodí se do cloudu
- Nemá však většinu benefitů [[Cloud native]] aplikace
- Vznikl s nástupem HTML5
- Přišlo hodně změn (hlavně i v protokolu HTTP)
- Před touto dobou poskytovatelé služeb moc nedodržovaly různé (verze 1.5)
- S tímto se to už opravilo, nikdo moc neví, co k tomu poskytovatele vedlo

# Principy
## Read-Write web
- Lidé nejsou jen konzumenti, ale vytvářejí obsah
## Programmable web
- Uživatelé interagují s aplikacemi skrze nějaké rozhraní
- Na rozhraní se napojuje
	- Prohlížeč přes UI
	- Nebo programátor přes API (REST / GraphQL) 
- Toto umožňuje vytváření ekosystému okolo stávajících aplikací
## Realtime web
- Data na webu mohou vznikat v reálném čase
- Může jít o:
	- Streamování
	- Aktualizace dat z webových systémů
- Toto umožňují například následující technologie
	- [[Websocket]]
	- [[SSE (Server Sent Events)]]
## Social web
- Představuje prostor sociálních sítí
- A algoritmy pro analýzu dat z těchto systémů

# Nárazová zátěž
- Aplikace z Web 2.0 musí umět obsloužit náhlé výkyvy v provozu

## Slashdot efekt
- Když do aplikace přijde velké množství požadavků, musí se s tím aplikace vypořádat
- Aplikace okolo toho musí mít i nějakou infrastrukturu, která umožňuje rozložení zátěže
	- Například load balancing, dynamické škálování aplikace
- Jméno tohoto pochází z názvu stránky, kde se občas zveřejnil zajímavý web, který potom dostal nebývalé množství požadavků
- Prostředí, které provozuje aplikační logiku
- Se serverem komunikuje klient (browser) přes aplikační protokol (HTTP)
- Dříve velký monolit, obsahující spoustu služeb (NGINX, aplikaci, DB apod)
	- Dnes je snaha to rozdělit (mám proces s logikou, NGINX, další vrstvy - například caching)
# Komunikace
- Způsob realizace komunikace se liší v principu fungování
- Může drasticky ovlivnit výkonnost serveru při zátěži
## Druhy
- [[Blokující komunikace|Blokující komunikace (Synchronní I/O)]]
- [[Neblokující komunikace|Neblokující komunikace (Asynchronní I/O)]]
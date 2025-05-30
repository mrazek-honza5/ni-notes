- Jedná se o možnosti realizace "realtime" komunikace
- Konceptuální základ v architekturách založených na zprávách
- HTTP je ale request-response model, takže podporuje pouze **polling**
## Polling
- Periodicky se doptávám na data
- Jednoduché na implementaci
- Problémy:
	- Vytěžování infrastruktury
	- Nemusí být úplně realtime (latence)

## Pushing (chunking, nebo long polling)
### Long polling
- Podobné jako [[#Polling]], ale server drží request nějakou dobu
- Využíváno v [[Browser#SSE (Server Sent Events)|SSE]]
- Výhody proti pollingu
	- Nevytěžuje tolik server ani síť
	- Lepší latence, pokud je interval mezi zprávami nejednotný
- Problémy
	- Při velkém počtu zpráv může být výkon horší než u pollingu
### Chunking
- Server odesílá aktualizace dat přes otevřené spojení
- Toto spojení se nastaví pomocí HTTP hlavičky (`Transfer-Encoding: chunked`)
- Problémy:
	- Chunky nemusí být považovány za eventy
	- Mohou být na klientské straně bufferovány a dostupné až po ukončení spojení
	- Není standardizované => toto zavedl až [[Browser#SSE (Server Sent Events)|Server Sent Events]]
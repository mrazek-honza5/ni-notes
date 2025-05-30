- Jedná se o úložiště, které můžeme nějakým způsobem připojit k [[Compute instance|instanci]]
- Pro high-performance je potřeba využít lokální disky, pro běžný workload stačí síťové disky
# Block storage
- Jedná se o disky privátní pro instanci uložené na File Storage Serveru
	- Připojení probíhá skrze protokolo **ISCSI**
	- Má to však výhodu, že je můžeme přepojovat do jiné instance
- Dělí se na dvě části, které se mountují do instance
	- Boot volume
		- Z této části se zavádí systém
		- Je malý, obsahuje jen OS a nejpotřebnější knihovny
	- Data volume
		- Větší část, obsahuje data 

# Bucket
- Úložiště pro objekty
- Ukládá různé formáty, velké množství dat
- Rozděluje se podle latence přístup na
	- Hot bucket
	- Cold bucket - archivační data, přístup je v rámci hodin
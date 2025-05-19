- Standardizovaný a přenositelný formát zasílání zpráv mezi procesy
- Procesy mohou běžet na různých počítači propojených komunikační sítí [[Paralelní a distribuované počítače#Distribuovaná paměť|NUMA model]]
- Definuje syntaxi a sémantiku knihovních funkcí pro psaní v různých jazycích

### MPI Knihovna
- Implementace standardu
- Velké množství - OpenMPI, MPICH....

# Komunikace procesů
- MPI procesy nesdílí paměť
- Komunikují zasíláním zpráv
- Veškeré proměnné jsou privátní, každý MPI proces má svoje instance
- Redukce se provádí například pomocí funkce `MPI_Allreduce`

# Funkce MPI
## MPI_Init_thread
- Parametry - nemá
- Vrací v proměnné zaručenou místu spolupráce MPI s vlákny (dělení procesu, jako v semestrálce)
- Ta může mít následující hodnoty (reálné hodnoty jsou číselné a vzestupné):
	- `MPI_THREAD_SINGLE` - pouze MPI, procesy se nedělí na vlákna
	- `MPI_THREAD_FUNNELED` - vícevláknové procesy s omezením, že pouze hlavní vlákno může volat funkce MPI (jednoportový model)
	- `MPI_THREAD_SERIALIZED` - vícevláknové procesy s omezením, že v daném okamžiku smí funkce MPI volat pouze jedno vlákno (MPI volání jsou kritická sekce)
	- `MPI_THREAD_MULTIPLE` - vícevláknové procesy bez omezení

## MPI_Comm_rank
- Identifikátor procesu v rámci daného intra-komunikátoru
- Parametry: `(MPI_Comm comm, int *rank`
	- comm - komunikátor
	- rank - sem bude uložen výsledek
## MPI_Comm_size
- Velikost skupiny asociované s daným intra-komunikátorem
- Parametry: `MPI_Comm comm, int *size`
	- comm - komunikátor
	- size - sem bude uložen výsledek

## MPI_Send 
- Odešle data a vrací se (v závislosti na variantě a blokující)
- Parametry: `const void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm`
	- buf - adresa data bufferu
	- count - počet prvků v buf
	- datatype - typ
	- dest - [[#MPI_Comm_rank]] cílového uzlu
	- tag - označení zprávy
	- comm - cílový komunikátor
- Varianty
	- Blokující - po návratu lze buf již přepsat
		 - **MPI_Send**
			- Vrátí se, pokud byla odeslána cílovému procesu a ten inicioval přijetí, nebo byla překopírována do dočasného systémového bufferu pro pozdější odeslání. 
			- Volba je na MPI knihovně a jde tedy o nelokální operaci 
		- **MPI_Bsend** 
			- Návrat z funkce zaručeně nezávisí na připravenosti příjemce přijímat data (u standardního může).  
			- Vrací se, po uložení dat do bufferu, předem připraveného pomocí `MPI_Buffer_attach`. 
			- Jedná se o lokální operaci
		- **MPI_Ssend**
			- K návratu dojde v případě, že cíl iniciuje přijetí dat
			- Jedná se o nelokální operaci
		- **MPI_Rsend**
			- Pokud není příjemcem inicializován příjem dat, funkce se vrací s chybou
			- Jinak se vrací pote, co příjemce iniciuje přijetí zprávy
			- Jedná se o nelokální operaci
	- Neblokující
		- Prefix I
		- Dále vrací (do pointeru, klasicky), objekt typu `MPI_Request`, která je vstupním argumentem testovacích funkcí
		- Po návratu ještě nelze buf přímo přepsat, musí se testovat, nebo explicitně vyčkat na dokončení
			- `MPI_Test(MPI_Request *request, int *flag, MPI_Status *status)`
			- `MPI_Wait(MPI_Request *request, MPI_Status *status)`
## MPI_Recv 
- Příjímá data
- Parametry: `void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status *status`
	- buf - adresa data bufferu, kam se uloží data
	- count - počet přijatých položek
	- datatype - typ přijímaných položek
	- source - [[#MPI_Comm_rank]] zdroje uzlu
	- tag - filtr přijímaného označení zprávy
	- comm - cílový komunikátor
	- status - ukazatel na stavový objekt

## MPI_Get_count
- Ze status objektu vrátí velikost zprávy (počet přijetých prvků)
- Parametry `const MPI_Status *status, MPI_Datatype datatype, int *count`
	- status - ukazatel na stavový objekt
	- datatype - typ přijatých dat
	- count - výsledek operace
# Skupiny procesů
- Každý proces v MPI se nachází ve skupině
- V rámci skupiny jsou procesy číslovány od 0 do $p-1$
- Implicitně existuje skupina obsahující **všechny** procesy
- Lze vytvářet nové skupiny 
- Proces může být součástí více skupin (má ale v každé jiné číslo)

# Komunikátory
- Každá MPI funkce má jako parametr komunikátor
- Komunikátor určuje množinu procesů se kterými komunikujeme
- **Intra-komunikátor** je asociovaný s konkrétní skupinou a určuje komunikaci mezi procesy této skupiny
- `MPI_COMM_WORLD` je implicitní intra-komunikátor umožňující komunikaci se všemi procesy
- **Inter-komunikátor** je asociovaný s dvěma různými skupinami procesů a určuje komunikaci mezi procesy více skupin (nebude probíráno)
- Komunikace může dále být
	- 2-bodová - mezi 2 procesy
		- Příklad: `MPI_Send => MPI_Recv`
	- Kolektivní - mezi všemi procesy asociovanými s komunikátorem
- A z hlediska provedení
	- Blokující - komunikační funkce se vrátí až po dokončení (dosáhnutí určitého stavu komunikace)
	- Neblokující - komunikační funkce se vrátí ihned po inicializaci, výsledek je třeba explicitně testovat

# Typy přenášených dat
- Pro všechny základní datové typy C/C++ definuje MPI jejich ekvivalenty (prefix `MPI_<datový typ>` - `MPI_DOUBLE`...)
- Jedná se o hodnotu typu `MPI_Datatype`
- Pro složitější typy lze vytvořit vlastní pomocí `MPI_Type_create`

# Status (MPI_Status)
- Jedná se o objekt označující stav operace
- Obsahuje
	- `MPI_SOURCE` - [[#MPI_Comm_rank]] zdrojového procesu
	- `MPI_TAG` - tag dané zprávy
- Pomocí funkce 
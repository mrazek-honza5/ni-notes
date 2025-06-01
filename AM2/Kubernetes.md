- Orchestrační nástroj pro kontejnerizované aplikace
- Poskytuje nástroje na automatizaci nasazování, škálování, správě kontejnerizovaných aplikací skrze více uzlů
- Vznikly na bázi Borgu - projekt od Googlu

# Featury
- Automatický binpacking
- Horizontální škálování - na bázi CPU využití škáluje aplikaci
- Automatizované rollouty a rollbacky
- Orchestrace úložiště
- Self-healing - automatické restarty kontejneru, uzlů a kontrola healthchecků
- Load balancing a service discovery
# Architektura
![[Pasted image 20250601141937.png]]
## Control plane
- Skupina služeb zajišťující běh služeb
- Obsahuje
	- etcd - ukládá stav zdrojů v kubernetes clusteru
	- API server - zpracovává požadavky na ovládání kubernetes (frontend pro control plane)
	- Scheduler - zajišťuje zdroje v nodech (vybírá nodes na kterých budou běžet pody), podle určitých pravidel
	- kube controller manager - 
## Data plane
- Služby samotné aplikace
# Administrace 
- Probíhá skrze `kubectl` - příkazová řádka pro kubernetes
- V rámci kubernetes běží **API server**, který zpracovává požadavky na ovládání kubernetes

# Node
- Runtime prostředí v kubernetes
- Udržuje běžící pody
- Obsahuje prostředky, jak komunikovat s control planem
	- kubelet - agent, který je odpovědný za udržování kontejnerů v podech
	- kube-proxy - udržuje pravidla sítě, umožňuje podům komunikovat. Využívá službu OS filtrování paketů, nebo sám pakety přeposílá  
	- Container runtime - odpovědné za provoz kontejnerů. Může jich být několi (containerd, CRI-O), ale musí implementovat Kubernetes CRI (Container Runtime Interface)
# Pod
- Jedná se o skupinu jednoho nebo více provázaných kontejnerů => **zdroj**
- Sdílejí úložiště a síťové zdroje
- Sdílejí kontext (množinu Linux namespaces)
- Běží vždy na jednom výpočetním node
- Je označený pomocí `label` - klíč-hodnota
- Není vytvářen přímo, ale stará se o to kubernetes podle konfigurace [[#Workloady|workloadu]]


# Workloady
- Aplikace běžící v kubernetes
- Běží v množině [[#Pod|podů]]
- Mají určité typy
- Kubernetes pody vytváří automaticky, podle pravidel
## Deployment
- Pro stateless aplikace
- Může být kdykoliv vyměněna v případě potřeby
## Stateful set
- Stavová aplikace, která potřebuje storage
## Daemon set
- Pod se vytváří na každém node
- Vhodné například pro monitoring
## Cron / crontab
- Definuje opakované úlohy, které se spustí a poté ukončí

# Deployment spec
- Definuje žádaný stav, jak má aplikace běžící v klastru vypadat
- Kubernetes toto čte a spravuje workload podle této konfigurace (například restarty, škálování...)

# Service
- Abstrakce, která umožňuje přistupovat k aplikaci běžící na množině podů
- Například se může jednat o množinu podů s labelem `nginx`
- Kubernetes službě přiřazuje IP adresu, kterou poté používá Service Proxy
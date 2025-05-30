- Aplikační infrastruktura spravovaná skrze soubory definicí
- Tyto soubory jsou verzované
- Hlavními technologiemi jsou:
	- Nástroje na správu konfigurace - instalace na stroje, které již existují
		- Ansible, Chef, Puppet
	- Abstrakce cloudové infrastruktury

# Terraform
- Abstrakce [[Datové centrum|datového centra]] a přiřazených služeb
- Jedná se o deklarativní jazyk (HashiCorp Configuration Language)
## Postup
1. Popíšu chtěný stav pomocí HCL
	- Zde jsou instance, sítě, pravidla firewall apod.
2. Terraform vygeneruje exekuční plán k dosažení cílového stavu
3. Provede plán
- Jedná se o formu [[Infrastructure as Code (IaC)]]
- Zdroj pravdy je vždy verzován v Gitu
- Populární pro správu [[Kubernetes]]
- Nástroje pro toto jsou hlavně **ArgoCD**, nebo **Flux**
- Jedná se o vyšší vrstvu oproti například [[Infrastructure as Code (IaC)#Terraform|Terraformu]]
# Princip
- Máme soubory zvané **manifesty**
- Nástroj pro správu sleduje tyto manifesty a aplikuje jej na [[Kubernetes]] klastr
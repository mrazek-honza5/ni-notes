- Nekdy se označuje pojmem *Availability Domain*
- Jedná se o výpočetní prostředky na lokacích v rámci [[Region|regionu]]
- Může jich v jednom regionu existovat více, ale
	- Musí být oddělené
	- Musí být nezávislé
	- Mají mezi sebou silné propojení
	- Ale napájení by mělo být také nezávislé
![[Pasted image 20250530163033.png]]

# Virtualizace sítě
- Starší datacentra neměla síťový hardware přizpůsobený na virtualizaci
	- Kvůli tomu byl použitý virtuální switch implementovaný v softwaru
	- Kvůli tomu potom docházelo k ovlivňování mezi tenanty, neboť ten virtuální switch byl vytížený
- Kvůli tomu byla vyvinuta technologie Smart-NIC
	- Potřebuje pro fungování specializovaný hardware
	- Na úrovni HW jsou poté implementovány virtuální a fyzické funkce, které zajišťují oddělení sítě mezi virtuálními stroji 
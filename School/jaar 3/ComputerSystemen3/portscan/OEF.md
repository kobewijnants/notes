# portscan oef

1. Hoe werd het binaire logbestand portscan2021.pcap aangemaakt?

Door wireshark of tcpdump te gebruiken en dan op te slaan als pcap bestand.

2. Wat is het IP adres van de aanvaller?

   192.168.56.1

3. Wat is het IP adres van de target?

   192.168.56.103

4. Welke poorten staan open op de target?

poort 80, 22, 53

5. De target werd op minstens 4 manieren gescand. Geef de 4 methodes en leg uit hoe ze werken.

- TCP scan: de aanvaller stuurt een TCP pakket naar de target en kijkt of de target een SYN-ACK terugstuurt. Zo ja, dan is de poort open. Anders een RST pakket.

- stealth scan: SYN scan, de aanvaller stuurt een SYN pakket naar de target en kijkt of de target een SYN-ACK terugstuurt. Zo ja, dan is de poort open. Anders een RST pakket.

- XMAS scan: de aanvaller stuurt een pakket met de FIN, URG en PSH vlaggen gezet. Als de poort open is, dan wordt er geen reactie teruggestuurd. Als de poort gesloten is, dan wordt er een RST pakket teruggestuurd.

- FIN scan: de aanvaller stuurt een pakket met de FIN vlag gezet. Als de poort open is, dan wordt er geen reactie teruggestuurd. Als de poort gesloten is, dan wordt er een RST pakket teruggestuurd.

6. Welke scantool werd gebruikt om de target te scannen? Hoe weet je dit?

Nmap, snort gaf dit weer

7. Welke commando's heeft de aanvaller gebruikt? (geef deze zo volledig mogelijk met alle parameters)

TCP scan: nmap -sT
Stealth scan: nmap -sS
XMAS scan: nmap -sX
FIn scan: nmap -sF

8. Welk besturingssysteem draait de aanvaller?

linux 3.11 and newer

9. Stel snort in (zorg dat portscans gedetecteerd worden) en laat snort de portscan analyseren.
   Welke aanval(len) zie je in de logs?

XMAS scan

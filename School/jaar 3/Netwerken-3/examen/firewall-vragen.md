Firewall vragen

## Begrippen: IP spoofing, source routing, ICMP redirect, actieve FTP, Passieve FTP,statefull filtering, statefull inspection,

1. **IP Spoofing**

IP spoofing houdt in dat een aanvaller het bron-IP-adres van een netwerkpakket vervalst, waardoor het lijkt alsof het pakket afkomstig is van een legitiem of vertrouwd adres. Dit wordt vaak gebruikt om systemen te misleiden of te omzeilen, zoals firewalls of andere beveiligingsmechanismen die verkeer controleren op basis van IP-adressen. Het doel kan variëren van het verkrijgen van ongeoorloofde toegang tot het uitvoeren van Denial of Service (DoS)-aanvallen.

2. **Source Routing**

Source routing is een techniek waarbij de verzender van een netwerkpakket expliciet de route van het pakket bepaalt in plaats van de router deze beslissingen te laten nemen. Dit kan worden gebruikt voor diagnostische doeleinden, zoals tracering van de route die een pakket doorloopt. Aanvallers kunnen echter source routing misbruiken om firewalls te omzeilen en toegang te krijgen tot netwerken die normaal gesproken afgeschermd zijn. Veel moderne netwerken blokkeren source routing om deze risico’s te beperken.

3. **ICMP Redirect**

Een ICMP (Internet Control Message Protocol) redirect is een bericht dat wordt verzonden door een router om een host te informeren dat er een efficiëntere route is voor een bepaald pakket. Dit bericht zorgt ervoor dat de host zijn routeringstabel bijwerkt om in de toekomst pakketten via de betere route te sturen. Dit mechanisme kan echter worden misbruikt in aanvallen waarbij een aanvaller valse ICMP-redirects stuurt om het verkeer om te leiden naar een kwaadwillende host, wat bijvoorbeeld kan leiden tot man-in-the-middle-aanvallen.

4. **Actieve FTP**

Bij **actieve FTP (File Transfer Protocol)** maakt de client verbinding met de server vanaf een willekeurige poort boven 1023 en stuurt een _PORT_-commando naar de server met zijn poortnummer, zodat de server een verbinding kan initiëren. De server verbindt dan via poort 20 naar de opgegeven poort van de client om de gegevens over te dragen. Dit kan problemen opleveren bij firewalls, omdat de firewall meestal alleen de inkomende verbindingen vanaf poort 20 zou moeten toestaan, maar het moeilijk is om willekeurige poorten van clients te monitoren.

5. **Passieve FTP**

Bij **passieve FTP** initieert de client beide verbindingen. Nadat de client de controlerende verbinding met de server tot stand heeft gebracht, stuurt de server een _PASV_-commando. Hierdoor opent de server een willekeurige poort boven 1023 en wacht hij tot de client verbinding maakt met deze poort. Passieve FTP wordt vaak gebruikt wanneer de client zich achter een firewall bevindt, omdat het eenvoudiger is voor firewalls om uitgaande verbindingen te monitoren en beheren dan inkomende verbindingen.

6. **Stateful Filtering**

Stateful filtering is een beveiligingsmechanisme waarbij een firewall de toestand van netwerkverbindingen bijhoudt. De firewall onthoudt welke verbindingen legitiem zijn en maakt onderscheid tussen nieuwe en bestaande verbindingen. Door de toestand van een verbinding (zoals SYN, ACK, FIN) te kennen, kan de firewall pakketten toestaan of blokkeren op basis van deze informatie. Dit is een geavanceerder en veiliger filtermechanisme dan stateless filtering, dat alleen pakketten individueel controleert zonder rekening te houden met eerdere pakketten.

7. **Stateful Inspection**

Stateful inspection (ook bekend als dynamic packet filtering) is een techniek waarbij een firewall niet alleen kijkt naar de gegevens in afzonderlijke pakketten, maar ook de toestand en context van een hele verbinding analyseert. Dit betekent dat de firewall de status van een verbinding bijhoudt en alleen toestaat dat pakketten worden doorgelaten als ze deel uitmaken van een legitieme sessie. Dit zorgt voor een hogere mate van beveiliging, omdat verdachte verbindingen of sessies kunnen worden geïdentificeerd en geblokkeerd.

## Je krijgt een firewall regel (of enkele regels) en je moet de functie ervan kunnen verklaren (zeggen wat ze doen en hoe ze werken).

Hier is een verkorte uitleg van hoe je firewallregels kunt verklaren:

### Structuur van een firewallregel:

- **Bronadres (Source IP)**: Waar het verkeer vandaan komt.
- **Bestemmingsadres (Destination IP)**: Waar het verkeer naartoe gaat.
- **Protocol**: Het gebruikte protocol (bijv. TCP, UDP, ICMP).
- **Poorten**: Welke poorten open of geblokkeerd zijn (bijv. 80 voor HTTP).
- **Actie**: Of het verkeer wordt toegestaan of geblokkeerd.

### Voorbeelden:

#### cisco:

1. HTTP-verkeer toestaan

```
Allow TCP 192.168.1.0/24 any 80
```

- Staat TCP-verkeer van netwerk 192.168.1.0/24 naar elke bestemming toe op poort 80 (HTTP-verkeer).

2. DNS-verzoeken blokkeren

```
Deny UDP any any 53
```

- Blokkeert alle UDP-verzoeken naar poort 53 (DNS-verkeer).

3. ICMP toestaan

```
Allow ICMP any any
```

- Staat ICMP-verkeer (zoals ping) van elke bron naar elke bestemming toe.

4. SSH-verkeer toestaan naar specifiek IP

```
Allow TCP any 192.168.1.10 22
```

- Staat SSH-verkeer (poort 22) naar IP 192.168.1.10 toe.

#### iptables:

1. HTTP-verkeer toestaan

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

- Sta TCP-verkeer toe op poort 80 (HTTP).

2. SSH-verkeer blokkeren

```bash
iptables -A INPUT -p tcp --dport 22 -j DROP
```

- Blokkeer inkomend SSH-verkeer (poort 22).

3. DNS-verzoeken toestaan

```bash
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
```

- Sta uitgaande DNS-verzoeken (poort 53, UDP) toe.

4. Ping (ICMP) blokkeren

```bash
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

- Blokkeer inkomende ping-verzoeken.

5. Verkeer van een specifiek IP blokkeren

```bash
iptables -A INPUT -s 192.168.1.100 -j DROP
```

- Blokkeer al het verkeer van IP 192.168.1.100.

### Belangrijk:

- **Stateful filtering**: Let op of de firewall sessies bijhoudt.
- **Inkomend/uitgaand verkeer**: Regels kunnen verschillen voor in- en uitgaand verkeer.
- **Logging**: Houd logboeken bij om regeltriggers te volgen.

Zo kun je firewallregels snel begrijpen en uitleggen!

## Leg het verschil uit tussen de werking met een interne en een externe DNS

### Verschil tussen interne en externe DNS:

1. **Interne DNS**:

   - **Gebruikt binnen een privé-netwerk** (zoals een bedrijfsnetwerk).
   - **Resolutie van interne hostnamen**: Verzorgt de vertaling van interne domeinnamen naar IP-adressen van apparaten binnen het netwerk (bijv. printers, servers, interne applicaties).
   - **Niet toegankelijk vanaf het internet**: Alleen binnen het netwerk beschikbaar.
   - **Beveiliging**: Zorgt voor betere controle en beveiliging van interne netwerkbronnen.

   **Voorbeeld**: Een DNS-server die `intranet.company.local` vertaalt naar een lokaal IP-adres.

2. **Externe DNS**:

   - **Publiek toegankelijk via het internet**.
   - **Resolutie van openbare domeinnamen**: Verzorgt de vertaling van domeinnamen naar IP-adressen van externe websites en services.
   - **Beheerd door externe partijen** zoals internetproviders of DNS-diensten (bijv. Google DNS, Cloudflare).
   - **Openbaar en wereldwijd beschikbaar**: Iedereen kan het gebruiken voor toegang tot websites zoals `example.com`.

   **Voorbeeld**: Een externe DNS-server die `google.com` vertaalt naar het IP-adres van de Google-webserver.

### Kort samengevat:

- **Interne DNS** is voor privé-netwerken en vertaalt lokale namen.
- **Externe DNS** is voor internetgebruik en vertaalt publieke domeinnamen.

## Hoe werkt een redlan en een bluelan.

Dit zijn termen die vaak worden gebruikt in netwerken voor **beveiliging** en **isolatie van systemen**, vooral in scenario's zoals penetratietests, IT-security-infrastructuren of gesegmenteerde netwerken.

### **RedLAN**:

- **Doel**: Simuleert een onveilig of onbeveiligd netwerk.
- **Gebruikt door**: Aanvallers of testteams (zoals Red Teams) die proberen kwetsbaarheden te exploiteren.
- **Kenmerken**:

  - Bevat apparaten die risico lopen of worden gebruikt om kwetsbaarheden te testen.
  - Verbindt met het interne netwerk (BlueLAN) via gecontroleerde en gemonitorde toegangspunten zoals firewalls.
  - Aanvallen, penetratietests en exploitatiepogingen worden vanuit RedLAN uitgevoerd.

  **Gebruik**: In cybersecurity-tests voor het simuleren van aanvallen.

### **BlueLAN**:

- **Doel**: Simuleert een veilig of beschermend netwerk.
- **Gebruikt door**: Verdedigingsteams (zoals Blue Teams) om systemen te beschermen.
- **Kenmerken**:

  - Bevat gevoelige systemen die moeten worden beschermd tegen aanvallen.
  - Dit netwerk is goed beveiligd en gemonitord om aanvallen te detecteren en af te weren.
  - Interactie met het RedLAN wordt streng gecontroleerd om de veiligheid te waarborgen.

  **Gebruik**: In cybersecurity-tests om verdediging te testen en kwetsbaarheden te ontdekken.

### Kort samengevat:

- **RedLAN**: Onveilig, simuleert aanvallers en kwetsbare systemen.
- **BlueLAN**: Veilig, simuleert verdedigde netwerken en beschermde systemen.

## Wat betekent de optie RELATED bij het gebruik van FTP bij iptables?Geef een voorbeeld.

De optie **`RELATED`** in iptables verwijst naar netwerkverkeer dat verband houdt met een bestaande verbinding, maar geen deel uitmaakt van de oorspronkelijke verbinding. Bij FTP, vooral bij **actieve FTP**, speelt deze optie een belangrijke rol omdat FTP twee verbindingen gebruikt: een controleverbinding en een gegevensverbinding. De gegevensverbinding kan als **`RELATED`** worden beschouwd omdat deze verband houdt met de al bestaande controleverbinding.

### Werking van **`RELATED`** bij FTP:

- Bij **actieve FTP** opent de client een controleverbinding naar de server, en de server opent vervolgens een nieuwe gegevensverbinding terug naar de client.
- De firewall moet deze gegevensverbinding toestaan omdat deze gerelateerd is aan de bestaande FTP-controleverbinding. Dit is waar de **`RELATED`**-optie van pas komt: het laat gerelateerd verkeer toe zonder dat het een expliciete regel nodig heeft voor de gegevensverbinding.

### Voorbeeldregel in iptables:

```bash
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
```

- **`-m state`**: Gebruik de "stateful" module om de staat van verbindingen te volgen.
- **`--state RELATED,ESTABLISHED`**: Sta verkeer toe dat deel uitmaakt van een bestaande verbinding (`ESTABLISHED`) of gerelateerd is aan een bestaande verbinding (`RELATED`), zoals de FTP-gegevensverbinding.
- **`-j ACCEPT`**: Verkeer dat aan deze voorwaarden voldoet, wordt toegestaan.

### Uitleg:

- Deze regel zorgt ervoor dat gerelateerde FTP-gegevensverbindingen worden toegestaan zonder expliciete poortnummers te openen. Het stelt de firewall in staat om automatisch verkeer toe te staan dat verband houdt met bestaande FTP-sessies, zoals de gegevensoverdracht in actieve FTP.

In het kort: **`RELATED`** zorgt ervoor dat de firewall gerelateerde verbindingen, zoals de gegevensverbinding in FTP, toestaat zonder extra configuratie van poorten.

## Wat betekent de optie ESTABLISHED bij gebruik van TCP bij iptables?Geef een voorbeeld

De optie **`ESTABLISHED`** in iptables verwijst naar netwerkverkeer dat behoort tot een bestaande, eerder opgezette verbinding. Dit betekent dat de firewall alleen verkeer toestaat dat deel uitmaakt van een sessie die al is gestart, zoals antwoorden op verzoeken die een client eerder heeft verstuurd.

Bij **TCP**-verkeer wordt een verbinding opgezet via een **3-way handshake** (SYN, SYN-ACK, ACK). Zodra deze handshake voltooid is, beschouwt iptables de verbinding als **`ESTABLISHED`**.

### Werking van **`ESTABLISHED`** bij TCP:

- Wanneer een TCP-verbinding eenmaal is opgezet, worden verdere pakketten als **`ESTABLISHED`** beschouwd.
- Dit is handig om verkeer in beide richtingen (in- en uitgaand) toe te staan voor bestaande verbindingen zonder expliciete regels voor elk pakket.

### Voorbeeldregel in iptables:

```bash
iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT
```

- **`-m state`**: Gebruik de "stateful" module om de staat van verbindingen te volgen.
- **`--state ESTABLISHED`**: Sta inkomend verkeer toe dat deel uitmaakt van een al bestaande TCP-verbinding.
- **`-j ACCEPT`**: Verkeer dat aan deze voorwaarde voldoet, wordt toegestaan.

### Uitleg:

- Deze regel zorgt ervoor dat al het **terugkerende** verkeer dat behoort tot een eerder opgezette TCP-verbinding wordt toegestaan. Bijvoorbeeld: als een gebruiker een verzoek naar een webserver stuurt, zorgt deze regel ervoor dat de firewall de reactie van de server accepteert.

### Kort voorbeeld:

1. Een client stuurt een **SYN**-pakket om een verbinding op te zetten.
2. De server reageert met **SYN-ACK**.
3. De client beantwoordt met **ACK**, waarmee de verbinding is opgezet.
4. Alle volgende pakketten worden beschouwd als **`ESTABLISHED`**, en deze iptables-regel laat ze door.

### In het kort:

De **`ESTABLISHED`**-optie staat verkeer toe dat behoort tot een eerder opgezet TCP-verbinding, waardoor antwoorden op bestaande verzoeken (zoals webverkeer) worden doorgelaten zonder dat nieuwe verbindingen onnodig worden toegestaan.

## Wat betekent de optie ESTABLISHED bij gebruik van UDP bij iptables?

Bij **UDP**-verkeer werkt de optie **`ESTABLISHED`** in iptables iets anders dan bij TCP, omdat UDP een verbindingloos protocol is. Dit betekent dat er geen formele verbinding wordt opgezet zoals bij TCP, en er is geen "handshake" om de status van een verbinding bij te houden.

### Wat betekent **`ESTABLISHED`** bij UDP?

- In de context van UDP verwijst **`ESTABLISHED`** naar verkeer dat deel uitmaakt van een sessie die initieel is opgezet via een andere manier, zoals een controlestroom of een eerder ontvangen pakket.
- Aangezien UDP geen sessie- of verbindingsstatus bijhoudt, is de gebruikelijke toepassing van **`ESTABLISHED`** minder relevant voor UDP dan voor TCP.

### Toepassing in iptables:

- Hoewel de **`ESTABLISHED`** status niet volledig van toepassing is op UDP, kan het nog steeds worden gebruikt om UDP-gegevens te beheren die worden verzonden als onderdeel van een eerder ontvangen aanvraag of een verbinding.
- Dit betekent dat als een UDP-pakket wordt ontvangen, het nog steeds kan worden beschouwd als "gerelateerd" aan een eerdere aanvraag.

### Voorbeeldregel in iptables:

```bash
iptables -A INPUT -m state --state ESTABLISHED -j ACCEPT
```

- **`-m state`**: Gebruik de "stateful" module om de staat van verbindingen te volgen.
- **`--state ESTABLISHED`**: Dit staat ook toe dat UDP-verkeer wordt geaccepteerd dat verband houdt met eerder ontvangen UDP-pakketten (bijv. antwoorden op een verzoek).
- **`-j ACCEPT`**: Verkeer dat aan deze voorwaarde voldoet, wordt toegestaan.

### Voorbeeldscenario:

Stel dat een client een verzoek stuurt naar een DNS-server via UDP (poort 53). De server antwoordt met het bijbehorende IP-adres. Het antwoord van de server kan worden beschouwd als gerelateerd aan het oorspronkelijke verzoek, en de firewall kan het antwoord toestaan, zelfs zonder een sessie zoals bij TCP.

### In het kort:

- De **`ESTABLISHED`** optie is minder relevant voor UDP, maar kan nog steeds worden gebruikt om antwoorden op eerder ontvangen UDP-pakketten toe te staan.
- Omdat UDP geen sessiebeheer heeft, is de context van **`ESTABLISHED`** vooral van toepassing op de antwoorden die volgen op initiële aanvragen.

## Wat betekent de optie RELATED bij gebruik van ICMP bij iptables? Geef een voorbeeld.

De optie **`RELATED`** in iptables wordt gebruikt om netwerkverkeer toe te staan dat verband houdt met een bestaande verbinding, maar deze optie is meestal niet van toepassing op **ICMP** (Internet Control Message Protocol) zoals het is bij TCP of UDP.

### Wat betekent **`RELATED`** bij ICMP?

- **ICMP** wordt voornamelijk gebruikt voor foutmeldingen en diagnostische functies, zoals **ping** en **traceroute**.
- De **`RELATED`**-optie kan in beperkte zin worden toegepast, bijvoorbeeld voor ICMP-berichten die betrekking hebben op het gedrag van andere protocollen, maar dit is niet de gebruikelijke toepassing. Een voorbeeld hiervan kan een ICMP-foutbericht zijn dat wordt teruggestuurd na een poging om een TCP-verbinding op te zetten.

### Voorbeeldscenario:

Stel dat een client een TCP-verbinding probeert op te zetten naar een server, maar het IP-pakket wordt geblokkeerd vanwege een firewallregel. De server kan een **ICMP Destination Unreachable**-bericht terugsturen naar de client om aan te geven dat de bestemming onbereikbaar is. In dit geval zou je het ICMP-bericht als **`RELATED`** kunnen beschouwen, omdat het gerelateerd is aan de oorspronkelijke TCP-verbinding.

### Voorbeeldregel in iptables:

```bash
iptables -A INPUT -p icmp -m state --state RELATED -j ACCEPT
```

- **`-p icmp`**: Specificeert dat de regel van toepassing is op ICMP-verkeer.
- **`-m state`**: Gebruik de "stateful" module om de staat van verbindingen te volgen.
- **`--state RELATED`**: Laat ICMP-pakketten toe die gerelateerd zijn aan bestaande verbindingen of verzoeken.
- **`-j ACCEPT`**: Verkeer dat aan deze voorwaarde voldoet, wordt toegestaan.

### In het kort:

- De **`RELATED`**-optie bij ICMP is niet zo gebruikelijk of noodzakelijk als bij TCP of UDP.
- Het kan worden gebruikt om bepaalde ICMP-berichten (zoals foutmeldingen) toe te staan die verband houden met andere verbindingen, maar ICMP zelf werkt voornamelijk onafhankelijk van de concepten van verbindingen en status.

## Ik stuur met nemesis spoofed FIN en RST pakketten naar mijn firewall. Al mijn connecties in het interne netwerk worden afgesloten. Wat kan ik doen om dat te verhinderen?

Als je met **Nemesis** spoofed **FIN**- en **RST**-pakketten naar je firewall stuurt en dit leidt tot het afsluiten van al je verbindingen in het interne netwerk, zijn er verschillende stappen die je kunt ondernemen om dit te verhinderen. Dit soort aanvallen kan worden aangeduid als een **TCP reset attack** en kan schadelijk zijn voor netwerkdiensten. Hier zijn enkele maatregelen die je kunt nemen:

### 1. **Firewall Configuratie Versterken**:

- **Regels voor TCP-reset-pakketten**: Configureer je firewall om spoofed **FIN** en **RST**-pakketten te blokkeren. Dit kan door regels toe te voegen die verkeer met deze flags verbieden, vooral als het afkomstig is van niet-betrouwbare bronnen.
  ```bash
  iptables -A INPUT -p tcp --tcp-flags RST RST -s <spoofed_IP> -j DROP
  iptables -A INPUT -p tcp --tcp-flags FIN FIN -s <spoofed_IP> -j DROP
  ```

### 2. **Intrusion Detection/Prevention Systems (IDS/IPS)**:

- **Implementatie van IDS/IPS**: Gebruik een Intrusion Detection System (IDS) of Intrusion Prevention System (IPS) om verdachte activiteiten te monitoren en automatisch te blokkeren.
- Configureer de IDS/IPS om te reageren op anomalieën, zoals een abnormaal aantal **RST**- of **FIN**-pakketten die vanuit hetzelfde IP-adres worden verzonden.

### 3. **TCP Sequence Number Randomization**:

- **Gebruik TCP Sequence Number Randomization**: Dit kan het moeilijker maken voor aanvallers om succesvolle reset- of beëindigingsaanvallen uit te voeren.

### 4. **Versterking van Netwerkbeveiliging**:

- **Netwerksegmentatie**: Segmenteer je netwerk, zodat een aanval op één segment niet automatisch invloed heeft op andere segmenten.
- **Gebruik van VPN**: Beperk toegang tot interne netwerken via VPN's om ongewenste pakketten van externe bronnen te verminderen.

### 5. **Logging en Monitoring**:

- **Log Verkeer**: Zorg ervoor dat je netwerkverkeer goed gelogd wordt, zodat je verdachte activiteiten kunt identificeren.
- **Bewaking**: Monitor je netwerk actief om snel te reageren op verdachte activiteiten of aanvallen.

### 6. **Beperk Toegang**:

- **Beperk IP-toegang**: Sta alleen verkeer toe van bekende, betrouwbare IP-adressen en gebruik whitelisting waar mogelijk.
- **Rate Limiting**: Pas rate limiting toe op je firewall om het aantal pakketten dat van een specifieke bron kan worden ontvangen te beperken.

### 7. **Updates en Patches**:

- **Houd je systeem up-to-date**: Zorg ervoor dat je firewall en netwerkapparatuur up-to-date zijn met de laatste beveiligingspatches en updates om kwetsbaarheden te minimaliseren.

### Samenvatting:

Door je firewall te configureren om spoofed **FIN** en **RST**-pakketten te blokkeren, gebruik te maken van IDS/IPS, netwerksegmentatie toe te passen en actief te monitoren, kun je de impact van dit soort aanvallen aanzienlijk verminderen en de veiligheid van je netwerk verbeteren.

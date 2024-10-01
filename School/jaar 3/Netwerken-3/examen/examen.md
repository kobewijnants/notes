# Mogelijke vragen voor het examen Netwerken 3:

- PAPIER

- Het is belangrijker om te weten HOE iets werkt, dan om een afkorting van buiten
  te kennen.
- Er komen ook zeker vragen over het praktische gedeelte, dus daar kijk je best
  jouw oplossingen nog eens van na.
- Je moet Firewall regels algemeen, met iptables, met vyos of met cisco routers
  verstaan
- Vragen kunnen ook omgekeerd zijn (dwz het antwoord wordt een vraag)
- Je krijgt ook meer open vragen, waar je je mening moet funderen met antwoorden
  gespreid over de cursus:
  bv Teken een netwerkontwerp MET geldige IP adressen, waarbij je een gebruik maakt
  van een proxy server, webserver, firewall en externe DNS
  bv Geef 2 technieken die je zou gebruiken bij een nieuwe WiFi standaard.
- Je moet ook een netwerk kunnen ontwerpen met onderdelen/services die je in de les
  hebt gezien, inclusief IP adressering

- Veel theorie
- firewall regels
- tekenen van topology
- oefeningen (bepaalde commando's etc.)

## FIREWALLS

Begrippen: IP spoofing, source routing, ICMP redirect, actieve FTP, Passieve FTP,
statefull filtering, statefull inspection,
Je krijgt een firewall regel (of enkele regels) en je moet de functie ervan kunnen
verklaren (zeggen wat ze doen en hoe ze werken).
Leg het verschil uit tussen de werking met een interne en een externe DNS
Hoe werkt een redlan en een bluelan.
Wat betekent de optie RELATED bij het gebruik van FTP bij iptables?Geef een
voorbeeld.
Wat betekent de optie ESTABLISHED bij gebruik van TCP bij iptables?Geef een
voorbeeld
Wat betekent de optie ESTABLISHED bij gebruik van UDP bij iptables?
Wat betekent de optie RELATED bij gebruik van ICMP bij iptables? Geef een
voorbeeld.
Ik stuur met nemesis spoofed FIN en RST pakketten naar mijn firewall. Al mijn
connecties in het interne netwerk worden afgesloten. Wat kan ik doen om dat te
verhinderen?

## DSL, KABEL EN FIBER

Begrippen: FDM, TDM, STDM, simplex, half duplex, full duplex, baudrate en bitrate,
scrambling/unscrambling, Huffman Encoding, Lempel-Ziv Encoding, modulatie, AM, FM,
PM, QAM
Wat is het verschil tussen het gebruik van TDM en STDM
Leg uit hoe QAM modulatie werkt.
Waarom is er bij de meeste internetverbindingen een verschil in upstream en
downstream verkeer?
Wat is er verbeterd bij ISP's in verband met ruis?
Hoe werkt echo cancellation en wat kan je er mee doen?
Waarvoor staat de A bij ADSL en wat betekent deze?
Hoe werken subkanalen bij DMT modulatie?
Wat zijn de belangrijkste verbeteringen bij ADSL2+
Wat is een realtime Signaal/Ruis monitor?
Welke aanpassingen moesten er aan de kabel gebeuren voor het gebruik van de nieuwe
kabeldiensten ten opzichte van vroeger?
Wat is HFC?
Wat is dispersie en wat zijn silitonen?
Wat is the last mile in verband met Powerline technology?
Geef enkele voorbeelden van het gebruik van powerline technologie.
Geef de belangrijkste verschillen tussen een circuit en een packet switched netwerk

## IPv6

Begrippen:link local adres, unique local adres, fragmentatie, MTU, QoS, label
Welke 3 soorten adressen kent IPv6? Geef een voorbeeld van hun gebruik.
Waarvoor dienen zones?
Mijn Windows IPv6 adres ziet er als volgt uit: fe80::250:56ff:fec0:8%1 Wat betekent
fe80? Wat betekent %1? Wat betekent :: ?
Mijn IPv4 adres in windows is 169.254.12.134. Wat betekent dit? Hoe werkt dit
systeem in IPv6?
Waarom is het uitbreiden van de header een voordeel bij IPv6 tov IPv4?
Waarom bestaat 'Time To Live' niet meer bij IPv6?
Wat doe je als IPv4 de té grote pakketten niet kan slikken?
Hoe werkt stateless autoconfiguration?
Hoe werkt statefull autoconfiguration?
Hoe werkt autoconfiguratie bij mobiele toestellen?
Geef kort de 5 belangrijkste wijzigingen bij IPv4 tov IPv6.

## PROXY

Begrippen: Disk cache, non-cacheble, neighbour proxy, parent proxy, transparante
proxy, ICP
Wanneer kan een proxy server een HIT/MISS hebben?
Geef 3 voorbeelden van TTL waarden bij een proxy server.
Waarvoor wordt SOCKS gebruikt?
Waarvoor dient ICP?
Aan de hand van Firewalls, proxy en LAN-WAN verbindingen moet je in staat zijn om
een basis netwerk ontwerp te maken en verbeteren.

## 2G3G

Begrippen: TDMA, CDMA, HLR, VLR, soft-, softer- hard-handover
Je hebt in het buitenland een GSM gesprek. Hoe weet diegene die belt, waar je je
bevind?
Wat is tromboning? Kan er iets tegen gedaan worden?
Wat zijn de zwakheden bij de veiligheid van het huidige GSM netwerk?
Welke aanpassing was er nodig aan het GSM netwerk voor het gebruik van WAP?
Waarom is GPRS verkeer sneller dan GSM verkeer?
Wat zijn de nadelen van GPRS?

## 3G

Waar gebruikt UMTS FDD?
Waar gebruikt UMTS TDD?
Waarom is UMTS sneller dan GPRS?
Wat is CDMA spreading en despreading?
Hoe werkt OFDM?
Waarom is Power Control belangrijk bij UMTS?
Wat is het verschil tussen een soft en een softer handover?
Wat is cell breathing?
Hoe werkt een Rake receiver?

## 4G-5G

Begrippen: HARQ, Beamforming
Wat zijn Dual Mode GSM's?
ATM BGP FR MPLS
Begrippen:
ATM:
PVC, SVC, UNI, NNI, Cell Loss Priority,VCI, VPI, network provisioning, overlay
network

- Bespreek bondig de header van een ATM cel.
- Wat is het verschil tussen UNI en NNI? Waarom is er een verschil?
- Geef de soorten ATM Adaption Lagen en geef een voorbeeld van het gebruik bij elk
  type.
- Waarom is ATM niet meer zo populair?

## BGP

Begrippen: AS, CIDR, prefix
Mag je de nummer van een AS kiezen? Verklaar.
Wat is het voordeel van het gebruik van CIDR?
Hoe bepaalt BGP welke netwerken hij in zijn tabel opneemt?
Hoe kan BGP misbruikt worden om internet verkeer te onderscheppen?

## FRAME RELAY

Begrippen: DCE, DTE, SVC, PVC, DLCI
Vergelijk een circuit switched met een packet switched netwerk
Wat is het verschil tussen X.25 en Frame Relay?
Wat is het verschil tussen LAP-B en LAP-F?
Waarvoor wordt LMI gebruikt?
Vergelijk Frame Relay met ATM

## MPLS

Begrippen: LSP, LSR, LER, label substitution
Wat is label substitution?
Hoe kent MPLS een label toe bij ATM, Frame Relay of andere protocollen?
Hoe worden de MPLS labels doorgestuurd?
Op welke 2 manieren kan een LSP worden opgezet bij MPLS?
Wat is het verschil tussen een LER en een LSR bij MPLS?

## VOIP

Begrippen: SIP, STUN, PBX, RTP, SuperNode
Wat is de functie van SIP?
Hoe werkt ENUM
Hoe kan je het NAT/PAT probleem oplossen voor VoIP?
Hoe geraakt Skype door een firewall?
Hoe wordt een gesprek gestart tussen 2 VoiP toestellen?

## WIRELESS

Begrippen: MANET, batman, Multi-radio AP,cut-through, fast-forward, store-and-
forward switching, gain, beamwidth
Hoe werkt wireless roaming?
Wat zijn de voornaamste wijzigingen bij 802.11ac ten opzichte van het huidige
802.11n protocol?

## WIRELESS SECURITY

Begrippen: TKIP, MSCHAPv2, MIC, LEAP, PEAP, TLS
Wat zijn de nadelen van WEP
Wat of wie ;-) is Michael?
Hoe werkt het wireless KdG netwerk?
Wat is de functie van een RADIUS server?

## RFID

Begrippen: RFID tag, Actief, Passief, Semi-Actief
Wat zijn de nadelen van RFID
Geef 3 voorbeelden RFID een van Actief, Passief en Semi-Actief

## BLUETOOTH

Begrippen: frequency hopping, piconet en scatternet

## SNMP

Begrippen: SNMP agent, SNMP trap, MIB, ASN, OID
Geef een voorbeeld van een OID.
Waarom zou je SNMPv3 gebruiken en niet SNMPv2?
Waarom zou je SNMPv2 gebruiken en niet SNMPv3?
Wat doet volgend commando: createUser authOnlyUser MD5 "secret007"
Wat doet volgend commando: createUser authPrivUser SHA "geheim007" AES

## HTTP3

Begrippen: ossification, 0-RTT, TCP line blocking, Connection Migration, spin bit
Op welke manieren kan HTTP3 HTTP2 versnellen?
Wat is het probleem met het invoeren van een nieuw transportprotocol?
Hoe wordt HTTP3 beschikbaar gesteld zodat een browser kan overschakelen naar QUIC?

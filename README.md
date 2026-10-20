# ReteLab

Simulatore didattico di reti che funziona nel browser. È una versione molto semplificata di Cisco Packet Tracer: si disegna lo schema della rete, si configura ogni dispositivo (dal pannello oppure, sui router, con i comandi Cisco) e si verifica che la rete funzioni con `ping`, `tracert`, `arp -a`, `nslookup` e un piccolo browser.

**Apri il simulatore:** https://pg-laboratorio.github.io/retelab/

Non serve installare niente e non serve un account. Il lavoro viene salvato automaticamente nel browser che si sta usando.

---

## Cosa si può fare

| Dispositivo | Cosa si configura |
|---|---|
| **PC / Portatile** | Indirizzo IP statico o DHCP, subnet mask, gateway, server DNS. Indirizzo MAC. Terminale e browser. |
| **Stampante** | Indirizzo IP statico o DHCP: serve per gli esercizi con dispositivi a indirizzo fisso. |
| **Server** | Indirizzo IP, servizi **HTTP** (pagina web), **DNS** (record A) e **DHCP**. |
| **Switch** | 8 porte, VLAN per ogni porta. Il cavo tra due switch funziona da trunk. |
| **Router** | 4 interfacce **accese o spente** (shutdown), rotte statiche e rotta di default, **NAT**, server **DHCP** con pool, gateway (default-router) e indirizzi esclusi. Si configura dal pannello o con i **comandi Cisco**. Come sui router Cisco, le interfacce di un router nuovo sono spente. |
| **Firewall** | Interfacce WAN / LAN / DMZ, rotte, NAT e **regole permit/deny** lette dall'alto in basso. |
| **Internet** | Rappresenta la rete pubblica: non instrada gli indirizzi privati. |

Quando un pacchetto non arriva, il terminale spiega **perché**: gateway fuori dalla rete, nessuna rotta, interfaccia spenta, NAT mancante, VLAN diversa, regola del firewall, nome DNS sconosciuto e così via. I pacchetti viaggiano animati sullo schema, con un colore per ogni protocollo.

Ogni scheda di rete ha un **indirizzo MAC**. I dispositivi imparano i MAC con **ARP**: la richiesta broadcast lampeggia su tutti i dispositivi che la ricevono, così si vede fin dove arriva un dominio di broadcast. Dopo un ping, il pulsante **Pacchetti** mostra il viaggio tratto per tratto: MAC e IP di sorgente e destinazione, richieste ARP, chi le riceve e chi risponde.

Un **indirizzo IP duplicato** si comporta come in una rete vera: `ipconfig` lo segnala con «(Duplicato)» e i ping verso quell'indirizzo hanno risposte instabili. Anche un pool DHCP che comprende l'indirizzo di una stampante statica, prima o poi, crea il conflitto.

### Comandi del terminale

| PC | Router (stile Cisco IOS) |
|---|---|
| `ipconfig`, `ipconfig /all` | `enable`, `disable`, `configure terminal`, `end`, `exit` |
| `ipconfig /renew`, `ipconfig /release` | `show ip interface brief`, `show interfaces` |
| `arp -a`, `arp -d` | `show ip route`, `show arp`, `show running-config` |
| `ping <ip o nome>` | `show ip dhcp binding`, `show ip dhcp pool` |
| `tracert <ip o nome>` | `ping`, `traceroute` |
| `nslookup <nome>` | |
| `http <ip o nome>` | |

Comandi di configurazione del router, dopo `configure terminal`:

```
hostname R1
interface g0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 82.0.0.1
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp pool LAN_A
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
end
```

Funzionano anche le abbreviazioni (`conf t`, `int g0/0`, `no shut`, `sh ip int br`), `no` davanti a un comando per annullarlo e `do` per eseguire un `show` in modalità configurazione. Sul NAT: `ip nat outside` sull'interfaccia verso Internet.

`help` mostra l'elenco, `cls` pulisce lo schermo, le frecce ↑ ↓ richiamano i comandi precedenti.

---

## Come si usa

1. Aggiungi i dispositivi dall'elenco **Dispositivi**.
2. Scegli lo strumento **Cavo** e clicca due dispositivi per collegarli. Viene usata la prima porta libera; cliccando il cavo puoi cambiarla.
3. Clicca un dispositivo per aprirne la configurazione.
4. Apri il **Terminale** di un PC e verifica la rete con `ping` e `tracert`.
5. **Verifica rete** elenca gli errori di configurazione (IP duplicati, gateway sbagliati, interfacce spente, ecc.).
6. Dopo un ping, **Pacchetti** mostra i frame tratto per tratto.

Trascina lo sfondo per spostare la vista, usa la rotella o i pulsanti **− Adatta +** per lo zoom. **Annulla** (o `Ctrl+Z`) annulla l'ultima modifica.

### Consegnare il lavoro

- **Esporta → Scarica file .json** salva la rete in un file: scegli il nome, per esempio `Rossi_Es3`.
- **Importa → Apri un file .json** la ricarica identica.

In questo modo uno studente può consegnare la propria rete e il docente può aprirla e provarla.

### Modalità esercizio

Nella finestra **Esporta** si può attivare la **modalità esercizio** prima di distribuire una rete. Chi la apre non vede il pulsante **Verifica rete**, le spiegazioni «Perché» del terminale e gli indirizzi sotto i PC: i guasti si trovano solo con `ping`, `ipconfig`, `arp -a` e i comandi `show` del router, come nella realtà. È pensata per gli esercizi di ricerca guasti. Si disattiva cliccando l'etichetta «Modalità esercizio» in alto.

---

## Reti di esempio

Nella cartella [`esempi/`](esempi/) ci sono tre reti pronte da importare (le stesse del menu **Carica esempio**):

| File | Contenuto |
|---|---|
| `01-rete-domestica.json` | Router di casa con DHCP e NAT, tre dispositivi, accesso a un server web su Internet. |
| `02-rete-ufficio.json` | Due VLAN (uffici e ospiti), router interno, firewall con DMZ e regole già impostate. |
| `03-lan-da-configurare.json` | Uno switch e tre PC senza indirizzi: punto di partenza per il primo esercizio. |

Per usarle: scarica il file da GitHub (pulsante **Download raw file**), poi nel simulatore **Importa → Apri un file .json**.

### Idee per le esercitazioni

- **LAN base.** Partendo da `03-lan-da-configurare.json`, assegna gli indirizzi ai tre PC nella rete 192.168.0.0/24 e verifica con `ping`.
- **Due reti e un router.** Collega due LAN con un router, configura gateway e indirizzi, verifica con `tracert`.
- **Rete domestica.** Nella rete domestica disattiva il NAT e osserva cosa succede al `ping` verso Internet. Spiega perché.
- **DHCP.** Nella rete domestica escludi dal pool tutti gli indirizzi tranne due e aggiungi un quarto PC: cosa succede con `ipconfig /renew`?
- **MAC e ARP.** Svuota la tabella ARP con `arp -d`, fai un ping verso un'altra rete e confronta `arp -a` con il pulsante **Pacchetti**: quale MAC conosce il PC?
- **Firewall.** Nella rete d'ufficio, modifica le regole perché gli ospiti possano fare `ping` verso Internet ma continuino a non vedere l'intranet.

---

## Pubblicare con GitHub Pages

Il simulatore è un solo file, `index.html`, senza dipendenze da installare.

1. In **Settings → Pages**, alla voce *Source* scegli **Deploy from a branch**.
2. Seleziona il branch `main` e la cartella `/ (root)`, poi **Save**.
3. Dopo un minuto il simulatore è online all'indirizzo `https://<utente>.github.io/retelab/`.

Funziona anche aprendo `index.html` direttamente dal computer, con un doppio clic.

---

## Licenza

- Il **codice** (`index.html`) è distribuito con licenza [MIT](LICENSE).
- I **contenuti didattici** (questo README, le reti di esempio e le esercitazioni) sono distribuiti con licenza [CC BY-NC-SA 4.0](LICENSE-CC-BY-NC-SA.md).

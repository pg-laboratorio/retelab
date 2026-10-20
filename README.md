# ReteLab

Simulatore didattico di reti che funziona nel browser. È una versione molto semplificata di Cisco Packet Tracer: si disegna lo schema della rete, si configura ogni dispositivo e si verifica che la rete funzioni con `ping`, `tracert`, `nslookup` e un piccolo browser.

**Apri il simulatore:** https://pg-laboratorio.github.io/retelab/

Non serve installare niente e non serve un account. Il lavoro viene salvato automaticamente nel browser che si sta usando.

---

## Cosa si può fare

| Dispositivo | Cosa si configura |
|---|---|
| **PC / Portatile** | Indirizzo IP statico o DHCP, subnet mask, gateway, server DNS. Terminale e browser. |
| **Server** | Indirizzo IP, servizi **HTTP** (pagina web), **DNS** (record A) e **DHCP**. |
| **Switch** | 8 porte, VLAN per ogni porta. Il cavo tra due switch funziona da trunk. |
| **Router** | 4 interfacce, rotte statiche e rotta di default, **NAT**, server **DHCP** per interfaccia. |
| **Firewall** | Interfacce WAN / LAN / DMZ, rotte, NAT e **regole permit/deny** lette dall'alto in basso. |
| **Internet** | Rappresenta la rete pubblica: non instrada gli indirizzi privati. |

Quando un pacchetto non arriva, il terminale spiega **perché**: gateway fuori dalla rete, nessuna rotta, NAT mancante, VLAN diversa, regola del firewall, nome DNS sconosciuto e così via. I pacchetti viaggiano animati sullo schema, con un colore per ogni protocollo.

### Comandi del terminale

| PC e server | Router e firewall |
|---|---|
| `ipconfig` | `show ip route` |
| `ipconfig /renew`, `ipconfig /release` | `show ip interface brief` |
| `ping <ip o nome>` | `show nat` |
| `tracert <ip o nome>` | `ping`, `tracert` |
| `nslookup <nome>` | |
| `http <ip o nome>` | |

`help` mostra l'elenco, `cls` pulisce lo schermo, le frecce ↑ ↓ richiamano i comandi precedenti.

---

## Come si usa

1. Aggiungi i dispositivi dall'elenco **Dispositivi**.
2. Scegli lo strumento **Cavo** e clicca due dispositivi per collegarli. Viene usata la prima porta libera; cliccando il cavo puoi cambiarla.
3. Clicca un dispositivo per aprirne la configurazione.
4. Apri il **Terminale** di un PC e verifica la rete con `ping` e `tracert`.
5. **Verifica rete** elenca gli errori di configurazione (IP duplicati, gateway sbagliati, ecc.).

Trascina lo sfondo per spostare la vista, usa la rotella o i pulsanti **− Adatta +** per lo zoom. **Annulla** (o `Ctrl+Z`) annulla l'ultima modifica.

### Consegnare il lavoro

- **Esporta → Scarica file .json** salva la rete in un file.
- **Importa → Apri un file .json** la ricarica identica.

In questo modo uno studente può consegnare la propria rete e il docente può aprirla e provarla.

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
- **DHCP.** Riduci il pool del router a due indirizzi e aggiungi un quarto PC: cosa succede con `ipconfig /renew`?
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

© 2026 Pietro Gardoni, IISS Bazoli-Polo

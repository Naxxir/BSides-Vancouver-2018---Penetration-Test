# 🛡️ BSides Vancouver 2018 - Penetration Test

Penetration test completo su una macchina virtuale vulnerabile (BSides Vancouver 2018 CTF VM), eseguito in laboratorio controllato come esercizio di formazione in ambito cybersecurity. Il sistema target è stato compromesso completamente tramite **due catene di attacco indipendenti**, entrambe culminate nell'ottenimento di privilegi di root.

## 🎯 Obiettivo

Identificare e sfruttare le vulnerabilità presenti sulla macchina target fino all'ottenimento di privilegi amministrativi (root), documentando ogni fase secondo una metodologia standard di penetration testing: ricognizione, enumerazione, exploitation e privilege escalation.

## 🖥️ Ambiente di Test

| Elemento | Dettaglio |
| --- | --- |
| Macchina target | `192.168.56.101` – BSides Vancouver 2018 VM |
| Macchina attaccante | `192.168.56.102` – Kali Linux |
| Virtualizzazione | Oracle VirtualBox, rete Host-Only (DHCP) |
| Servizi esposti | FTP (21), SSH (22), HTTP (80) |

> ⚠️ Test condotto esclusivamente in ambiente virtuale isolato, a scopo didattico. Nessuna rete o sistema reale è stato coinvolto.

## 🛠️ Strumenti Utilizzati

`arp-scan` · `nmap` · `dirb` · `ftp` · `hydra` · `ncrack` · Metasploit Framework / `meterpreter` · `msfvenom` · WordPress Theme Editor (webshell PHP)

## 🔓 Percorsi di Attacco

### Percorso 1 — WordPress → Root
Enumerazione di un backup WordPress dimenticato (rivelato da `robots.txt`) → bruteforce delle credenziali admin con **Hydra** → **RCE autenticata** tramite l'editor dei temi WordPress (scrittura di una webshell PHP) → scoperta di uno script (`cleanup`) scrivibile da chiunque ed eseguito periodicamente da root → sovrascrittura con payload `msfvenom` → sessione **meterpreter** con privilegi di root.

### Percorso 2 — SSH Bruteforce → Root
Enumerazione utenti tramite accesso **FTP anonimo** (file `users.txt.bk`) → bruteforce delle credenziali SSH dell'utente `anne` con **Ncrack** → login SSH → policy `sudoers` eccessivamente permissiva (`ALL : ALL) ALL`) → escalation diretta a root con `sudo su`.

Entrambi i percorsi si concludono con la lettura del file `flag.txt`, a conferma della compromissione completa del sistema.

## 📂 Struttura del Repository

```
.
├── README.md
├── report/
│   └── BSides-Vancouver-2018-Pentest-Report.pdf
└── screenshots/
    ├── 01-recon/
    ├── 02-percorso1-wordpress/
    └── 03-percorso2-ssh/
```

## 📄 Report Completo

Il report dettagliato, comprensivo di tutte le evidenze (screenshot), analisi delle vulnerabilità e raccomandazioni di remediation, è disponibile nella cartella [`report/`](./report).

## ⚠️ Disclaimer

Questo progetto è stato realizzato **esclusivamente a scopo didattico e di formazione personale**, su una macchina virtuale appositamente predisposta per attività di CTF/training, in un ambiente di laboratorio isolato. Le tecniche descritte non devono essere utilizzate contro sistemi reali senza autorizzazione esplicita.

## 👤 Autore

**Daniele Perri** — Studente di Cybersecurity (Epicode)

- GitHub: [@Naxxir](https://github.com/Naxxir)
- LinkedIn: [daniele-perri-cybersecurity-specialist](https://www.linkedin.com/in/daniele-perri-cybersecurity-specialist)

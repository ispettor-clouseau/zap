# Risoluzione Problema OWASP ZAP su Ubuntu

## L'Errore
Quando si tenta di avviare il browser da ZAP, compare l'errore:
> **"ZAP your browser profile cannot be loaded"**

## La Causa
Sulle versioni recenti di Ubuntu (dalla 22.04 in poi), Firefox è installato di default come pacchetto **Snap**. I pacchetti Snap girano in un ambiente isolato (sandbox) per motivi di sicurezza. Questo impedisce al browser di accedere ai profili temporanei creati da ZAP (solitamente generati all'interno della cartella `/tmp`).

## La Soluzione: Installare la versione nativa di Firefox (.deb)
La soluzione definitiva consiste nel rimuovere la versione Snap e installare la versione `.deb` ufficiale tramite il PPA di Mozilla.

### Passaggi:

**1. Rimuovi la versione Snap di Firefox:**
```bash
sudo snap remove firefox
```

**2. Aggiungi il repository (PPA) ufficiale di Mozilla:

```bash
sudo add-apt-repository ppa:mozillateam/ppa
```
**3. Modifica la priorità dei pacchetti (Apt Pinning):


```bash
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001

Package: firefox
Pin: version 1:1snap1-0ubuntu2
Pin-Priority: -1
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
```

**4. Aggiorna i repository e installa Firefox:
```bash
sudo apt update
sudo apt install firefox
```

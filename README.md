# Django Deployment

GitHub Actions workflow per il deploy automatico di applicazioni Django su server self-hosted.

## Come funziona

Il workflow si attiva ad ogni push sul branch `main` ed esegue i seguenti step sul runner `ubuntustrike1`:

1. **Pull codice** — aggiorna il repository in `/var/www/$APP_NAME`
2. **Installa dipendenze** — esegue lo script `~/install.sh`
3. **Migrazione database** — esegue `python manage.py migrate`
4. **Raccolta file statici** — esegue `python manage.py collectstatic`
5. **Riavvia servizio** — riavvia il servizio systemd dell'applicazione

## Configurazione

### Variabili richieste

Impostare nel repository GitHub (Settings → Secrets and variables → Actions):

| Variabile | Descrizione |
|-----------|-------------|
| `APP_NAME` | Nome dell'applicazione (usato come nome directory e nome servizio systemd) |

### Prerequisiti sul server

- Runner GitHub Actions self-hosted registrato come `ubuntustrike1`
- Applicazione Django presente in `/var/www/$APP_NAME`
- Virtual environment Python in `/var/www/$APP_NAME/.venv`
- Script di installazione dipendenze in `~/install.sh`
- Servizio systemd configurato con il nome `$APP_NAME`
- Permessi sudo per il runner per eseguire `systemctl restart`

## Utilizzo

1. Aggiungere questo file come `.github/workflows/deploy.yml` nel repository dell'applicazione Django
2. Configurare la variabile `APP_NAME`
3. Ogni push su `main` avvierà il deploy automatico

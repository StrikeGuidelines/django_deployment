# Django Deployment

GitHub Actions workflow for automatic deployment of Django applications on a self-hosted server.

## How it works

The workflow triggers on every push to the `main` branch and runs the following steps on the `ubuntustrike1` runner:

1. **Pull code** — updates the repository at `/var/www/$APP_NAME`
2. **Install dependencies** — runs the `~/install.sh` script
3. **Database migration** — runs `python manage.py migrate`
4. **Collect static files** — runs `python manage.py collectstatic`
5. **Restart service** — restarts the application's systemd service

## Configuration

### Required variables

Set in the GitHub repository (Settings → Secrets and variables → Actions):

| Variable | Description |
|----------|-------------|
| `APP_NAME` | Application name (used as the directory name and systemd service name) |

### Server prerequisites

- GitHub Actions self-hosted runner registered as `ubuntustrike1`
- Django application present at `/var/www/$APP_NAME`
- Python virtual environment at `/var/www/$APP_NAME/.venv`
- Dependency installation script at `~/install.sh`
- systemd service configured with the name `$APP_NAME`
- sudo permissions for the runner to execute `systemctl restart`

## Usage

1. Add this file as `.github/workflows/deploy.yml` in the Django application repository
2. Configure the `APP_NAME` variable
3. Every push to `main` will trigger an automatic deploy

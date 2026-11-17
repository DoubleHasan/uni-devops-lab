# University Student Portal - DevOps Lab

## Layihe Haqqinda
Bu layihe Flask ve Redis esasli telebe portalinin Git, Docker Compose ve Ansible vasitesile avtomatlasdirilmis sekilde yerlesdirilmesini (deployment) heyata kecirir.

## Struktur
- app/: Flask veb tetbiqi (/, /health, /visits) ve requirements.txt
- Dockerfile: python:3.12-slim esasli konteyner konfiqurasiyasi
- compose.yml: Portal ve persistent redis_data volume ile isleyen Redis servisi
- ansible/: inventory.ini, vars.yml ve idempotent deploy.yml playbook-u

## Ise Salma Qaydasi
cd ansible && ansible-playbook -i inventory.ini deploy.yml

## Qarsilasilan Problem ve Helli
- Problem: Konteyner esasli App Server daxilinde Docker daemon ise salindiqdan sonra docker compose build zamani 'failed to solve: mount source: overlay ... invalid argument' xetasi yarandi.
- Sebeb: Konteyner daxilinde Docker (Docker-in-Docker) muhitinde standart overlay2 storage driver-i ic-ice overlay fayl sistemini desteklemir.
- Hell: Ansible deploy.yml playbook-unda Docker daemon-un --storage-driver=vfs parametri ile basladilmasi temin edildi ve problem hell olundu.

# n8n with Supabase and Ngrok

1. Start Ngrok
  - `ngrok http http://localhost:5678`
2. Update .env file with new ngrok subdomain name
3. Start full stack
  - `docker compose up -d`
4. Web UIs
  - [n8n](http://localhost:5678)
  - [Supabase](http://localhost:8000)
  - [Open WebUI](http://localhost:3000)
  - [Ngrok Web Interface](http://127.0.0.1:4040)

# Project setup

- Supabase Docker
  1. This is a minimal Docker Compose setup for self-hosting Supabase.
  2. Follow the steps [here](https://supabase.com/docs/guides/hosting/docker) to get started.
- n8n
  1. Add n8n to docker compose
  2. Update .env file with n8n vars

# Backup

## Workflows
- `docker exec -it n8n n8n export:workflow --backup --output=backups/latest/`
- `docker cp n8n:/home/node/backups/latest ./archive/backups/latest`
- Cleanup
- `docker exec -it n8n rm -rf /home/node/backups`

## Credentials
- `docker exec -it n8n n8n export:credentials --all --decrypted --output=creds.json`
- `docker cp n8n:/home/node/creds.json tmp/`
- Copy to keepassxc
- Cleanup
- `shred -u tmp/creds.json`
- `docker exec -it n8n shred -u /home/node/creds.json`

# Import
- Workflows
- `docker cp ./archive/backups n8n:/tmp`
- `docker exec -it n8n n8n import:workflow --separate --input=/tmp/backups`
- Credentials
- Copy the creds.json file from keepassxc to /tmp/creds.json
- `docker cp ./tmp/creds.json n8n:/tmp`
- `docker exec -it n8n n8n import:credentials --input=/tmp/creds.json`
- Clean Up
- `docker exec -it n8n shred -u /tmp/creds.json`
- `docker exec -it n8n rm -rf /tmp/backups`

# n8n-local

Local install of N8N using Docker and Ngrok

## Start up

- Turn on VPN (a fix for currently api.telegram.org is DNS failing)
- Start ngrok tunnel (for external web hooks)
- `ngrok http http://localhost:5678`
- Copy the ngrok subdomain to .env file
- Start n8n
- `docker compose -f docker-compose_local.yml up`

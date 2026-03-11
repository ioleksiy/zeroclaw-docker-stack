# ZeroClaw Docker Swarm Stacks for Portainer

This repository provides Docker Swarm stack files for deploying ZeroClaw with Portainer.

The stacks use the pre-built image published from:
https://github.com/ioleksiy/zeroclaw-docker

Upstream ZeroClaw project:
https://github.com/zeroclaw-labs/zeroclaw

## Stack Files

- docker-compose.yml
	- Standard ZeroClaw stack.
	- Exposes ZeroClaw on a host port (default 42617).

- docker-compose.cloudflare.yml
	- Same ZeroClaw service plus a Cloudflare Tunnel sidecar.
	- Intended for access through Cloudflare Tunnel.
	- Port exposure is still kept for direct access/debugging.

Both files are Swarm-compatible and use deploy: sections. They do not use build: because the image is pre-built and published externally.

## Environment Variables

Set these in Portainer Stack environment variables (or via .env when testing locally).

### ZeroClaw

- API_KEY
	- ZeroClaw API key for gateway access/auth.
- PROVIDER (default: anthropic)
	- LLM provider used by ZeroClaw.
- ANTHROPIC_API_KEY
	- API key for Anthropic when PROVIDER=anthropic.
- ZEROCLAW_ALLOW_PUBLIC_BIND (default: true)
	- Allows binding to public interfaces.
- ZEROCLAW_GATEWAY_PORT (default: 42617)
	- Internal ZeroClaw gateway port.

### Git

- GIT_USER_NAME
	- Git author name used by ZeroClaw.
- GIT_USER_EMAIL
	- Git author email used by ZeroClaw.
- GITHUB_TOKEN
	- GitHub token used for repository access/automation.

### Network

- HOST_PORT (default: 42617)
	- Host port mapped to container port 42617.

### Cloudflare (docker-compose.cloudflare.yml only)

- CLOUDFLARE_TUNNEL_TOKEN
	- Cloudflare Tunnel token consumed by cloudflared.

## Portainer Deployment

1. In Portainer, go to Stacks -> Add Stack.
2. Choose the Repository method and point to this repository.
3. Select the compose file to deploy:
	 - docker-compose.yml, or
	 - docker-compose.cloudflare.yml
4. Add the required environment variables in the Portainer UI.
5. Deploy the stack.

## Notes

- Portainer can read .env.example as a reference, but real values should be set in the Portainer Stack environment UI.
- The Cloudflare variant keeps port mapping on ZeroClaw for optional direct/debug access while primary traffic can go through the tunnel.
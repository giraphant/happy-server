# Happy Server

Minimal backend for open-source end-to-end encrypted Claude Code clients.

## What is Happy?

Happy Server is the synchronization backbone for secure Claude Code clients. It enables multiple devices to share encrypted conversations while maintaining complete privacy - the server never sees your messages, only encrypted blobs it cannot read.

## Features

- 🔐 **Zero Knowledge** - The server stores encrypted data but has no ability to decrypt it
- 🎯 **Minimal Surface** - Only essential features for secure sync, nothing more  
- 🕵️ **Privacy First** - No analytics, no tracking, no data mining
- 📖 **Open Source** - Transparent implementation you can audit and self-host
- 🔑 **Cryptographic Auth** - No passwords stored, only public key signatures
- ⚡ **Real-time Sync** - WebSocket-based synchronization across all your devices
- 📱 **Multi-device** - Seamless session management across phones, tablets, and computers
- 🔔 **Push Notifications** - Notify when Claude Code finishes tasks or needs permissions (encrypted, we can't see the content)
- 🌐 **Distributed Ready** - Built to scale horizontally when needed

## How It Works

Your Claude Code clients generate encryption keys locally and use Happy Server as a secure relay. Messages are end-to-end encrypted before leaving your device. The server's job is simple: store encrypted blobs and sync them between your devices in real-time.

## Hosting

**You don't need to self-host!** Our free cloud Happy Server at `happy-api.slopus.com` is just as secure as running your own. Since all data is end-to-end encrypted before it reaches our servers, we literally cannot read your messages even if we wanted to. The encryption happens on your device, and only you have the keys.

That said, Happy Server is open source and self-hostable if you prefer running your own infrastructure. The security model is identical whether you use our servers or your own.

## Self-Hosting with Docker Compose

The easiest way to self-host Happy Server is using Docker Compose, which sets up all required services automatically.

### Prerequisites

- Docker and Docker Compose installed
- At least 2GB of RAM available

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/giraphant/happy-server.git
   cd happy-server
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   # Edit .env and set a secure HANDY_MASTER_SECRET
   ```

3. **Start all services**
   ```bash
   docker compose up -d
   ```

4. **Run database migrations** (first time only)
   ```bash
   docker compose run --rm migrate
   ```

5. **Verify the server is running**
   ```bash
   curl http://localhost:3005/health
   ```

### Services

The Docker Compose setup includes:

| Service | Port | Description |
|---------|------|-------------|
| server | 3005 | Happy Server API |
| postgres | 5432 | PostgreSQL database |
| redis | 6379 | Real-time sync & caching |
| minio | 9000, 9001 | S3-compatible file storage |

### Configuration

Key environment variables (see `.env.example` for all options):

| Variable | Required | Description |
|----------|----------|-------------|
| `HANDY_MASTER_SECRET` | Yes | Secret key for encryption (use a strong random string) |
| `S3_PUBLIC_URL` | Yes | Public URL for accessing uploaded files |

### Production Considerations

For production deployments:

1. **Use strong secrets**: Generate a secure `HANDY_MASTER_SECRET`
   ```bash
   openssl rand -base64 32
   ```

2. **Set up a reverse proxy**: Use nginx or Caddy to handle TLS/SSL

3. **Configure S3_PUBLIC_URL**: Point to your public-facing file URL

4. **Back up your data**: The volumes `postgres_data`, `redis_data`, and `minio_data` contain all persistent data

### Connecting Clients

Configure your Happy clients to use your self-hosted server:

- **Mobile app**: Enable developer mode (tap version 10 times in Settings), then set custom server URL
- **CLI**: Set `HAPPY_SERVER_URL=http://your-server:3005` environment variable

## License

MIT - Use it, modify it, deploy it anywhere.
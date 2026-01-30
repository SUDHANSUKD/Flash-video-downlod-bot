# Bot Credentials & Deployment Guide

## All Credentials Extracted from Code

### Bot Token
```
BOT_TOKEN=
```

### Spotify API
```
SPOTIFY_CLIENT_ID=
SPOTIFY_CLIENT_SECRET=
```

### Redis Database (Upstash)
```
REDIS_URL=
REDIS_TOKEN=
```

### Proxies (6 proxies)
```
PROXIES=
```

### Sticker IDs (Optional)
```
IG_STICKER=
YT_STICKER=
PIN_STICKER=
MUSIC_STICKER=
```

## Quick Deployment on VPS

### Method 1: Using .env file (Recommended)
1. Copy the `.env` file to your VPS
2. Make sure it's in the same directory as `main.py`
3. Run: `python main.py`

### Method 2: Export as Environment Variables
```bash
export BOT_TOKEN=""
export SPOTIFY_CLIENT_ID=""
export SPOTIFY_CLIENT_SECRET=""
export REDIS_URL="https://together-snail-28026.upstash.io"
export REDIS_TOKEN=""
export PROXIES=""
```

### Method 3: Using systemd service
Create `/etc/systemd/system/nagu-bot.service`:
```ini
[Unit]
Description=
After=network.target

[Service]
Type=simple
User=your_user
WorkingDirectory=/path/to/bot
Environment="BOT_TOKEN="
Environment="SPOTIFY_CLIENT_ID="
Environment="SPOTIFY_CLIENT_SECRET="
Environment="REDIS_URL=https://together-snail-28026.upstash.io"
Environment="REDIS_TOKEN="
Environment="PROXIES="
ExecStart=/usr/bin/python3 main.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable nagu-bot
sudo systemctl start nagu-bot
```

## Docker Deployment

The bot already has a Dockerfile. Build and run:
```bash
docker build -t nagu-bot .
docker run -d \
  -e BOT_TOKEN="" \
  -e SPOTIFY_CLIENT_ID="" \
  -e SPOTIFY_CLIENT_SECRET="" \
  -e REDIS_URL="https://together-snail-28026.upstash.io" \
  -e REDIS_TOKEN="" \
  -e PROXIES="" \
  --name nagu-bot \
  nagu-bot
```

## Important Notes

1. **Security**: The `.env` file is in `.gitignore` and won't be committed to Git
2. **Redis**: Free tier is sufficient for management features
3. **Proxies**: All 6 proxies are included and will be rotated automatically
4. **Picture**: Add your custom image to `assets/picture.png` for premium start message

## Testing

After deployment, test with:
- `/start` - Should show welcome message (with image if added)
- `/help` - Should show all commands
- Send any Instagram/YouTube/Spotify link
- Try management commands in a group

## Troubleshooting

If bot doesn't start:
1. Check if all environment variables are set: `env | grep BOT_TOKEN`
2. Check Redis connection: `curl https://together-snail-28026.upstash.io`
3. Check logs: `tail -f /var/log/nagu-bot.log`
4. Verify Python dependencies: `pip install -r requirements.txt`

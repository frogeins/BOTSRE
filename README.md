# Discord Bot for War Thunder SRE/SQB Community

This is a Discord bot designed for War Thunder, providing various features such as squadron management, game logging, and stat tracking.

## Usage

### Prerequisites

- Python 3.10 or higher
- Dependencies from `requirements.txt`

### Running the Bot

You can start the bot by running `BotScript.py` once you put your discord bot token into your env, this bot is currently meant to be ran inside Replit.

```bash
python3 BotScript.py
```

## Deploying on Railway (Pre-Deploy Checklist)

Before deploying to Railway, verify all of the following:

1. **Required runtime files are in your deploy source**
   - `BotScript.py`
   - `AutoLog.py`
   - `Parse_Replay.py`
   - `Data_Parser.py` runtime dependencies/assets (including `lang.vromfs.bin`)
   - required data dirs/files (`src_send/.../VROMFs`, `ICONS/`, `MAPS/`, `fonts/`, `char.vromfs.bin`)

2. **Set Railway environment variables**
   - `DISCORD_KEY`
   - `DEEPL_KEY`
   - `SID`
   - `REPLIT_OBJECT_STORAGE_BUCKET_ID` (required on Railway if you are using your own Replit Object Storage bucket)

3. **Install dependencies**
   - Railway must install `requirements.txt` (includes `discord-py`, `deepl`, `replit`, `replit-object-storage`, etc.)

4. **Configure object storage**
   - The bot persists state in object storage keys such as:
     - `SQUADRONS.json`
     - `SESSIONS.json`
     - `BILLING.json`
     - `PREFERENCES/...`
   - Railway does not provide Replit Object Storage natively. You must either:
     - provide valid Replit Object Storage credentials + `REPLIT_OBJECT_STORAGE_BUCKET_ID` for a Replit bucket your bot can access, or
     - replace `replit.object_storage` usage with a Railway-compatible backend (for example, S3-compatible storage or a database) before production deployment.

5. **Set process start command**
   - Use `python3 BotScript.py` (included via `Procfile` as `worker: python3 BotScript.py`)

6. **Smoke test before production**
   - process starts successfully
   - bot logs in to Discord
   - slash-command sync works
   - object storage read/write succeeds
   - DeepL translation path works

7. **Monitor first production rollout**
   - verify rate-limit behavior
   - verify memory usage during leaderboard/recurring tasks
   - verify replay cleanup and retention behavior

## Main Features
### Alarms
 - Monitor squadron changes and notify when members leave with points, point changes, and automatic logs.

### Comp
 - Given an enemy username, return the composition of their team's vehicles from the last known game.

### Translation
 - Useful for international squadrons, be sure to toggle the feature!

### General Squadron / Player Information
 - SQ-Info, Top, Track, and Stat should be useful for that.

# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies. Contains a full Discord moderation + ticket bot.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Discord bot**: discord.js v14

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
- `pnpm --filter @workspace/discord-bot run start` — run Discord bot

## Discord Bot (`artifacts/discord-bot`)

A full-featured Discord moderation and ticket system bot.

### Required Secrets

- `DISCORD_TOKEN` — Bot token from Discord Developer Portal
- `CLIENT_ID` — Application/Client ID from Discord Developer Portal
- `GUILD_ID` — Server ID (for instant slash command registration)

### Optional Env Vars (IDs copied from original config)

- `OWNER_ID`, `SUPPORT_CATEGORY_ID`, `WHITELIST_ROLE_ID`, `STAFF_ROLE_ID`
- `STAFF_PING_CHANNEL_ID`, `LOGS_CHANNEL_ID`
- `TRANSCRIPT_PING_CHANNEL_ID`, `TRANSCRIPT_FILE_CHANNEL_ID`
- `AUDIT_LOG_CHANNEL_ID` — Where all server actions are logged
- `MOD_LOG_CHANNEL_ID` — Where moderation actions are logged
- `AUTOMOD_ENABLED`, `SPAM_THRESHOLD`, `SPAM_WINDOW_MS`, `AUTOMOD_MUTE_MS`
- `BANNED_WORDS` — Comma-separated list of banned words

### Slash Commands

- `/ban` — Ban a user (with optional message delete days)
- `/unban` — Unban a user by ID
- `/kick` — Kick a user
- `/mute` — Timeout a user (e.g. `10m`, `1h`, `2d`)
- `/unmute` — Remove timeout from a user
- `/warn` — Warn a user (sends DM, logs it)
- `/warnings` — View warnings for a user
- `/purge` — Bulk delete messages (with user/keyword/bots filters)
- `/slowmode` — Set channel slowmode
- `/lock` / `/unlock` — Lock/unlock a channel
- `/automod` — Configure automod settings (status/addword/removeword/toggle/spam)
- `/userinfo` — Get info about a user
- `/serverinfo` — Get info about the server

### AutoMod Features

- **Banned word filter**: Deletes messages containing banned words, logs the event
- **Spam detection**: Tracks messages per user/window, auto-timeouts spammers

### Audit Logging

All server events are logged to a configured channel:
- Messages deleted / edited
- Voice channel joins, leaves, moves
- Member join / leave
- Member updates (nickname, roles)
- Channel created / deleted
- Role created / deleted

### Ticket System (inherited from original bot)

- `!ticket-embed` — Send the ticket creation embed (owner only)
- Supports 6 ticket types with modal description input
- Transcript saving to configured channels + DM to ticket creator

See `artifacts/discord-bot/.env.example` for all configuration options.

# listmonk selfhost notes

Newsletter + mailing lists. App + postgres, install/upgrade runs
automatically on start (see the `command:` line). ~150mb ram. Manual runs.

## up

```bash
cp selfhost/.env.example selfhost/.env
docker compose -f selfhost/docker-compose.yml --env-file selfhost/.env up -d
```

open http://localhost:8095, login with admin creds from .env.

## smtp

sending needs an SMTP relay — set it in admin → settings → SMTP.
Any relay works (your mailbox provider, amazon ses, etc).
Without SMTP it's still useful: subscriber + template management.

## backup

```bash
docker exec listmonk-db pg_dump -U listmonk listmonk > backups/listmonk-$(date +%F).sql
docker run --rm -v listmonk-uploads:/u -v $(pwd)/backups:/b alpine \
  tar czf /b/listmonk-uploads-$(date +%F).tar.gz /u
```

## update

pull + up -d — `--upgrade` in the start command runs migrations itself.
Check `docker logs listmonk` after.

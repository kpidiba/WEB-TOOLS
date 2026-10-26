

```md
Container medibible-app-1 Starting                                                                                           0.2s
Error response from daemon: ports are not available: 
exposing port TCP 0.0.0.0:8080 -> 127.0.0.1:0: 
listen tcp 0.0.0.0:8080: bind: address already in use
```

Something on your machine is already using port 8080, so Docker can’t bind to it. You have two options.

### Option 1: Use a different host port (easiest)

In `docker-compose.yml`, change the left side of the port mapping. Only the left number is the host port; the right side (`80`) is the port inside the container and should stay as is:

```yaml
    ports:
      - "8081:80"
```

Then update `APP_URL` in `.env.docker` to match:

```env
APP_URL=http://localhost:8081
```

Restart:

```bash
docker compose down
docker compose up --build -d
```

Then open [http://localhost:8081](http://localhost:8081).

### Option 2: Find and stop what’s using 8080

Check what is holding the port:

```bash

```

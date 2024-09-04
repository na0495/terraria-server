# Terraria Server with Docker

This project provides a setup to run a dedicated Terraria server using Docker. It uses a `docker-compose.yml` file and a `config.json` configuration file to define the server's environment and gameplay settings.

## Prerequisites

- Docker installed on your machine
- Docker Compose installed on your machine

## Project Structure

- **config.json**: This file contains the configuration for your Terraria server, such as world settings, difficulty, number of players, and other parameters.
- **docker-compose.yml**: This file defines how Docker will set up the Terraria server container, including ports, environment variables, and auto-restart behavior.

## Usage

### 1. Clone the Repository
First, clone the repository containing the `config.json` and `docker-compose.yml` files.

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Update `config.json`
Before running the server, make sure to review and update the `config.json` file. This file includes key settings like the server password, world name, difficulty level, and more.

Example configuration:
```json
{
  "world": "/root/.local/share/Terraria/Worlds/world.wld",
  "autocreate": 3,
  "seed": "",
  "difficulty": 3,
  "maxplayers": 10,
  "port": 17777,
  "password": "potato123",
  "motd": "Welcome techerbou atay!",
  "worldname": "Potato",
  "banlist": "banlist.txt",
  "secure": true,
  "npcstream": 60,
  "priority": 1,
  "upnp": true
}
```

### 3. Update `docker-compose.yml`
The `docker-compose.yml` file is set to use the latest Terraria Docker image. The default configuration includes the following setup:

```yaml
version: "3.8"

services:
  terraria:
    container_name: terraria
    image: ryshe/terraria:latest
    stdin_open: true
    tty: true
    environment:
      - WORLD_FILENAME=world.wld
      - CONFIGPATH=config.json
      - autocreate=3
    ports:
      - 17777:17777
    restart: unless-stopped
```

You can adjust the ports or other Docker settings if needed.

### 4. Start the Server

Once the files are configured, start the Terraria server using Docker Compose:

```bash
docker-compose up -d
```

This will run the server in detached mode. You can monitor the logs using the following command:

```bash
docker-compose logs -f
```

### 5. Access the Server

The Terraria server will be accessible on `localhost:17777` (or replace `localhost` with your server's IP address if running remotely). Players can join the server using the provided server address and password (`potato123` in this case).

### 6. Stopping the Server

To stop the server, run:

```bash
docker-compose down
```

This will stop and remove the container, but the world data and configuration will remain intact.

## Customization

- **World Settings**: The `WORLD_FILENAME` and other world-related settings can be adjusted in the `config.json` file.
- **Max Players**: Change the `maxplayers` value in `config.json` to increase or decrease the number of players allowed on the server.
- **Difficulty**: Adjust the `difficulty` field in the configuration file (1: Normal, 2: Expert, 3: Master).

## Persistent Data

By default, the world and configuration files are saved inside the container. If you'd like to persist data across container restarts, consider mounting a volume to store the world data on the host machine.

### Example Volume Mount:
In the `docker-compose.yml`, add a volume for world data:
```yaml
    volumes:
      - ./data:/root/.local/share/Terraria/Worlds
```

This will ensure your world data is saved locally in a `data` folder, even after the container is stopped.

## Troubleshooting

- Ensure that ports 17777 are open and not being blocked by your firewall.
- Verify the Docker image (`ryshe/terraria:latest`) is properly pulled and up-to-date.
  
## License

This project is open-source and available under the MIT License.

Feel free to modify and extend the project to fit your needs.

Enjoy playing Terraria with your friends!

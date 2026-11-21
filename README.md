# Minecraft Docker Server

A Linux-focused project for building, preparing, and running a **Minecraft Spigot server** with Docker Compose.

Common Docker operations are listed in [`cheatfile.json`](./cheatfile.json). You can **run these commands manually** in your Linux terminal or use **[CheatShell](https://github.com/GiDRaPiTaRdA/Linux-Scripts/tree/master/cheatshell)** to browse and execute them interactively. CheatShell is optional.

## Features

- Build a Minecraft server JAR using a Docker Compose builder.
- Prepare a version-specific server directory, including its JAR and EULA file.
- Launch the server with Docker Compose.
- Keep frequently used commands in a readable JSON cheat file.
- Run the commands manually or browse and execute them with CheatShell.

## Requirements

- Linux
- Docker Engine with the Docker Compose plugin (`docker compose`)
- Git (to clone the repository)
- Access to Docker from your Linux account

The example commands in this repository specify `JAVA_VERSION=25` and `MC_VERSION=26.3`. Adjust these values and paths to match the Minecraft version you intend to build and the configuration of the Compose files.

## Project structure

```text
Minecraft-Docker-Server/
├── build/
│   ├── Dockerfile
│   └── compose.yml
├── servers/
│   ├── compose.yml
│   └── .env             # Local configuration (shown in development setup)
└── cheatfile.json      # Named commands for server operations
```

`build/` contains the builder configuration, while `servers/` contains the Compose configuration used to run the server. Generated version directories and JAR files are created during the build/deploy workflow and are not necessarily committed to Git.

## Getting started

Clone this repository and enter its directory:

```bash
git clone https://github.com/GiDRaPiTaRdA/Minecraft-Docker-Server.git
cd Minecraft-Docker-Server
```

Review `build/compose.yml`, `servers/compose.yml`, and any required environment settings before starting.

## Choose how to run the commands

You have two options. **CheatShell is not required** to build, deploy, or run the Minecraft server.

| Method | How it works |
| --- | --- |
| **Option 1: Manual** | Open [`cheatfile.json`](./cheatfile.json), copy the `Command` value for the operation you want, and run it in a Linux terminal from the repository root. The commands are also shown below. |
| **Option 2: CheatShell** | Install [CheatShell using its README](https://github.com/GiDRaPiTaRdA/Linux-Scripts/blob/master/cheatshell/README.md), then run `cheatshell` from the repository root to select and execute commands in an interactive menu. |

### Option 1 — Run commands manually

These commands correspond to operations in `cheatfile.json`. Run them from the repository root in Bash or a compatible Linux shell.

#### 1. Build the server

```bash
JAVA_VERSION=25 MC_VERSION=26.3 docker compose -f build/compose.yml run --build --rm builder
```

This runs the Compose builder for the selected Java and Minecraft versions. The deployment command below expects the output JAR at `build/versions/26.3/spigot-26.3.jar`.

#### 2. Deploy the built JAR

```bash
MC_VERSION=26.3
mkdir -p "servers/$MC_VERSION"
echo 'eula=true' > "servers/$MC_VERSION/eula.txt"
cp "build/versions/$MC_VERSION/spigot-$MC_VERSION.jar" "servers/$MC_VERSION/spigot.jar"
```

**Important:** `eula=true` indicates acceptance of the Minecraft EULA. Read and agree to the [Minecraft EULA](https://www.minecraft.net/en-us/eula) before running this command.

#### 3. Launch the server

```bash
JAVA_VERSION=25 MC_VERSION=26.3 docker compose -f servers/compose.yml run --rm --service-ports spigot
```

The `--service-ports` option publishes the ports configured for the `spigot` service in the Compose file. Check that file for the actual port mapping.

### Option 2 — Use CheatShell (optional)

[CheatShell](https://github.com/GiDRaPiTaRdA/Linux-Scripts/tree/master/cheatshell) reads the project's `cheatfile.json` and shows its named commands in an interactive terminal menu. This is a convenience feature; the commands do not depend on CheatShell.

**Installation:** Follow the instructions in the **[CheatShell README — installation guide](https://github.com/GiDRaPiTaRdA/Linux-Scripts/blob/master/cheatshell/README.md)**. This project intentionally does not duplicate those instructions.

Once CheatShell is installed, launch it **from the Minecraft-Docker-Server repository root**, where `cheatfile.json` is located:

```bash
cd Minecraft-Docker-Server
cheatshell
```

Select **Build Version 26.3**, **Deploy Server 26.3**, **Launch Server 26.3**, or another available entry. Review or edit the command when prompted before executing it. Changes in the prompt do not modify `cheatfile.json`.

If you prefer not to install CheatShell, use **Option 1** above and copy the commands directly from [`cheatfile.json`](./cheatfile.json).

## Configuration notes

- The commands use paths relative to the repository root. Run them from that directory.
- `MC_VERSION` and `JAVA_VERSION` are supplied as environment variables to the Docker Compose commands.
- The included ownership example (`sudo chown -R chriss:chriss ~/minecraft/servers`) contains a machine-specific username and path. **Change both** before using it on another system.
- Review local `.env` files for settings required by your Compose services. Do not commit passwords, tokens, or other secrets.
- Back up server worlds before changing versions or replacing server files.

## Related project

- [CheatShell](https://github.com/GiDRaPiTaRdA/Linux-Scripts/tree/master/cheatshell) — the Linux terminal launcher that reads this project's `cheatfile.json`.
- [Linux-Scripts](https://github.com/GiDRaPiTaRdA/Linux-Scripts) — the parent repository containing CheatShell and other Linux utilities.

## Disclaimer

This is a personal Linux/Docker server setup. Check the commands, Docker configuration, Minecraft version compatibility, and EULA terms before using it on your machine.

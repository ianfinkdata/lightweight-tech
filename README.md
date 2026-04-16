# Lightweight Tech: Open-Source Data Analyst Environment
an experiment in using an old laptop as a fully functional data analysis tech stack.

This repository documents the configuration of a zero-cost, open-source data analytics environment on repurposed aging hardware. The goal is a completely hardware agnostic ecosystem that produces a low financial and environmental cost.  

## Hardware Profile
* **Machine:** HP ProBook 450 G3
* **OS:** Ubuntu 24.04.3 LTS
* **Processor:** Intel Core i5-6200U
* **Current Specs:** 8GB RAM, 500GB Storage

### Hardware Upgrades (To Prevent Bottlenecks)
* Replace existing storage with a SATA Solid State Drive (SSD).
* Upgrade memory to the motherboard maximum of 16GB RAM.
* Install a replacement battery for portability.

## The Software Stack
* **Version Control:** Git & GitHub
* **Code Editor:** Visual Studio Code (via Snap)
* **Data Processing:** Python 3 & pip (Pandas replaces Power Query)
* **Database Engine:** MySQL Server (Running locally)
* **Database Client:** Beekeeper Studio (Modern, lightweight GUI) or VS Code SQLTools Extension
* **Containerization:** Docker Desktop
* **Business Intelligence:** Metabase (Self-hosted via Docker, replaces Power BI)

## Setup Execution Plan

### Phase 1: Core Installs
Open the Ubuntu terminal (`Ctrl` + `Alt` + `T`) and execute:

```bash
# Update OS
sudo apt update && sudo apt upgrade -y

# Batch install core terminal tools and database engine
sudo apt install git python3-pip python3-venv mysql-server -y

# Ensure database starts on boot
sudo systemctl enable mysql

# Install visual tools via Snap
sudo snap install code --classic
sudo snap install beekeeper-studio
```

### Phase 2: Local MySQL Configuration

**Objective:** Ubuntu's default MySQL installation restricts the `root` user to terminal access only via socket authentication. To connect visual database clients (such as Beekeeper Studio or VS Code) to your local engine, you must create a standard database user with network access privileges.

## Execution Steps

1. **Access the Database Engine**
   Open your Ubuntu terminal (`Ctrl` + `Alt` + `T`) and log in as the root user. You will be prompted for your standard Ubuntu laptop password:
   ```bash
   sudo mysql

Create User and Grant Permissions
    Once the prompt changes to mysql>, execute the following SQL commands exactly as written. Replace 'YourStrongPassword' with a secure password of your choice:
    SQL

    CREATE USER 'ian'@'localhost' IDENTIFIED BY 'YourStrongPassword';
    GRANT ALL PRIVILEGES ON *.* TO 'ian'@'localhost';
    FLUSH PRIVILEGES;
    exit

Expected Outcome

The terminal will return you to your standard command prompt. You can now configure your visual database tools to connect to your local environment using the following credentials:

    Host: localhost

    Port: 3306

    Username: ian

    Password: (The password you set in step 2)

### Phase 3: Business Intelligence (BI) Tool Setup

**Objective:** To replace the Microsoft-dependent Power BI Desktop, you will deploy Metabase. Metabase is a lightweight, open-source analytics platform. To keep your base Ubuntu operating system fast and uncluttered, you will run the reporting tool inside an isolated Docker container.

## Execution Steps

1. **Install the Container Engine**
   Download **Docker Desktop for Linux** by getting the official `.deb` package from the Docker website. You can install it by double-clicking the file to open it in the Ubuntu App Center.

2. **Deploy the Metabase Server**
   Once Docker Desktop is installed and running in the background, open your terminal (`Ctrl` + `Alt` + `T`) and execute this single command. It will automatically download the Metabase image and spin up the server:
   ```bash
   docker run -d -p 3000:3000 --name metabase metabase/metabase
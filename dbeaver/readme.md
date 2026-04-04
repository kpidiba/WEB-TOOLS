# DBeaver Universal Database Manager

**DBeaver** is a free, multi-platform database tool for developers, SQL programmers, analysts, and database administrators. It supports all popular databases with an intuitive interface and powerful features.

## Use Cases

- **Database Development** – Write, execute, and debug SQL scripts.

- **Data Analysis** – Browse, edit, export, and visualize data.

- **Schema Management** – Create, modify, and compare database schemas.

- **Migration** – Move data between different database systems.

- **Troubleshooting** – Monitor queries, view execution plans, and manage connections.

- **Team Collaboration** – Share connections, scripts, and projects.

## Supported Databases (in this guide)

| Database       | Type       | Use Case                                   |
| -------------- | ---------- | ------------------------------------------ |
| **MariaDB**    | Relational | Web apps, open-source alternative to MySQL |
| **PostgreSQL** | Relational | Enterprise apps, geospatial, analytics     |
| **SQL Server** | Relational | .NET apps, corporate systems               |
| **MongoDB**    | NoSQL      | Document stores, big data, flexible schema |
| **SQLite**     | Embedded   | Mobile apps, desktop software, prototyping |

## Installation

### Linux (Ubuntu/Debian)

**Method 1: Install from repository**

```bash
# Add DBeaver repository
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list

# Update and install
sudo apt update
sudo apt install dbeaver-ce
```

**Method 2: Download and run (any distro)**

```bash
# Download the Linux .tar.gz
wget https://dbeaver.io/files/dbeaver-ce-latest-linux.gtk.x86_64.tar.gz

# Extract
tar -xzf dbeaver-ce-*.tar.gz

# Run
cd dbeaver && ./dbeaver
```

### Windows

1. **Download** – Go to [dbeaver.io/download](https://dbeaver.io/download) and get the Windows installer (`.exe`).

2. **Install** – Run the installer. Choose:
   
   - Installation path (default: `C:\Program Files\DBeaver`)
   
   - Components (recommended: keep all)

3. **Launch** – From Start Menu or Desktop shortcut.

> 💡 **Portable version** – Download the ZIP archive, extract anywhere, run `dbeaver.exe`.

### Verify Installation

After launching DBeaver, you should see the **Welcome** screen and the **Database Navigator** panel.

## Managing Databases

### 1. Add a Database Connection

1. Click the **New Database Connection** button (plug icon) in the toolbar.

2. Select your database type from the list.

3. Fill in connection details (see examples below).

4. Click **Test Connection** → download missing drivers if prompted.

5. Click **Finish**.

### 2. Connection Examples

#### MariaDB / MySQL

## 📦 Installation de MariaDB

Suivez les étapes ci-dessous pour installer MariaDB sur Linux :

1. 🔎 **Rechercher MariaDB**
   
   - Ouvrez votre navigateur et tapez : **"MariaDB for Linux"** sur Google.

2. 📘 **Accéder à la documentation officielle**
   
   - Cliquez sur le guide officiel :  
     👉 MariaDB Documentation
   - Ou utilisez directement ce lien :  
     [Installing MariaDB Server Guide | Server | MariaDB Documentation](https://mariadb.com/docs/server/mariadb-quickstart-guides/installing-mariadb-server-guide)

3. ⚙️ **Suivre le guide d’installation**
   
   - Choisissez votre distribution Linux (Ubuntu, Debian, CentOS, etc.)
   - Exécutez les commandes fournies dans la documentation

4. 🚀 **Après l’installation**
   
   - Démarrez le service MariaDB :
     
     ```bash
     sudo systemctl start mariadb
     ```
   
   - Vérifiez qu’il fonctionne :a
     
     ```bash
     sudo systemctl status mariadb
     ```
   
   - 🔐 **Accéder à MariaDB**
     
     ```bash
     sudo mariadb
     ```
- ```bash
  sudo mariadb
  ```

- ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED VIA mysql_native_password USING PASSWORD('root');
  ```

```bash
Host: localhost
Port: 3306
Database: myapp
Username: root
Password: ****
```

> *Driver: MariaDB / MySQL*

#### PostgreSQL

```textile
Host: localhost
Port: 5432
Database: postgres
Username: postgres
Password: ****
```

#### SQL Server

```json
Host: localhost
Port: 1433
Database: master
Authentication: Windows Single Sign-On or SQL Server Auth
Username: sa
Password: ****
```

> *Driver: Microsoft SQL Server / JTDS*

#### MongoDB

```json
Host: localhost
Port: 27017
Database: test
Authentication: None or SCRAM-SHA-1
Username: (optional)
```

> *Driver: MongoDB*

#### SQLite

```json
Path: /home/user/data/mydb.sqlite  (Linux)
Path: C:\data\mydb.sqlite         (Windows)
```

> *Driver: SQLite* – No host/port needed.

### 3. Manage Data & Schema

Once connected, you can:

| Action                | How to do it                                                                           |
| --------------------- | -------------------------------------------------------------------------------------- |
| **Browse tables**     | Expand database → Schemas → Tables                                                     |
| **View data**         | Right-click table → **View Data**                                                      |
| **Edit cells**        | Double-click a cell → edit → press Enter                                               |
| **Run SQL**           | Open **SQL Editor** (Ctrl+`) → Write query → Execute (Ctrl+Enter) \|                   |
| **Export results**    | Right-click result grid → **Export Data** → CSV, JSON, XML, Excel                      |
| **Modify schema**     | Right-click table → **Alter Table** → Add/remove columns, indexes, constraints         |
| **Compare databases** | Tools → **Schema Compare**                                                             |
| **Backup**            | Right-click database → **Tools** → **Dump Database** (for PostgreSQL, MariaDB, SQLite) |

### 4. Working with Multiple Databases

DBeaver allows you to connect to different database types **simultaneously**.

**Example: Join data across databases (limited)**

- You cannot directly join tables from different DBMS, but you can:
  
  1. Export data from one database to CSV.
  
  2. Import CSV into another database as a temporary table.
  
  3. Run cross-database queries within the **same** DBMS (e.g., PostgreSQL foreign data wrappers).

**Better approach** – Use DBeaver's **Data Transfer** feature:

- Right-click any table/result → **Export Data** → Target: another database connection.

## Pro Tips

- **Dark Theme** – Window → Preferences → Appearance → Theme → Dark

- **ER Diagram** – Right-click database/schema → **View Diagram**

- **Query History** – Ctrl+Shift+H

- **Saved Queries** – Drag `.sql` files into **Projects** view

- **SSH Tunneling** – In connection settings → **SSH** tab → Enable SSH tunnel

- **Connection Groups** – Right-click in Database Navigator → Create Folder → drag connections inside

## Troubleshooting

| Problem                         | Solution                                                                |
| ------------------------------- | ----------------------------------------------------------------------- |
| Driver not found                | Click **Download** in the driver edit window                            |
| Connection refused              | Check host/port, firewall, and if database service is running           |
| MongoDB shows no databases      | Enable "Show all databases" in connection settings → MongoDB → Advanced |
| SQLite database locked          | Close other applications using the SQLite file                          |
| Cannot see tables in PostgreSQL | Check schema selection in connection settings → PostgreSQL → Schemas    |

## Uninstall

**Linux**:

```bash
sudo apt remove dbeaver-ce   # Debian/Ubuntu
# Or delete the extracted folder if using tarball
```

**Windows**:

- Settings → Apps → DBeaver → Uninstall

## Resources

- [Official Documentation](https://dbeaver.com/docs/)

- [Community Forum](https://github.com/dbeaver/dbeaver/discussions)

- [Download Page](https://dbeaver.io/download)

---

**License**: DBeaver Community Edition is free and open-source (Apache 2.0).  
**Enterprise Edition** (paid) adds NoSQL support (MongoDB is fully supported in CE), ERD enhancements, and team collaboration.

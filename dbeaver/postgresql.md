# PostgreSQL & pgAdmin Installation Guide (Ubuntu)

This guide walks you through installing PostgreSQL and pgAdmin on an Ubuntu-based system using the official APT repositories.

---

## 1. Visit PostgreSQL Website

Go to the official PostgreSQL website:

- [https://www.postgresql.org/](https://www.postgresql.org/)

Navigate to the **Download** section and select your **Ubuntu distribution**.

---

## 2. Add PostgreSQL APT Repository

Follow the instructions provided on the PostgreSQL site to add the official repository to your system.

---

## 3. Visit pgAdmin Website

Go to:

- [https://www.pgadmin.org/](https://www.pgadmin.org/)

Navigate to the **Download** section and select:

- **APT (Debian/Ubuntu)**

You will be redirected to:

- https://www.pgadmin.org/download/pgadmin-4-apt/

---

## 4. Follow pgAdmin Installation Instructions

Run the commands provided on the pgAdmin APT download page. These typically include:

- Adding the repository

- Importing the repository key

- Updating package lists

- Installing pgAdmin

---

## 5. Update PostgreSQL Repository Configuration

Edit the PostgreSQL repository file:

```bash
sudo vim /etc/apt/sources.list.d/pgdg.list
```

Replace or update its contents with:

```bash
deb [arch=amd64 signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt noble-pgdg main
```

---

## 6. Update Package List

After saving the file, update your package list:

```bash
sudo apt update
```

---

## Notes

- Ensure the `signed-by` path matches the key installed on your system.

- The `arch=amd64` option restricts packages to 64-bit architecture.

- Replace `noble` with your Ubuntu codename if needed (e.g., `jammy`, `focal`).

---

## Done 🎉

You should now have PostgreSQL and pgAdmin properly configured using the official APT repositories.# PostgreSQL & pgAdmin Installation Guide (Ubuntu)

This guide walks you through installing PostgreSQL and pgAdmin on an Ubuntu-based system using the official APT repositories.

---

## 1. Visit PostgreSQL Website

Go to the official PostgreSQL website:

- [https://www.postgresql.org/](https://www.postgresql.org/)

Navigate to the **Download** section and select your **Ubuntu distribution**.

---

## 2. Add PostgreSQL APT Repository

Follow the instructions provided on the PostgreSQL site to add the official repository to your system.

---

## 3. Visit pgAdmin Website

Go to:

- [https://www.pgadmin.org/](https://www.pgadmin.org/)

Navigate to the **Download** section and select:

- **APT (Debian/Ubuntu)**

You will be redirected to:

- https://www.pgadmin.org/download/pgadmin-4-apt/

---

## 4. Follow pgAdmin Installation Instructions

Run the commands provided on the pgAdmin APT download page. These typically include:

- Adding the repository

- Importing the repository key

- Updating package lists

- Installing pgAdmin

---

## 5. Update PostgreSQL Repository Configuration

Edit the PostgreSQL repository file:

```bash
sudo vim /etc/apt/sources.list.d/pgdg.list
```

Replace or update its contents with:

```bash
deb [arch=amd64 signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt noble-pgdg main
```

---

## 6. Update Package List

After saving the file, update your package list:

```bash
sudo apt update
```

---

## Notes

- Ensure the `signed-by` path matches the key installed on your system.

- The `arch=amd64` option restricts packages to 64-bit architecture.

- Replace `noble` with your Ubuntu codename if needed (e.g., `jammy`, `focal`).

---

## Done 🎉

You should now have PostgreSQL and pgAdmin properly configured using the official APT repositories.

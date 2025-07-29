---
title: Camunas Server Guide
---

# Utilizing the Jcamunas Server

## Server Information

* **IP**: `130.241.204.241`
* **Note**: Each user has a **personal login**. Do **not** use the shared `Jcamunas` account for work or file transfer.

---

## IMPORTANT: VPN Required

You must be connected to the **Cisco Secure VPN client** to access the server.

### VPN Setup

1. Install the VPN client by following instructions here: [https://min.it.gu.se/](https://min.it.gu.se/)
2. Connect via Cisco Secure Client before proceeding.

---

## 1. Access Server via Web Interface (Recommended)

You can access the server using your web browser:

* URL: [http://130.241.204.241:5000/](http://130.241.204.241:5000/)
* Username: `{your_name}`
* Password: `Sequenceit!9`

---

## 2. Connecting to Server via SSH

### a. Generate an SSH Key

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_camunas
```

* Accept the default file location or enter a custom filename.
* Set a passphrase or press Enter twice for none.

### b. Confirm Key Creation

```bash
ls -l ~/.ssh/id_ed25519_camunas*
```

Should output two files:

* `id_ed25519_camunas` (private key)
* `id_ed25519_camunas.pub` (public key)

### c. Add Your Key to the Server (Manual Method)

If `ssh-copy-id` fails due to too many keys, use this instead:

```bash
cat ~/.ssh/id_ed25519_camunas.pub | ssh -o IdentitiesOnly=yes -i ~/.ssh/id_ed25519_camunas {your_name}@130.241.204.241 \
'mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys'
```

### d. SSH Config Entry (Optional)

Edit or create `~/.ssh/config`:

```bash
Host camunas
    HostName 130.241.204.241
    User {your_name}
    IdentityFile ~/.ssh/id_ed25519_camunas
    IdentitiesOnly yes
```

Then simply connect using:

```bash
ssh camunas
```

---

## 3. File Storage Policy

Upon login, the system warns:

> "Data should only be stored in shared folders. Data stored elsewhere may be deleted when the system is updated/restarted."

### Shared Folder Path

```bash
/camunaslab_nas/individual/
```

Each user should create their personal directory:

```bash
mkdir -p /camunaslab_nas/individual/{your_name}
```

Use only your folder for storing files to avoid loss.

---

## 4. Transferring Files To the Server

Because inbound `scp`/`rsync` is restricted, **you must pull data from the server side**.

### a. Pull Files from Remote Server

```bash
rsync -avzP -e "ssh -i ~/.ssh/your_key" \
user@remote.server:/path/to/data/ \
/camunaslab_nas/individual/your_name/
```

### b. Make Transfer Persistent with `nohup`

```bash
nohup rsync -avzP -e "ssh -i ~/.ssh/your_key" \
user@remote.server:/path/to/data/ \
/camunaslab_nas/individual/your_name/ > rsync.log 2>&1 &
disown
```

### c. Monitor Transfer Progress

```bash
tail -f rsync.log
```

---

## 6. Admin Guide

* **Default Admin User**: `Jcamunas`
* **Default Password**: `Sequenceit!9`



Admins can eg create individual logins for each user.
See more info: [https://kb.synology.com/sv-se/DSM/help/DSM/AdminCenter/file_user_desc?version=7]


## Support

For login issues, access problems, or help creating user accounts, contact the system administrator.

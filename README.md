# Proctor Backup

**Proctor Backup** is a free, modern database backup software with a sleek web UI for managing and monitoring your backup processes.
While the source code is not publicly available, we provide releases for Windows and Linux platforms.

---

## Features

- **Scheduled Database Backups**  
  Automate your database backups with Proctor. Currently, only MySQL is supported. Additional database types are planned or can be added upon request.

- **Database Backup Synchronization in Local Network**  
  Sync backups across local network machines using the **WinRM protocol** for Windows. Support for SSH is coming soon, or let us know if you need it urgently.

- **Cloud Backup Synchronization**  
  Synchronize your backups to the cloud. Currently, only **Google Drive** is supported. Support for other cloud platforms is coming soon or can be prioritized based on user requests.

- **Backup Security**  
  Your backups are safe with us! All files sent over the LAN and stored in the cloud are **encrypted** by default.

- **Real-Time Monitoring Web UI**  
  Track and monitor running backup jobs and synchronization tasks in real time through Proctor’s modern web-based interface.

- **Simple Configuration**  
  Configure Proctor effortlessly using a **YAML configuration file**. Check out our [sample config file](sample-conf.yml).

- **Cross-Platform Support**  
  Proctor works on **Windows** and **Linux**. Need a macOS version? Reach out to us!

---

## Getting Started

1. **Download Proctor**  
   Grab the latest release for your platform from the [Releases](https://github.com/csso-iam/proctor-backup/releases/tag/Latest).

2. **Configure Your Backup Jobs**  
   Edit the provided YAML configuration file to match your setup. Refer to the [sample configuration file](#) for details.

3. **Run Proctor**  
   Start Proctor, schedule your backups, and monitor everything using the web UI.

---

## Configuration Details

Proctor uses a **YAML configuration file** to define your backup settings, synchronization preferences, and security options.  
For example, here’s a basic snippet:

```yaml
# Proctor database backup configuration
version: 1
description: Backup scheduler configuration

# Number of days to retain old local backup files
retention: 5

# Cron schedule for backup file rotation (old backups will be removed based on this schedule)
rotation-cron: '1 13 * * *' # Runs at 13:01 every day

# Path to your mysqldump executable file
mysqldump-dir: 'absolutepath/to/mysqldump.exe'

# Directory where local backup files will be stored
backup-dir: 'path/to/backups/folder'

# UI settings for monitoring backup jobs
# Port for accessing the backup UI (use a free port)
# ui-port: 443    # Uncomment this for HTTPS
# ui-port: 80     # Uncomment this for HTTP
ui-port: 3000      # Set to a free port for web access

# HTTPS configuration for secure access to the UI (if desired)
# Uncomment and provide certificate and key files for HTTPS
# https-config:
#   certificate: 'path/to/certificate.crt'
#   key: 'path/to/private-key.key'

# Web UI credentials for authentication
ui-user:
  name: admin@admin.com
  password: proctor.2.0.2.5  # Shared, non-sensitive password (UI doesn't handle secret info)

# Database connection details
datasource:
  # Hostname of the MySQL server
  hostname: 127.0.0.1

  # Port of the MySQL server
  port: 3306

  # Uncomment and set if using a custom database URL
  # url: 'root@tcp(127.0.0.1:3306)/books?parseTime=true'

  # MySQL username for backup user
  username: your_mysql_backup_user_name

  # MySQL password (use an environment variable for better security)
  password: ${YOUR_MYSQL_BACKUP_USER_PASSWORD}  # Set as an environment variable to avoid exposing credentials

# Email settings for backup notifications or syncing backups via email
email:
  hostname: smtp.gmail.com  # SMTP server hostname (e.g., Gmail or Yahoo)
  port: 587               # SMTP server port
  username: <your smtp email@email>
  to: <mailTo@>            # Email address to send notifications to
  password: ${YOUR_SMTP_PASSWORD}  # Set SMTP password as an environment variable for security

# Backup job settings (you can define multiple jobs)
jobs:
  - name: everyTenMinutes    # Unique job name
    cron: '*/10 * * * *'     # Cron expression for backup frequency
    dbname: firstdatabase    # Name of the database to backup

  # Example of another job (commented out for now)
  # - name: hourly_backup
  #   cron: '*/30 * * * * *'
  #   dbname: other_database

# Cloud syncing job settings (optional)
# Sync backups to Google Drive or via email
cloud:
  - method: email           # Method for cloud syncing (email or google_drive)
    auto: true               # Set to true for automatic syncing
    cron: '*/32 * * * *'     # Sync cron schedule
  - method: google_drive     # Another method: Google Drive
    auto: true
    cron: '*/32 * * * *'     # Sync cron schedule for Google Drive
    credentials-file: '<absolutepath\to\your\google-drive-service-account-credentials-file.json>'  # Path to Google Drive credentials
    folder-id: '<your google drive folder id>'   # Google Drive folder ID for backups

# LAN syncing settings (optional, for syncing backups to a local network computer)
lan:
  - computer-name: '<WINDOWS-COMPUTER-NAME>'  # Name of the Windows computer to sync to
    port: 5985                                  # The WinRM port (only 5985 supported)
    credential: '<WINDOWS-COMPUTER-NAME>\<target-user>'  # Username for remote connection
    secret: ${REMOTE_USER_PASSWORD}  # Remote user password (use environment variable for security)
    destination: 'path\to\remote-machine\backup_folder'  # Remote folder path for backup
    cron: '*/15 * * * *'     # Cron schedule for LAN syncing
    retention: 1             # Retention period for remote backups (in days)
    rotation-cron: '*/30 * * * *'  # Cron schedule for remote backup file rotation (old files deleted automatically)

```

---


## Support
If you encounter any issues, have feature requests, or need a specific platform or protocol supported, feel free to reach out!

LinkedIn: [Tsognong Fidele](https://www.linkedin.com/in/tsognong-fidele)

## Built With
- Go (Golang) - The foundation of Proctor’s backend ensures performance and reliability.

---

## Contributing
You can support the project by:

- Suggesting features or improvements.
- Reporting bugs.
- Sharing Proctor with others who may find it useful.

---

## License
Proctor is distributed as-is for free use. Contact the author for inquiries about custom features or modifications.

## Download
Head to the Releases section to download the latest version of Proctor for your platform.

![backup](https://github.com/user-attachments/assets/cca797e1-350c-4686-a282-04e1e9a344ef)

![capture](https://github.com/user-attachments/assets/d4899b31-f2b1-47f5-bea1-0d7c2587323a)

![logger](https://github.com/user-attachments/assets/c0ec5090-9d03-43c6-8de8-ffd3f9a475c3)

![mertcis](https://github.com/user-attachments/assets/9bf0a94b-3f64-460f-965e-c99abb5515a4)
















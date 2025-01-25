# proctor-backup

**Proctor** is a free, modern database backup software with a sleek web UI for managing and monitoring your backup processes.
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
   Grab the latest release for your platform from the [Releases](https://github.com/tsognong/proctor-backup/releases/latest).

2. **Configure Your Backup Jobs**  
   Edit the provided YAML configuration file to match your setup. Refer to the [sample configuration file](#) for details.

3. **Run Proctor**  
   Start Proctor, schedule your backups, and monitor everything using the web UI.

---

## Configuration Details

Proctor uses a **YAML configuration file** to define your backup settings, synchronization preferences, and security options.  
For example, here’s a basic snippet:

```yaml
# Proctor database backup 
# configuration
version: 1
description: Backup scheduler configuration
# Number of days of local backup files retention
retention: 0
#The rotation cron, that means at whihc frequency the old backup will be removed
rotation-cron: '1 13 * * *' # 13H01 minutes every day
#The path to your mysqldump.exe file
mysqldump-dir: 'absolutepath/to/mysqldump.exe'
#The path to the folder where your local backups files will be kept
backup-dir: 'path/to/backups/folder'
#The http ui port, where you will monitor your jobs
#ui-port: 443    #in case of https
#ui-port: 80    #in case of http
ui-port: 3000    #Use any free port you like
#https-config:   #activate this section if you want to use use access the web interface using https intead of http by default
#  certificate: 'path/to/certicate.crt file'
#  key: 'path/to/private key.key file'
#The credentials that you will use to authenticate to the web interface
ui-user:
  name: admin@admin.com
  password: proctor.2.0.2.5     #set a password to access the web interface, no config and no secret information is performed via the ui, so this is a sharable credential
datasource:
  # Hostname of the mysql server to connect to, as you like
  hostname: 127.0.0.1
  
  # Port of the mysql server to connect to
  port: 3306
  
  # The database url, activate this in case you have a custom url
  # url: 'root@tcp(127.0.0.1:3306)/books?parseTime=true'
    
  # username of the mysql server to connect to
  username: root
  # password of the mysql server to connect to
  password: 'root'
  
#The email settings that will be used for notifications (or email backup syncing, see the cloud config section)
#Remove the emqil settings if not needed
email:
  hostname: smtp.gmail.com  #smtp.mail.yahoo.com
  port: 587
  username: <your smtp email@email>
  to: <mailTo@>
  password: <your smtp password>
  
#Backups jobs settings, you can add as jobs as you need, actually there is only one database per job, option for all databases at once will come later
jobs:
  - name: everyTheninutes    #The unique job name
    cron: '*/10 * * * *'     #The job cron expression
    # The database name
    dbname: firstdatabase
    # The backup files will be placed to <backup-dir>/<dbname> in the format {DATABASE_NAME}_{TIMESTAMP}.sql.zip or gz
# - name: hourly_backup
    #The cron expression of the backup job
#   cron: '*/30 * * * * *'
#   dbname: other database

#Cloud syncing jobs settings, actually you can sync your databases dumps in google drive or by email
#Add as jobs as you need, the cloud syncing is not mandatory, remove this section if you need
cloud:
  - method: email #The backups syncing method (actually only email and google_drive are possible)
    auto: true    #The auto mode is used if you mean the syncing job to be automatic (true) or manual (false), for manuall syncing you will use the web interface to sync manually
    cron: '*/32 * * * *'    #The syncing cron job expression, in case of auto syncing
  - method: google_drive
    auto: true
    cron: '*/32 * * * *' #The syncing cron job expression, in case of auto syncing
    credentials-file: '<absolutepath\to\your\google-drive-service-account-credentials-file.json>'   #If you choose to sync by google drive, please create a google drive service account in google console, download the credentials and provide the path here
    folder-id: '<your google drive folder id>'   #The google drive folder Id where you want the backups files to be uploaded
	
#Lan syncig settings, this section is usefull in case you wan to save your backups files in the other computer in your local network
#This section is also optional
#Setup as LAN syncs jobs as you like
#Actually only WINRM settings are accepted
lan:
  - computer-name: '<WINDOWS-COMPUTER-NAME>'  #The computer name to whihc you want to sync
    port: 5985     #The winrm port number, actually only 5985 is suppported
    credential: '<WINDOWS-COMPUTER-NAME>\<target-user>'   #Your user name to connect to the target host, please preferably setup a particular backup user in the target host; assgning him, add the user to the Remote group
    secret: '<password>'   #The remote user password
    destination: 'path\to\remote-machine\backup_folder'  #The folder of the remote machine where the backup will be saved
    cron: '*/15 * * * *'             #The syncing cron job expression
    retention: 1        #The remote backups retention in days
    rotation-cron: '*/30 * * * *'     #The remote backups rotation cron expression, you will not need to delete old bqckup backups files in remote machine manually

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












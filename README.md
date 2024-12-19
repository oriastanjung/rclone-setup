
# Rclone Installation and Setup for Syncing Google Drive with Local System

This guide provides a step-by-step process to install Rclone, configure it for Google Drive, perform synchronization from Drive to local, copy from local to Drive, and set up automation using cron.

## 1. Install Rclone

### **On Linux**
1. Download the Rclone binary:
   ```bash
   curl -O https://downloads.rclone.org/rclone-current-linux-amd64.zip
   ```
2. Extract the downloaded file:
   ```bash
   unzip rclone-current-linux-amd64.zip
   cd rclone-*-linux-amd64
   ```
3. Move the binary to `/usr/bin/`:
   ```bash
   sudo cp rclone /usr/bin/
   sudo chmod +x /usr/bin/rclone
   ```
4. Verify installation:
   ```bash
   rclone version
   ```

### **On macOS**
1. Install using Homebrew:
   ```bash
   brew install rclone
   ```

2. Verify installation:
   ```bash
   rclone version
   ```

### **On Windows**
1. Download the binary from [Rclone Downloads](https://rclone.org/downloads/).
2. Extract the ZIP file and add the folder to your system PATH.

---

## 2. Configure Rclone with Google Drive

1. Start Rclone configuration:
   ```bash
   rclone config
   ```

2. Create a new remote:
   - Choose `n` (New remote).
   - Enter a name for the remote (e.g., `mygdrive`).
   - Choose `13` for Google Drive.

3. Authenticate:
   - Follow the URL provided to authenticate your Google account.
   - Copy the access token back into the terminal.

4. Test the configuration:
   ```bash
   rclone ls mygdrive:
   ```

---

## 3. Sync Files from Google Drive to Local

To synchronize files from Google Drive to a local folder:

1. Create a local folder:
   ```bash
   mkdir ~/local-drive
   ```

2. Sync files from Google Drive to the local folder:
   ```bash
   rclone sync mygdrive:/ ~/local-drive --progress
   ```

   - This command ensures that the local folder matches the content of Google Drive.

3. Optional: Test the sync operation without making changes:
   ```bash
   rclone sync mygdrive:/ ~/local-drive --progress --dry-run
   ```

---

## 4. Copy Files from Local to Google Drive

To copy files from a local folder to Google Drive:

1. Copy files:
   ```bash
   rclone copy ~/local-drive mygdrive:/backup-folder --progress
   ```

   - This command uploads files from the local folder to a specific folder in Google Drive (`backup-folder`).

2. Optional: Test the copy operation:
   ```bash
   rclone copy ~/local-drive mygdrive:/backup-folder --progress --dry-run
   ```

---

## 5. Automate Sync and Copy with Cron

### **Sync from Drive to Local**
1. Open the crontab editor:
   ```bash
   crontab -e
   ```

2. Add the following line to sync every day at 2 AM:
   ```bash
   0 2 * * * rclone sync mygdrive:/ ~/local-drive --progress
   ```

### **Copy from Local to Drive**
1. Add the following line to copy every day at 3 AM:
   ```bash
   0 3 * * * rclone copy ~/local-drive mygdrive:/backup-folder --progress
   ```

3. Save and exit.

---

## 6. Verify Cron Jobs

1. List active cron jobs:
   ```bash
   crontab -l
   ```

2. Check cron logs for errors:
   ```bash
   cat /var/log/syslog | grep CRON
   ```

---

With these steps, you have successfully set up Rclone for Google Drive synchronization and automation with cron! 🎉

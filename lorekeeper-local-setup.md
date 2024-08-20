# Lorekeeper Local Setup

## Pre-Requisites

### Install PHP

1. Go to [https://windows.php.net/download](https://windows.php.net/download) and download the latest **VS16 x64 Non Thread Safe** ZIP file.

2. Extract the contents of the ZIP file. I usually create a folder in the system drive, like: `C:\.php` 

    - **Important:** You will need the folder filepath (i.e., `C:\.php`) when [setting your environment variables](#update-environment-variables).

### Install MariaDB / HeidiSQL

1. Go to [https://mariadb.org/download](https://mariadb.org/download) and download the MSI package for the latest release. You can use the default settings on the page when downloading.

2. Double click the MSI package to launch the MariaDB installer.

3. Complete the MariaDB installation using default settings. Record the **root user password** and the **data directory**, you will need them during later steps.

    - **Important:** You will need the password of your root user for [setting up your database](), and the data directory for [setting your environment variables](#update-environment-variables).

### Update Environment Variables 

1. Type `Win + R` to open the **Run** dialog, and enter `systempropertiesadvanced`. This will open the **System Properties** dialog.

2. Select **Environment Variables…** button to open the **Environment Variables** dialog.

3. Select the **Path** variable and the **Edit…** button to open the **Edit environment variable** dialog.

4. Select **New** to create a new line, and enter the path to your PHP folder (in my case this is `C:\.php`).

5. Select **New** again to create another new line, and enter your MariaDB bin directory. To get the bin directory path, change the `data\` on your data directory path to `bin`. It should look similar to the below example:

    - `C:\Program Files\MariaDB 11.4\data\` => `C:\Program Files\MariaDB 11.4\bin`

6. Hit **OK** and exit the dialogs.

7. Open the **Terminal** application. This can be found in Windows search, or by entering `cmd` into the **Run** dialog.

8. Verify the environment variables are configured correctly, but entering the following commands:

    - `php -v`
    - `mysql --version`
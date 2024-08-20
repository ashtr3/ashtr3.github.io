# Lorekeeper Local Setup

## Pre-Requisites

### Install VS Code

1. Go to [https://code.visualstudio.com/](https://code.visualstudio.com/) and select **Download for Windows**.

2. Double click the executable to launch the Visual Studio Code installer.

3. Complete the Visual Studio Code installation.

    I recommend checking all of the Other options in the **Select Additional Tasks** step.

    ![](/images/08202024-vs-code-options.png)

### Install PHP

1. Go to [https://windows.php.net/download](https://windows.php.net/download) and download the latest **VS16 x64 Non Thread Safe** ZIP file.

2. Extract the contents of the ZIP file. I usually create a folder in the system drive, like: `C:\.php` 

    > **Important:** You will need the folder filepath (i.e., `C:\.php`) when [setting your environment variables](#update-environment-variables).

### Install Composer

1. Go to [https://getcomposer.org/download/](https://getcomposer.org/download/) and download the **Composer-Setup.exe** in the Windows Installer section.

2. Double click the executable to launch the Composer installer.

3. Complete the Composer installation using default settings.

### Install MariaDB / HeidiSQL

1. Go to [https://mariadb.org/download](https://mariadb.org/download) and download the MSI package for the latest release. You can use the default settings on the page when downloading.

2. Double click the MSI package to launch the MariaDB installer.

3. Complete the MariaDB installation using default settings.

    > **Important:** You will need the password of your root user for [setting up your database](#create-database), and the data directory for [setting your environment variables](#update-environment-variables).

### Install Git

1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win) and download the latest 64-bit version for **Git for Windows**.

2. Double click the executable to launch the Git installer.

3. Complete the Git installation using default settings.

### Update Environment Variables 

1. Type `Win + R` to open the **Run** dialog, and enter `systempropertiesadvanced`. This will open the **System Properties** dialog.

2. Select **Environment Variables…** button to open the **Environment Variables** dialog.

3. Select the **Path** variable and the **Edit…** button to open the **Edit environment variable** dialog.

4. Select **New** to create a new line, and enter the path to your PHP folder (in my case this is `C:\.php`).

5. Select **New** again to create another new line, and enter your MariaDB bin directory. To get the bin directory path, change the `data\` on your data directory path to `bin`. It should look similar to the below example:

    - `C:\Program Files\MariaDB 11.4\data\` => `C:\Program Files\MariaDB 11.4\bin`

6. Hit **OK** and exit the dialogs.

7. Open the **Terminal** application. This can be found in Windows search, or by entering `cmd` into the **Run** dialog.

8. Verify the environment variables are configured correctly by entering the following commands. These should all return version numbers if they are successful.

    - `php -v`
    - `composer --version`
    - `mysql --version`
    - `git -v`

## Setup

### Create Database

1. Open **HeidiSQL**. This was installed with your MariaDB installation.

2. The **Session manager** will open. Enter your root user password (created during the [MariaDB installation](#install-mariadb--heidisql)) to the **Password** field. The other fields can keep their default values.

    - Optional, right-click on the Session name and rename it to something more descriptive. I usually set it as `Local`.

3. Select **Save** and then select **Open**.

4. **HeidiSQL** will open. On the left sidebar, right-click on the session name and select **Create New** > **Database** from the context menu.

    ![](/images/08202024-heidisql-create-db.png)

5. Enter your database **Name** and select **OK**.

    > **Important:** You will need the database name for your [Lorekeeper setup](#configure-and-setup-lorekeeper).

### Clone Lorekeeper

1. Create a new folder where you want the site files to be installed.

2. Launch **Visual Studio Code**, select **File** > **Open Folder...** and select your newly created folder.

3. Select **Terminal** > **New Terminal**.

4. Enter the following commands in the terminal:

    - `git init`

        This will initiate your local Git repository.

    - `git remote add lorekeeper https://github.com/corowne/lorekeeper.git`

        This will add the official Lorekeeper repository as a remote named `lorekeeper`.

    - `git fetch`

        This will fetch all of the remote branches.

    - `git pull lorekeeper release/v3.0.0`

        This will pull all of the commits on the v3 release branch and merge them to your local master branch.

### Configure and Setup Lorekeeper

1. Enter the following commands in the terminal:

    - `composer install`

        This will trigger Composer to install all of the vendor packages bundled with Lorekeeper.

    - `cp .env.example .env`

        This will make a copy of `.env.example` named `.env`.

    - `php artisan key:generate`

        This will generate an application key that is added to your `.env` file.

2. Open the `.env` file, and update the following:

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE={DB_NAME}
    DB_USERNAME=root
    DB_PASSWORD={ROOT_PASSWORD}
    ```
    `{DB_NAME}` and `{ROOT_PASSWORD}` should be replaced with the database name set during [database creation](#create-database) and the root password set during [MariaDB installation](#install-mariadb--heidisql).

3. Enter the following commands in the terminal:

    - `php artisan migrate`
    
        This will create the database tables required by Lorekeeper.

    - `php artisan add-site-settings`

    - `php artisan add-text-pages`

    - `php artisan copy-default-images`

    - `php artisan setup-admin-user`

        This has multiple steps, and will create an admin user on your local site.

        I recommend answering `yes` when the prompts ask if you are on a local/testing instance and when they ask if you want to mark your email address as verified.

### Run Lorekeeper

1. Enter `php artisan serve` in the terminal.

2. Open your preferred web browser and navigate to [http://localhost:8000/](http://localhost:8000/).

    You should now see a blank instance of Lorekeeper.

3. To take your testing site offline, enter `Ctrl + C` in the terminal or close out Visual Studio Code.
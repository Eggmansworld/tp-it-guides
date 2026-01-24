If these tools, dats, or archives save you time or preserve history, consider supporting the work:

<a href="https://buymeacoffee.com/eggmansworld">
  <img src="https://cdn.buymeacoffee.com/buttons/v2/default-orange.png" height="45" alt="Buy Me a Coffee">
</a>


# Incredible Technologies Super Easy Universal Setup Guide

The databases for all iT games are now setup automatically by the TPUI app when the games are run. There is no longer a need to edit anything in the databases. This universal guide applies to ALL Incredible Technologies games and will help you:

- install PostgreSQL v8.3,
- restore a game database from your game backup, and
- make one simple setting change to the imported database to finish the install.

For historical purposes, the original per-game guides are still available in the releases section and can still be used to setup your game databases.  It is NOT recommended to use the old guides. The TPUI app will setup and make the necessary changes to the game databases even if you do them yourself - you're just wasting your time messing with tables when you could be playing the games instead.

---

# INTRODUCTION

This universal guide is created to assist people with setting up their legally owned backups of their incredible Technologies (iT) games, to be used with the TeknoParrot application (TPUI). This guide assumes that the reader has a basic understanding of how to use a Windows computer. It has been written with a level of detail that even an inexperienced user should be able to follow. If you have your legally owned backup of the game, you have everything you need for this setup.

Do not be alarmed at the length of the guide. Most of it is screenshots to visually assist you along the way. 
- For a first-time full Postgres install and your first game database restore: 10-15 minutes
- If you have Postgress already installed and you're adding another game database:  2 minutes
- Take your time! Read each step first to understand it, then perform the step. 
- Do not skip over anything or work ahead. This guide is meant to be done in the order it is presented, from start to finish. 
- If you miss a step or do something incorrectly along the way thinking you're smarter than the guide, it is very likely you will experience problems when running the game.
- **Don’t panic!** Retrace your steps, figure out where you went wrong and make the necessary correction(s).
- If you're really having problems, ask for help in the Teknoparrot Discord #golden-tee channel.

---

# PRE-SETUP

- If you have NOT installed PostgreSQL v8.3 on your Windows PC yet, carry on to the next section titled “PostgreSQL v8.3 Install”.  
- If you have already installed PostgreSQL, skip ahead to the “**PostgreSQL CREATE DATABASE**” section to setup a game database. 
- Ensure each of your game’s files are extracted to their respective folders – one game per folder.

---

# PostgreSQL v8.3 INSTALL

**First, let's calm your fear and doubt about installing an app you may be unfamiliar with. When you install PostgreSQL v8.3 on your Windows computer, you're simply setting up a local database server that stores and organizes information in a structured way — similar to how Microsoft Excel uses rows and columns. It runs quietly in the background and only responds to programs or tools you authorize to connect to it, which in this case will only be the TeknoParrot app. PostgreSQL doesn’t collect or send your personal data anywhere by default, and it doesn’t interfere with other programs on your PC. It's a trusted, open-source tool used by developers, researchers, and businesses around the world. As long as you follow this guide and keep your system secure, there’s nothing risky about having it installed.**

# DOWNLOAD THE POSTGRES v8.3 installer from the release section of this repository
- For your own safety, do not download the installer from anywhere else other than here. 
- Do NOT install a different Postgres version. If you did, uninstall it and install v8.3. 
- Do NOT deviate from the default installation path. 
- DO NOT use a pre-installed or pre-configured standalone Postgres database folder, thinking your clever or you paid some drive seller for a sketchy setup. This guide will not help you at all and you're on your own.

---

You must be logged into your Windows PC with an account that has administrator privileges.

1.  Using the Windows Search function, type “Services” and run the app.

    <img width="303" height="113" alt="image" src="https://github.com/user-attachments/assets/e187315d-b40e-4cc6-b04f-762b04b3a3d8" />

2.  In the list of Services, find “**Secondary Login**”
    - Double-click the name to open the **Secondary Login** properties
    - Change the Startup Type to **Automatic**
    - Under “Service Status”, click the **Start** button
    - Click on OK and close the Services window.
  
    <img width="486" height="427" alt="image" src="https://github.com/user-attachments/assets/b6b34d9d-449c-4290-bd20-7144575c6866" />

3.  Inside the Postgres install folder you downloaded and extracted, run “**SETUP.bat**”. VC2005 is installed first, then the Postgres installer starts after. If VC2005 is already installed on your system, you can ignore the “another version already installed” message and continue the Postgres installation.  DO NOT USE ANY OTHER VERSION OF POSTGRES OTHER THAN v8.3.

    <img width="501" height="251" alt="image" src="https://github.com/user-attachments/assets/fc885d80-e78b-4c4b-92e2-7ff6a220e5bb" />

4.  Click through the PostgreSQL installer, changing NOTHING (use all default installer settings), until you get to the **Service configuration** window.

5.  On the Service Configuration window, leave everything as default except:
- for the Account Password box, type in a strong password. Postgres wants to see a password that contains, at minimum, a combination of upper and lower case letters + numbers. 
- This account creation is for the Service Account in your Windows OS that is responsible for starting and running the Postgres database server in the background.
- This account has absolutely **nothing** to do with the TeknoParrot frontend and is only used to run the Postgres server as a system service.
- Store the password somewhere safe.
- If you need to remove this account later on, see the Troubleshooting/Help section at the end of this document.

6.  Click **Next**. You’ll see a prompt that the “postgres user was not found” and if you want the account to be created. Select **Yes**

    NOTE: If you used a weak password, you’ll be prompted if you want Postgres to replace it with a random password. It is best to choose “No”, then click the Back button to change your password to something stronger. If you choose “Yes”, then it will create a random 32-character scrambled password which you have to write down by hand (no copy/paste available!)

    <img width="499" height="385" alt="image" src="https://github.com/user-attachments/assets/6e3751a3-8658-49ea-b239-83e06847eb96" />

7.  On the next screen titled “**Initialize database cluster”**, make the necessary changes to match the screenshot below. This step creates the internal Postgres database **superuser** account.  
    - **The “C” Locale must be selected.** It is in the pulldown list at the start of the locales that begin with the letter C. I will not explain why this is used other than to say it is performance related for the database only.
    - Both Encoding settings must be set to **UTF8**.
    
       <img width="496" height="382" alt="image" src="https://github.com/user-attachments/assets/92c05458-3114-4a99-9a5e-a82362d1c780" />
       <img width="380" height="304" alt="image" src="https://github.com/user-attachments/assets/8a8ca720-8028-4126-87c0-2841e216436a" />
    
    - This is the account you use to open and manage the game databases in the pgAdmin app, and is also the account that TPUI uses to access the game database.
    - Yes, it has the same name as the Windows Service account, but this account is only used to access the database, not run it.
    - There is no requirement to make a strong or complex password, just make it something easy to remember. A memorable example would be “teknoparrot”.

8.  Click **Next** to continue

9.  Click **Next** on all subsequent screens, changing nothing, until the “Installation complete” window comes up

10.  Uncheck the “**Launch Stack Builder at exit**”

11. Click **Finish**

12. **REBOOT YOUR COMPUTER! THIS MUST BE DONE BEFORE YOU CONTINUE. Do not try to be clever and skip this step.**
- After rebooting, you are now done installing PostgreSQL on your computer and you are ready to create and import game databases.
- YOU ONLY NEED TO INSTALL POSTGRES ONCE ON YOUR PC. You use the pgAdmin app to add game databases to the Postgres install. That's all there is to it.

# PostgreSQL CREATE DATABASE

1.  From the Windows Start Menu -\> **PostgreSQL 8.3** folder, click on **pgAdmin III** (or use Windows Search to find the app). **pgAdmin** is the program that you use to setup and manage your game databases.

3.  When the **pgAdmin** window opens:
    - Under **Servers**, double-click on **PostgreSQL Database Server 8.3**
    - type in your postgres **superuser** password you created earlier to connect to the database.

    <img width="422" height="263" alt="image" src="https://github.com/user-attachments/assets/494caf1a-7a09-4817-b665-e60fba059745" />

4.  In the Object browser tree, right-click on **Databases** and select **New Database**  

    <img width="419" height="130" alt="image" src="https://github.com/user-attachments/assets/f1a3fdde-d252-47a2-90b6-b8d64d0859fd" />

5.  On the Properties tab, you need to give the new database a name. The name can be anything you want but keep it simple and use only alphanumeric characters. In this example, we are using the default database name for Golden Tee 2014.
   
    **Reference Appendix A at the bottom of this document for a full list of games and their TPUI default database names.**  
      
    - **Name**: GameDB14
    - **Owner**: postgres
    - **Encoding**: SQL_ASCII      (note: all games use SQL_ASCII encoding except for Golden Tee 2006, which uses UTF8)
    - **Template**: template0
    - **Tablespace**: pg_default
  
    <img width="396" height="522" alt="image" src="https://github.com/user-attachments/assets/5cb01352-4637-4155-be8a-541b68125df2" />
   
6. Click on the “**Variables**” tab.
    - under **Variable Name**, use the pulldown menu and select "**standard_conforming_strings**"
    - put a checkmark in the **Variable Value** box
    - click the **Add/Change** button, and it will add the new variable in the list.
    - click **OK** to exit
      
    <img width="393" height="519" alt="image" src="https://github.com/user-attachments/assets/00e4e833-3c9a-4f4a-b6e4-ef1e8e6661a3" />
   
7.  The new database will be created and is now located under **Databases**. Don’t worry if there is a red X in it, it just means you haven’t selected it. The red X will go away when you click on the database.

# RESTORE GAME DATABASE

1.  Right-click on your newly created database and select **Restore**

    <img width="228" height="387" alt="image" src="https://github.com/user-attachments/assets/668387a3-1d8a-420c-b799-24d14de05893" />

2.  On the “Filename” line, click the “…” file browse button, then **browse to the game folder location where you extracted your game files.**  

    <img width="639" height="122" alt="image" src="https://github.com/user-attachments/assets/8f5fd1dd-aaa8-4760-8eeb-e04d1f7808ed" />

     - The backup files have no file extension, so change the File type from “Backup files (\*.backup)” to “**All Files (\*.\*)**” to be able to see them.
     - **Reference Appendix A** for the location and name of each specific game file backup.
     - All of the backups are in a folder called “pg_backup”, and may or may not have a YYYY-MM-DD subfolder that contain the backup files.
     - You will load the backup file that is in a folder with the newest date and/or starts with the highest 4-digit number.
     - It’s as simple as that.

    <img width="853" height="393" alt="image" src="https://github.com/user-attachments/assets/86638341-b2ef-4453-a80f-e62600fe3af5" />

3.  Left click on the file to highlight it, then click **Open**. The file path will now be added to the Restore Database filename line.

4.  Changing no other options, click on **OK**.

    <img width="400" height="378" alt="image" src="https://github.com/user-attachments/assets/de8fd796-9f6b-4c1e-8db6-e9aa078a287e" />

5.  The database will be imported. **IGNORE the warning about the errors on restore.**  
      
    Click on **CANCEL**. Yes it’s counter-intuitive, just do it. You're done!
      
    <img width="719" height="583" alt="image" src="https://github.com/user-attachments/assets/13033034-1387-4209-9f1b-0be9ef2e89fa" />

# START THE TEKNOPARROT FRONTEND

1.  If you haven’t already, add the game to your TP library. If you don’t see it, run the internal updater.

2.  Select the game from the game library list, then click on **Game Settings**

3.  click on the Game Executable file path, browse to your game folder location, select the “**game.bin**” file and click on **Open**

4.  The default settings are as follows. If you have setup Postgres as per this guide and didn’t deviate from the recommended settings, you only need to change the DbName and enter the postgres password. If you’ve gone out on your own and customized your install path or any other settings, make the appropriate changes as necessary.
      
    Postgres – Path:      C:\Program Files (x86)\PostgreSQL\8.3\bin\\  
    Postgres – Address:   127.0.0.1  
    Postgres – Port:      5432  
    Postgres – DbName:    see Appendix A for each game’s default database name  
    Postgres – Username:  postgres  
    Postgres – Pass:      type your postgres superuser password here (NOTE: THE PASSWORD LINE IS EMPTY BY DEFAULT!)

Your settings should look something like this:

  <img width="792" height="264" alt="image" src="https://github.com/user-attachments/assets/c560399e-c151-4613-955e-81478beb60ed" />

5.  click on **Save Settings**

6.  Click on **Controller Setup** and assign keyboard keys to each game button/function as desired

7.  **Click on LAUNCH GAME.**

For questions about different game settings and problems with gameplay, please visit the TeknoParrot Discord and ask your questions there.

# APPENDIX A

These are the default TPUI game database names and restore settings. As stated earlier, you can call the databases whatever you want, but be sure you update the game settings with your database name.

**Golden Tee Live 2006** (1.00.48)   
DB name: GameDB06   
Encoding: UTF8   
db backup location: \pg_backup\2017-12-18  
db filename: 1922-postgresql_database-GameDB-backup

**Golden Tee Live 2007** (2.00.20)  
DB name: GameDB07   
Encoding: SQL_ASCII   
db backup location: \pg_backup\2017-12-18\   
db filename: 2043-postgresql_database-GameDB-backup

**Golden Tee Live 2008** (3.00.12)   
DB name: GameDB08   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2016-01-30\   
db filename: 1850-postgresql_database-GameDB-backup

**Golden Tee Live 2009** (4.00.07)   
DB name: GameDB09   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2015-07-27\   
db filename: 2035-postgresql_database-GameDB-backup

**Golden Tee Live 2010** (5.00.10)   
DB name: GameDB10   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2017-12-06\   
db filename: 1442-postgresql_database-GameDB-backup

**Golden Tee Live 2011** (6.00.09)   
DB name: GameDB11   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2017-12-06\   
db filename: 1621-postgresql_database-GameDB-backup

**Golden Tee Live 2012** (7.00.10)   
DB name: GameDB12   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2025-03-22\   
db filename: 1826-postgresql_database-GameDB-backup

**Golden Tee Live 2013** (8.05.09)   
DB name: GameDB13   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2025-03-22\   
db filename: 1920-postgresql_database-GameDB-backup

**Golden Tee Live 2014** (9.04.06)   
DB name: GameDB14   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2018-01-29\   
db filename: 2202-postgresql_database-GameDB-backup

**Golden Tee Live 2015** (10.04.06)   
DB name: GameDB15   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2018-01-30\   
db filename: 0206-postgresql_database-GameDB-backup

**Golden Tee Live 2016** (11.04.10)   
DB name: GameDB16   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2018-01-07\   
db filename: 1942-postgresql_database-GameDB-backup

**Golden Tee Live 2017** (12.04.05)   
DB name: GameDB17   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2018-01-30\   
db filename: 0241-postgresql_database-GameDB-backup

**Orange County Choppers Pinball** (0.00.51)   
DB name: GameDBOCC   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2020-09-12\   
db filename: 1549-postgresql_database-GameDB-backup

**Power Putt Live 2012** (1.01.08)   
DB name: GAMEDBPP12   
Encoding: SQL_ASCII    
db backup location: \pg_backup\   
db filename: 1433-postgresql_database-GameDB-backup

**Power Putt Live 2013** (2.01.08)   
DB name: GAMEDBPP13   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2023-10-27\   
db filename: 2042-postgresql_database-GameDB-backup

**Silver Strike Bowling Live** (3.02.07)   
DB name: GameDBSSB   
Encoding: SQL_ASCII    
db backup location: \pg_backup\   
db filename: 2222-postgresql_database-GameDB-backup

**Target Toss Pro Bags** (1.00.10)   
DB name: GameDBBags   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2017-11-16\   
db filename: 0136-postgresql_database-GameDB-backup

**Target Toss Pro Lawn Darts & Bags** (2.00.05)   
DB name: GameDBLawndarts   
Encoding: SQL_ASCII    
db backup location: \pg_backup\2017-05-28\   
db filename: 2104-postgresql_database-GameDB-backup

# TROUBLESHOOTING AND HELP

# Postgres account modifications

Postgres Windows Service Account
  - do a Windows Search for “Computer Management” and run the app. Expand the “Local Users and Groups” branch and click on "Users".
    - to delete the account, right click the "postgres" account and select Delete
    - to change the password, right click and select "Set Password". Click on Proceed, and follow on-screen instructions.

  <img width="975" height="73" alt="image" src="https://github.com/user-attachments/assets/0f31046c-421f-4a91-9ed6-319c861ca998" />

Postgres SuperUser Database Account (the account you use in TPUI settings):
  - in pgAdmin, connect to the server using your current credentials
  - In the Object browser, go to Login Roles and expand the branch to view the users
    - to change the password, right-click the user (e.g., postgres) and select Properties. Enter a new password and click OK.
  
  <img width="287" height="132" alt="image" src="https://github.com/user-attachments/assets/7f43a277-4d28-441b-8e5b-f16f356bfdc3" />

# Restrict PostgreSQL Access to Localhost Only

If you followed the above guide to setup Postgres on your computer, restricting access to localhost only is already setup by default. This means the Postgres service will only respond to local users and communication to and from the local database. This information is provided to bring attention on how you would check to ensure the config files are setup correctly. It would be highly recommended to check this over if you obtained a pre-configured TeknoParrot setup from another source, such as paying money for a pre-configured hard drive from a dirtbag drive seller. In the interest of security, I'm providing this information for your own safety.

**Step 1: Edit postgresql.conf**
  - Find the postgresql.conf file (usually in C:\Program Files\PostgreSQL\8.3\data\).
  - Open it with a text editor (like Notepad run as administrator).
  - Look for this line:
  -     #listen_addresses =

    Uncomment the line and change it so it reads:
  -     listen_addresses = 'localhost'
    This tells PostgreSQL to only listen for connections on the local computer.
  - Save and close the file.
  
**Step 2: Edit pg_hba.conf**
  - In the same folder (data\), open pg_hba.conf.
  - Scroll to the bottom of the file. You’ll see a list of access rules. Make sure only localhost access is allowed, like this:

<img width="542" height="108" alt="image" src="https://github.com/user-attachments/assets/fa255502-aa25-41bf-ae05-8b8af3b0b74b" />

  - This allows any user to connect to any database — but only from the local machine using a password (md5).
  - If there are other host lines that allow wider access (e.g., 0.0.0.0/0 or other IPs), comment them out by adding a # at the start of the line.
  - Save and close the file.
  
**Step 3: Restart the PostgreSQL Service**
  To apply your changes:
  - Open Services (services.msc)
  - Find PostgreSQL 8.3, right-click, and click Restart
  
That’s it. PostgreSQL will now:
  - Only listen for connections from localhost
  - Only allow access via password from 127.0.0.1
  - Block all remote access attempts

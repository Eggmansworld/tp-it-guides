# The Super Easy Universal Setup Guide for all TeknoParrot iT Games

All internal database changes are now done automatically by the TPUI app, so this guide below now applies universally to all iT games in TeknoParrot.

For historical purposes, the older individual game guides are still available in the releases section, and **can still be used** to setup your game databases.

# INTRODUCTION

This universal guide is created to assist people with setting up their legally owned backups of their incredible Technologies (iT) games, to be used with the TeknoParrot application (TPUI). This guide assumes that the reader has a basic understanding of how to use a Windows computer. It has been written with a level of detail that even an inexperienced user should be able to follow. Do not be alarmed at the number of pages – there are lots of screenshots to visually assist you along the way. If you have your legally owned backup of the game, you have everything you need for this setup.

ESTIMATED TIME FOR FIRST-TIME FULL POSTGRES INSTALL AND DATABASE SETUP: **10 minutes**

PRE-EXISTING POSTGRES INSTALL, DATABASE SETUP ONLY: **2 minutes**
  
- Take your time! 
- Read each step first to understand it, then perform the step. 
- Do not skip over anything or work ahead. 
- This guide is meant to be done in the order it is presented, from start to finish. 
- If you miss a step or do something incorrectly along the way, it is very likely you will experience problems when running the game. **Don’t panic!** Retrace your steps, figure out where you went wrong and make the necessary correction(s).

# PRE-SETUP

If you have NOT setup PostgreSQL yet, carry on to the next section titled “PostgreSQL Install”.  
  
If you have already installed PostgreSQL from setting up another iT game, skip ahead to the “**PostgreSQL CREATE DATABASE**” section to install another game. Do not make any changes to existing Postgres databases! To add a new game, you simply need to create a new database for the game and make the necessary adjustments as instructed.  
  
Ensure each game’s files are in a folder – one game per folder. Place the game folders in a static location where you will run your TP games from. Leave the game folder(s) alone for now, we’ll come back to them later.

# PostgreSQL INSTALL

**First, let's calm your fear and doubt about installing an app you may be unfamiliar with. When you install PostgreSQL on your Windows computer, you're simply setting up a local database server that stores and organizes information in a structured way — similar to how Microsoft Excel uses rows and columns. It runs quietly in the background and only responds to programs or tools you authorize to connect to it, which in this case will only be the TeknoParrot app. PostgreSQL doesn’t collect or send your personal data anywhere by default, and it doesn’t interfere with other programs on your PC. It's a trusted, open-source tool used by developers, researchers, and businesses around the world. As long as you follow this guide and keep your system secure, there’s nothing risky about having it installed.**

You must be logged into your Windows PC with an account that has administrator privileges.

1.  Using the Windows Search function, type “Services” and run the app.

    <img width="303" height="113" alt="image" src="https://github.com/user-attachments/assets/e187315d-b40e-4cc6-b04f-762b04b3a3d8" />

2.  In the list of Services, find “**Secondary Login**”

    a.  Double-click the name to open the **Secondary Login** properties

    b.  Change the Startup Type to **Automatic**

    c.  Under “Service Status”, click the **Start** button

    d.  Click on OK and close the Services window.

    <img width="486" height="427" alt="image" src="https://github.com/user-attachments/assets/b6b34d9d-449c-4290-bd20-7144575c6866" />

3.  Inside the folder you extracted from the download, run “**SETUP.bat**”. VC2005 is installed first, then the Postgres installer starts after. If VC2005 is already installed on your system, you can ignore the “another version already installed” message and continue the Postgres installation.

4.  Click through the PostgreSQL installer, changing NOTHING (use all default installer settings), until you get to the **Service configuration** window.

5.  On the Service Configuration window, leave everything as default except for the Account Password box, type in a strong password. Postgres wants to see a password that contains, at minimum, a combination of upper and lower case letters, and numbers. This creates a Service Account in your Windows OS that is responsible for starting and running the Postgres database server in the background. This account has absolutely **nothing** to do with the TeknoParrot frontend and is only used to run the Postgres server as a system service. STORE THIS PASSWORD SOMEWHERE SAFE.

6.  Click **Next**. You’ll see a prompt that the “postgres user was not found” and if you want the account to be created. Select **Yes**

    Note: If you used a weak password, you’ll be prompted if you want Postgres to replace it with a random password. It is best to choose “No”, then click the Back button to change your password to something stronger. If you choose “Yes”, then it will create a random 32-character scrambled password which you have to write down by hand (no copy/paste available!)

  <img width="478" height="366" alt="image" src="https://github.com/user-attachments/assets/194cd9de-cffa-4341-9efe-ba7403809688" />

7.  On the next screen titled “**Initialize database cluster”**, make the necessary changes to match the screenshot below. This step creates the internal Postgres database **superuser** account.  

- **The “C” Locale must be selected.** It is in the pulldown list at the start of the locales that begin with the letter C. I will not explain why this is used other than to say it is performance related for the database only.
- This is the account you use to manage the game databases in Postgres.
- Yes, it has the same name as the Windows Service account, but this account is only used to access the database, not run it.
- This is the account that TPUI uses to access the game database
- It is highly advisable to make the password different than the Service Account password. There is no requirement to make it strong or complex, just make it something you can remember. A memorable example would be “teknoparrot”.

  <img width="558" height="427" alt="image" src="https://github.com/user-attachments/assets/f605bfa4-ab00-4efa-882c-8f87a453bac6" />

8.  Click **Next** to continue

9.  Click **Next** on all subsequent screens, changing nothing, until the “Installation complete” window comes up

10.  Uncheck the “**Launch Stack Builder at exit**”

11. Click **Finish**

12. **REBOOT YOUR COMPUTER! This must be done before you continue.**

After rebooting, you are now done installing PostgreSQL on your computer and you are ready to create and import the game database.

# PostgreSQL CREATE DATABASE

1.  From the Windows Start Menu -\> **PostgreSQL 8.3** folder, click on **pgAdmin III** (or use Windows Search to find the app). **pgAdmin** is the program that allows you to connect to the database server and manage the database you will be setting up in the following steps.

2.  When the **pgAdmin** window opens:

    1.  Under **Servers**, double-click on **PostgreSQL Database Server 8.3**

    2.  type in your postgres **superuser** password you created earlier to connect to the database. In the example setup in the previous section, the example password was “teknoparrot”.

<img width="422" height="263" alt="image" src="https://github.com/user-attachments/assets/494caf1a-7a09-4817-b665-e60fba059745" />

3.  In the Object browser tree, right-click on **Databases** and select **New Database**  

<img width="419" height="130" alt="image" src="https://github.com/user-attachments/assets/f1a3fdde-d252-47a2-90b6-b8d64d0859fd" />

4.  On the Properties tab, we need to give the new database a name. The name can be anything you want but keep it simple and use only alphanumeric characters.  
   
    **Reference the bottom of this document (Appendix A), for a full list of games and their TPUI default database names.**  
      
    **Name**: GameDB14
    
    **Owner**: postgres
    
    **Encoding**: SQL_ASCII
          (note: all games have this encoding except for Golden Tee 2006)
    
    **Template**: template0
    
    **Tablespace**: pg_default
          
6. Click on the “**Variables**” tab.
    a. under **Variable Name**, use the pulldown menu and select "**standard_conforming_strings**"
    b. put a checkmark in the **Variable Value** box
    c. click the **Add/Change** button, and it will add the new variable in the list.
    d. click **OK** to exit

<img width="396" height="522" alt="image" src="https://github.com/user-attachments/assets/5cb01352-4637-4155-be8a-541b68125df2" />

<img width="393" height="519" alt="image" src="https://github.com/user-attachments/assets/00e4e833-3c9a-4f4a-b6e4-ef1e8e6661a3" />

5.  The new database will be created and is now located under **Databases**. Don’t worry if there is a red X in it, it just means you haven’t selected it. The red X will go away when you click on the database.

# RESTORE GAME DATABASE

6.  Right-click on your newly created database and select **Restore**

<img width="228" height="387" alt="image" src="https://github.com/user-attachments/assets/668387a3-1d8a-420c-b799-24d14de05893" />

7.  On the “Filename” line, click the “…” file browse button, then **browse to the game folder location where you extracted your game files.**  

<img width="639" height="122" alt="image" src="https://github.com/user-attachments/assets/8f5fd1dd-aaa8-4760-8eeb-e04d1f7808ed" />

   a.  The backup files have no file extension, so change the File type from “Backup files (\*.backup)” to “**All Files (\*.\*)**” to be able to see them.

   b.  **Reference Appendix A** for the location and name of each specific game file backup. All of the backups are in a folder called “pg_backup”, and may or may not have a YYYY-MM-DD subfolder that contain the backup files. You will load the backup file that starts with the highest 4-digit number. It’s as simple as that.

<img width="853" height="393" alt="image" src="https://github.com/user-attachments/assets/86638341-b2ef-4453-a80f-e62600fe3af5" />

3.  Left click on the file to highlight it, then click **Open**. The file path will now be added to the Restore Database filename line.

4.  Changing no other options, click on **OK**.

<img width="400" height="378" alt="image" src="https://github.com/user-attachments/assets/de8fd796-9f6b-4c1e-8db6-e9aa078a287e" />

5.  The database will be imported. **IGNORE the warning about the errors on restore.**  
      
    Click on **CANCEL**. Yes it’s counter-intuitive, just do it.  
      
<img width="719" height="583" alt="image" src="https://github.com/user-attachments/assets/13033034-1387-4209-9f1b-0be9ef2e89fa" />

6.  **TPUI now sets up all the incredible Technologies games database settings automatically, so you are now finished the database setup.  **

# START THE TEKNOPARROT FRONTEND

1.  If you haven’t already, add the game to your TP library. If you don’t see it, UPDATE TP.

2.  Select the game from the game library list, then click on **Game Settings  **

3.  click on the Game Executable file path, browse to your game folder location, select the “**game.bin**” file and click on **Open**

4.  The default settings are as follows. If you have setup Postgres as per this guide and didn’t deviate from the recommended settings, you only need to change the DbName and enter the postgres user password. If you’ve gone out on your own and customized your     install path or any other settings, make the appropriate changes as necessary.  
      
    Postgres – Path:      C:\Program Files (x86)\PostgreSQL\8.3\bin\\  
    Postgres – Address:   127.0.0.1  
    Postgres – Port:      5432  
    Postgres – DbName:    see Appendix A for each game’s default database name
    Postgres – Username:  postgres  
    Postgres – Pass:      type your postgres superuser password here
      
    NOTE: THE PASSWORD LINE IS EMPTY BY DEFAULT!

Your settings should look something like this:

<img width="792" height="264" alt="image" src="https://github.com/user-attachments/assets/c560399e-c151-4613-955e-81478beb60ed" />

5.  click on **Save Settings**

6.  Click on **Controller Setup** and assign keyboard keys to each game
    button/function as desired

7.  **Click on LAUNCH GAME.**

YOU’RE DONE!  
  
For questions about different game settings and problems with gameplay, please visit the TeknoParrot Discord and ask your questions there.


# APPENDIX A

Golden Tee Live 2006
DB name: GameDB06  
Encoding: **UTF8**
db backup location: \pg_backup\2017-12-18\1922-postgresql_database-GameDB-backup  
  
Golden Tee Live 2007  
DB name: GameDB07  
Encoding: **SQL_ASCII**  
db backup location: \pg_backup\2017-12-18\2043-postgresql_database-GameDB-backup

Golden Tee Live 2008  
DB name: GameDB08  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2016-01-30\1850-postgresql_database-GameDB-backup

Golden Tee Live 2009  
DB name: GameDB09  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2015-07-27\2035-postgresql_database-GameDB-backup

Golden Tee Live 2010  
DB name: GameDB10  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2017-12-06\1442-postgresql_database-GameDB-backup

Golden Tee Live 2011  
DB name: GameDB11  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2017-12-06\1621-postgresql_database-GameDB-backup

Golden Tee Live 2012  
DB name: GameDB12  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2025-03-22\1826-postgresql_database-GameDB-backup

Golden Tee Live 2013  
DB name: GameDB13  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2025-03-22\1920-postgresql_database-GameDB-backup

Golden Tee Live 2014  
DB name: GameDB14  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2018-01-29\2202-postgresql_database-GameDB-backup

Orange County Choppers Pinball  
DB name: GameDBOCC  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2020-09-12\1549-postgresql_database-GameDB-backup

Power Putt Live 2012  
DB name: GAMEDBPP12  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\1433-postgresql_database-GameDB-backup

Power Putt Live 2013  
DB name: GAMEDBPP13  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2023-10-27\2042-postgresql_database-GameDB-backup

Silver Strike Bowling Live  
DB name: GameDBSSB  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2222-postgresql_database-GameDB-backup

Target Toss Pro Bags  
DB name: GameDBBags  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2017-11-16\0136-postgresql_database-GameDB-backup

Target Toss Pro Lawn Darts & Bags  
DB name: GameDBLawndarts  
Encoding: **SQL_ASCII**   
db backup location: \pg_backup\2017-05-28\2104-postgresql_database-GameDB-backup

# MISCELLANEOUS HELP

# Postgres account modifications
- to remove the Postgres service account, do a Windows Search for “Computer Management” and run the app. Under “Local Users and Groups/Users”, right click and delete the “postgres” account.  
  
  <img width="975" height="73" alt="image" src="https://github.com/user-attachments/assets/0f31046c-421f-4a91-9ed6-319c861ca998" />

- to change the password on the Postgres service account, go to the same location as above, right click the postgres user and select "change password".

- to remove the Postgres database account, uninstall the PostgreSQL app from Windows, or right-click on the database server in pgAdmin and select Delete/Drop.  You're on your own to recreate your database server.
  
  <img width="287" height="132" alt="image" src="https://github.com/user-attachments/assets/7f43a277-4d28-441b-8e5b-f16f356bfdc3" />

- to change the password on the Postgres database account (the one you type into TPUI settings):
  - Open pgAdmin III
  - Connect to the server using your current credentials
  - In the Object browser, go to Login Roles, expand the tree and right-click the user (e.g., postgres) and select Properties
  - Enter a new password and click OK.




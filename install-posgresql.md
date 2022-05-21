## Install PosgreSQL

## Download PosgreSQL
PosgreSQL can be downloaded [here](https://www.postgresql.org/download/). At this time (21-May-2022) latest version of PostgreSQL is 14.3.  
![Download PosgreSQL](setup-git/images/postgresql-download.png)
* Download process can be done using browser.
* Or using PowerShell  
```sh
>md d:\git-docs
>cd d:\git-docs
>Invoke-WebRequest https://get.enterprisedb.com/postgresql/postgresql-14.3-1-windows-x64.exe -OutFile posgresql.exe
>.\posgresql.exe
```

PowerShell downloading PostgreSQL like this.  
![PostgreSQL downloading](setup-git/images/posgre-downloading.png)

---
Follow installing instruction.  
![Setup PostgreSQL - step 1](setup-git/images/postgresql-setup-s1.png)

Choose install directory - suggest to keep default value.  
![Setup PostgreSQL - step 2](setup-git/images/postgresql-setup-s2.png)

Choose components to install - suggest to keep default value.  
![Setup PostgreSQL - step 3](setup-git/images/postgresql-setup-s3.png)

Select directory to store the data - suggest to keep default value.  
![Setup PostgreSQL - step 4](setup-git/images/postgresql-setup-s4.png)

Enter password for default logins. Default Username is: postgres  
![Setup PostgreSQL - step 5](setup-git/images/postgresql-setup-s5.png)

Enter `port` for PostgreSQL connection. If you don't have conflict or special purpose, please keep default value 5432.  
![Setup PostgreSQL - step 6](setup-git/images/postgresql-setup-s6.png)

Choose default locale of database - keep default if you don't have special purpose.  
![Setup PostgreSQL - step 7](setup-git/images/postgresql-setup-s7.png)

Review selected options.  
![Setup PostgreSQL - step 8](setup-git/images/postgresql-setup-s8.png)

Confirm installation.  
![Setup PostgreSQL - step 9](setup-git/images/postgresql-setup-s9.png)

Installation in progress  
![Setup PostgreSQL - step 10](setup-git/images/postgresql-setup-s10.png)

Wait and finish installation.  
![Setup PostgreSQL - step 11](setup-git/images/postgresql-setup-s11-finish.png)

**Note: Computer my restart to apply the changes.**

### Test PostgreSQL
You can use pgAdmin win all document can be found [here](https://www.pgadmin.org/docs/pgadmin4/development/getting_started.html).
I use HeidiSQL as my favour. It's an open source tool, lightweight yet powerful and easy to use. HeidiSQL can be downloaded [here](https://www.heidisql.com/).

Open HeidiSQL.  
![HeidiSQL](setup-git/images/heidisql-s1.png)  

Click `New` to add a connection session. Input information as the image. Imput user name `posgres`, password `use password you enter at installation step`. Click `Open`, if all information is correct and PostgreSQL install OK, you should go into manage page.
![HeidiSQL connection](setup-git/images/heidisql-s2.png)  
![HeidiSQL manage](setup-git/images/heidisql-s3.png)  

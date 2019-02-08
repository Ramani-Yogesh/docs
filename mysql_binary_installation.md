==================================================

Mysql_Binary_Installation

=================================================

* Check the  **mysql service** is currently running or not, if yes, stop the service and remove all the previous setup

        ps -ef | grep mysql
        
* Create a **mysql** user and group
        
        sudo useradd -r  -s /bin/false mysql 
        
   ==>  -r means system user id range. Check "mysql" group is created.
   
        grep mysql /etc/group
        
* Click **[here](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.25-linux-glibc2.12-x86_64.tar)** to download the mysql tar file.

* Create the directory and move the tar file to that location

        sudo mkdir /data/
        
        cd /data/
        
    I have placed the tar file in the above location.
    
* List the zip files inside the tar.

        tar -tvf mysql-5.7.25-linux-glibc2.12-x86_64.tar         
        
* Extract the tar file.

        sudo tar -xvf mysql-5.7.25-linux-glibc2.12-x86_64.tar
        
    Now you have a two gzip files. Extract **mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz** this gzip file. 
       
* Extract the gzip file.

        sudo tar -xvzf mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz
        
* Now you have a new directory **mysql-5.7.25-linux-glibc2.12-x86_64**

* For the convenient I have rename the **mysql-5.7.25-linux-glibc2.12-x86_64** directory to **mysql-5.7**

        sudo mv mysql-5.7.25-linux-glibc2.12-x86_64 mysql-5.7
        
* Change into the directory

        cd mysql-5.7/
        
* Create  a **my.cnf** file in that directory

         sudo vim my.cnf
         
    Add the below contents to the file.

        [mysqld]
        user            = mysql
        pid-file        = /data/mysql-5.7/mysql/mysqld.pid
        socket          = /data/mysql-5.7/mysql/mysqld.sock
        port            = 3388
        basedir         = /data/mysql-5.7
        datadir         = /data/mysql-5.7/data_dir
        log_error = /data/mysql-5.7/mysql/error.log
        
    Here I mention the port number **3388**. You can change the port number as per your request.
    
* Create a data directory.

    Now I am creating a **data_dir** and **mysql** directory based on the **my.cnf** file.
    
        sudo mkdir /data/mysql-5.7/data_dir
        
        sudo mkdir /data/mysql-5.7/mysql
        
* Change the ownership to **mysql**.

         sudo chown -R mysql:mysql  /data/
         
    Go to the **bin** directory
    
        sudo cd /data/mysql-5.7/bin
        
* Start the *mysqld* 

        sudo ./mysqld --defaults-file=/data/mysql-5.7/my.cnf --initialize
        
    If it shows error related to **libaio.so.1**. You need to install that package and run again.
    
        sudo apt-get install libaio1
        
        sudo ./mysqld --defaults-file=/data/mysql-5.7/my.cnf --initialize
        
* After you start the **mysqld** without any error. You need to check your error log file.

        sudo vim /data/mysql-5.7/mysql/error.log
        
    Here you have a **temporary password** for root user. You have something related to the below content.
    
        2019-02-08T08:13:17.621666Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
        2019-02-08T08:13:17.827244Z 0 [Warning] InnoDB: New log files created, LSN=45790
        2019-02-08T08:13:17.891187Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
        2019-02-08T08:13:18.026192Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 63ffe080-2b79-11e9-b000-08002710132e.
        2019-02-08T08:13:18.033019Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
        2019-02-08T08:13:18.034512Z 1 [Note] A temporary password is generated for root@localhost: eJjAyvq?0vKN
    
    Note the last line. here root password is show. Please note the password.
    
* Now you start the **mysql** with **root** user.

        sudo ./mysqld --defaults-file=/data/mysql-5.7/my.cnf --user=root &
     
    You can check mysql is running or not.
    
        ps -ef | grep mysql
        
* Login to mysql using **root** user

         sudo ./mysql --socket=/data/mysql-5.7/mysql/mysqld.sock -u root -p
         
    **Note:** Here you need to mention the socket location as mentioned in the **my.cnf**.
    
    Then it will ask the root password. Here you need to put the temporary password of root.(Shows in error log file)
    
        mysql> show databases;
   
    After login, if you run any query, it will show error like below. 
    
    **_ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement._**
    
    Because, we login using the temporary password. So, we need to change the root password. 

    So, **exit** from the mysql. 
        
        exit;          
       
* Now do the **mysql_secure_installation**.

         ./mysql_secure_installation  --defaults-file=/data/mysql-5.7/my.cnf

    It shows some error like below
    
    Error: Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)        
    
    We need to add the client details in **my.cnf** file
    
        sudo vim /data/mysql-5.7/my.cnf
        
        [client]
        socket          = /data/mysql-5.7/mysql/mysqld.sock

        [mysqld]
        user            = mysql
        pid-file        = /data/mysql-5.7/mysql/mysqld.pid
        socket          = /data/mysql-5.7/mysql/mysqld.sock
        port            = 3388
        basedir         = /data/mysql-5.7
        datadir         = /data/mysql-5.7/data_dir
        log_error = /data/mysql-5.7/mysql/error.log
              
    Now, you can start the secure installation.
    
        ./mysql_secure_installation  --defaults-file=/data/mysql-5.7/my.cnf/data/mysql-5.7/
        
        Securing the MySQL server deployment.

        Enter password for user root: 

        VALIDATE PASSWORD PLUGIN can be used to test passwords
        and improve security. It checks the strength of password
        and allows the users to set only those passwords which are
        secure enough. Would you like to setup VALIDATE PASSWORD plugin?

        Press y|Y for Yes, any other key for No:   
        Using existing password for root.
        Change the password for root ? ((Press y|Y for Yes, any other key for No) : y

        New password: 

        Re-enter new password: 
        By default, a MySQL installation has an anonymous user,
        allowing anyone to log into MySQL without having to have
        a user account created for them. This is intended only for
        testing, and to make the installation go a bit smoother.
        You should remove them before moving into a production
        environment.

        Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
        Success.


        Normally, root should only be allowed to connect from
        'localhost'. This ensures that someone cannot guess at
        the root password from the network.

        Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
        Success.

        By default, MySQL comes with a database named 'test' that
        anyone can access. This is also intended only for testing,
        and should be removed before moving into a production
        environment.


        Remove test database and access to it? (Press y|Y for Yes, any other key for No) : No

        ... skipping.
        Reloading the privilege tables will ensure that all changes
        made so far will take effect immediately.

        Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
        Success.

        All done! 
        
* Now check you can access the mysql. 

        sudo cd /data/mysql-5.7/bin

        ./mysql --defaults-file=/data/mysql-5.7/my.cnf -u root -p
        
        mysql> show databases;
        
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | mysql              |
        | performance_schema |
        | sys                |
        +--------------------+
        4 rows in set (0.00 sec)

        mysql>  exit;

* Now you restart the system

        sudo reboot
        
* Check the mysql is running or not

        ps -ef | grep mysql
        
    It is not running. So you need to start the process.
    
        sudo ./mysqld --defaults-file=/data/mysql-5.7/my.cnf --user=root &   
        
    Then check and login.
    
        ps -ef | grep mysql
        
        sudo ./mysql --defaults-file=/data/mysql-5.7/my.cnf -u root -p
        
        mysql> show databases;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | mysql              |
        | perfios            |
        | performance_schema |
        | sys                |
        +--------------------+
        5 rows in set (0.04 sec)
        
        mysql> exit;  
        
    Check the port number
    
        netstat -tunlp           
    
        
    

                   
           
    
        
        
    
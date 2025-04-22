## Create New Mysql User with all previledge and Delete old user



1. **Step 1: Log into MySQL as Root or Admin**
    ```
    sudo mysql -u root -p
    ```
2. **Step 2: Check the Existing MySQL Users**

    ```
    SELECT user, host FROM mysql.user;
    ```
   

3. **Step 3: Create a New Secure User**
    ```
    CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'StrongPassword123!';
    
    ```
4. **Step 4: Give previledge to user for specific table**

    ```
        GRANT ALL PRIVILEGES ON your_database.* TO 'newuser'@'localhost';
        FLUSH PRIVILEGES;
    ```

5. **Step : Give previledge to user for all table**

    ```
        GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' WITH GRANT OPTION;
        FLUSH PRIVILEGES;
    ```

6. **Step 6: Remove the Old User**

    ```
        DROP USER 'olduser'@'localhost';
        FLUSH PRIVILEGES;

    ```

## 1 day | 26/07/23 - 30min
 ### Connect to wifi
 - 
    ```bash
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
    ```

  - 
      ```bash
      country=US
      ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
      update_config=1
      network={
        ssid="YOURSSID"
        scan_ssid=1
        psk="YOURPASSWORD"
        key_mgmt=WPA-PSK
      }
    ```

## 2 day | 03/08/23 - 2h
 ### Rozeznanie po posiadanych modułach, przekaźnikach i dostosowanie lapka


## 3 day | 04/06/23 - 3h [19:30-22:30] 
  ### Updated system  
  - 
    ```bash
    sudo apt update
    sudo apt upgrade
    sudo apt update
    ```
  ### Install the apache server
  - 
    ```bash
    sudo apt install apache2 -y
    ```

  ### Create yourself permissions to /var/www/html dir
  - 
    ```bash
    sudo usermod -a -G www-data pi
    sudo chown -R -f www-data:www-data /var/www/html
    ```
    
  ### Create first page
  - 
    ```bash
    sudo mkdir /var/www/html/forbot
    cd /var/www/html/forbot
    sudo nano index.html
    ```
  - 
    ```HTML
    <html>
      <head>
        <title>FORBOT</title>
      </head>
      <body>
        <p style="text-align: center; font-size: 20px">FORBOT TEST</p>
      </body>
    </html>
    ```

    Go to path in browser raspberry_id/page/index.html

  ### Install php and first page
  - 
    ```bash
    sudo apt install php -y
    ```

  - 
    ```bash
    cd /var/www/html/forbot
    sudo nano index.html
    ```
  - 
    ```php
    <?php
      echo("TEST PHP OK - " . date('Y-m-d H:i:s'));
    ?>
    ```

  *If we get error, I can try restart server apache*

  - 
    ```bash
    sudo service apache2 restart
    ```

  - #### Create php info page to preview detail information

    - 
      ```bash
      sudo nano info.php
      ```

    - 
      ```php
      <?php
        phpinfo();
      ?>
        ```

      Go to path in browser raspberry_id/page/index.php

### Install mysql
- 
  ```bash
  sudo apt install mysql-server php-mysql -y
  ```

*If we get error "E: Package 'mysql-server' has no installation candidate", we can use default-mysql-server*

- 
  ```bash
  sudo apt install default-mysql-server php-mysql -y
  ```

- #### After installing immediately reset a serve

  - 
    ```bash
    sudo service apache2 restart
    ```

- #### First login to database

  - 
    ```bash
      sudo mysql --user=root
    ```
    *(default credensials is a user as login and root as password)*

- #### Create new user to our database

  - 
    ```sql
    CREATE USER 'mysql_user'@'localhost' IDENTIFIED BY 'secret_pass';
    ```
    *(default credensials is a mysql_user as login and secret_pass as password)*
  
  - 
    ```sql
    GRANT ALL PRIVILEGES ON *.* TO 'mysql_user'@'localhost';
    ```
    *(set permissions)*

### Checkout to new user

- Press to ctrl + c to exit with mysql "client"

- 
  ```bash	
  mysql --user=mysql_user -p
  ```
  *(Login with our user login and -p flag (p is short from password, in the next step we enter our password))*

## 4 day | 05/08/23 [14:00-16:00]
  ### Radio transmitter FM with RDS
  - Mounted "cable" to pin 4 gipo 
  - #### Download the packages to handling mp3 files
    - 
      ```bas
      sudo apt-get install sox libsox-fmt-all
      sudo apt-get install libsox-fmt-mp3
      sudo apt-get install libsndfile1-dev
  - #### Clone github repository
    - 
      ```bash
      git clone https://github.com/ChristopheJacquet/PiFmRds.git
      ```
  - #### Compile program
    - 
      ```bash
      cd PiFmRds/src 
      make clean 
      make
      ```
  - #### First run program
    - 
      ```bash      	
      sudo ./pi_fm_rds -audio sound.wav
      ```
      *(We have to be in PiFmRds/src, default frequency FM is 107,9)*

  *If an default frequency is a busy we need to cheng it for other*

  - 
      ```bash
      sudo ./pi_fm_rds -audio sound.wav -freq 88.1
      ```
  - Play custom file mp3

    ```bash
    sox -t mp3 /home/pi/PiFmRds/muzyka/demo.mp3 -t wav - | sudo /home/pi/PiFmRds/src/pi_fm_rds -freq 88.1 -audio -
    ```
    *(Create file on raspberry to path /home/pi/PiFmRds/muzyka/demo.mp3)*
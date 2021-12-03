# Praktikum Modul 2

## Group 12 [IT02-01]

**Rifqi Naufal A 1202190012 || Andi Tadang P 1202190046**

<hr> 

# Keadaan

Gilang dan adit yang telah menyelesaikan virtualisasi server sesuai dengan hasil rapat mereka dengan tim programmer akan mencoba menginstall beberapa aplikasi yang akan digunakan. Setelah sukses menginstall framework codeigniter beserta php5.6 dan membuat server database dengan mariadb serta phpmyadmin. Mereka akan mencoba menginstall 2 framework lain yang dibutuhkan. Tentunya, mereka akan membuat skrip instalasi secara automasi dengan ansible. Mereka cukup jumawa karena telah lulus SAS pak Aldo dengan nilai A.

Namun mereka diberi tahu lagi oleh programmer senior, pak Dzul. dimana ternyata ubuntu 18.04 Bionic sudah tidak disupport lagi untuk php7.4. Lantas mereka akan menggantinya dengan ubuntu 20.04 focal.

# Soal Praktikum

1. Rubah LXC landing dengan ubuntu focal (destroy n create, same ip, same name)
2. Rubah LXC php7 dengan ubuntu focal (destroy n create, same ip, same name)
3. vm.local/

   - akan diinstall menggunakan framework laravel 8 pada lxc_landing

   - laravel 8 menggunakan php7.4

   - tentunya harus bisa connect ke server database (lxc_mariadb)

   - semua script instalasi tidak ada yang manual (kecuali openssh-server), harus menggunakan ansible, termasuk membuat database (sungguh mereka jumawa sekali)

4. vm.local/blog

   - install wordpress terbaru pada lxc_php7.4
   - wordpress menggunakan php7.4
   - tentunya harus bisa connect ke server database (lxc_mariadb)
   - semua script instalasi tidak ada yang manual (kecuali openssh-server), harus menggunakan ansible, termasuk membuat database (sungguh mereka jumawa sekali)
   - Bisa masuk dashboard
   - 
<hr>

## 1. Rubah LXC landing dengan ubuntu focal (destroy n create, same ip, same name)

   * Check lxc by using

   ```markdown
    lxc-ls -f
   ```
   ![image](https://user-images.githubusercontent.com/93064971/144439079-dda93157-dd5c-4dd0-adef-240191e96dd4.png)

   * Remove ubuntu landing by using the command as below, and create a new ubuntu focal with lxc-create

   ```markdown
    lxc-destroy ubuntu_landing
   ```
   
   ![image](https://user-images.githubusercontent.com/93064971/144439281-b02e7318-2f38-4717-b3f2-4bad6aaeb3b9.png)

   * Installing
   
   ```
   sudo lxc-create -n ubuntu_landing -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   ```

   ![image](https://user-images.githubusercontent.com/93064971/144441579-93170cb9-ec86-4070-a6db-fbbc886a0536.png)
   
   * After creating a new one, use command lxc-start to start ubuntu_landing and use command lxc-attach to open ubuntu_landing. Then use install nano to edit the config.

    
    lxc-start ubuntu_landing
    lxc-attach ubuntu_landing
    apt-get install nano net-tools curl
    
   
   ![image](https://user-images.githubusercontent.com/93064971/144442963-06fae96b-75e7-4e93-a884-b47c966a4b59.png)
   ![image](https://user-images.githubusercontent.com/93064971/144442988-c30d7d9d-4cc2-4b2b-8e07-bd210ed2ec38.png)

   * Set IP ubuntu_landing

    
    nano /etc/netplan/10-lxc.yaml
    
   ![image](https://user-images.githubusercontent.com/93064971/144443124-e632ed70-b236-4672-9964-550cada9f74e.png)
   ![image](https://user-images.githubusercontent.com/93064971/144443287-7f009e58-2a5c-4b2e-9554-660b1529be99.png)

    
    netplay apply
    
    
   * set autostart lxc

   ![image](https://user-images.githubusercontent.com/93064971/144443581-4ed24006-4399-4f9d-92db-b83d67d8d712.png)

   * Install SSH & set password
    
   ![image](https://user-images.githubusercontent.com/93064971/144443804-14208401-654d-483a-913f-378dd77a2d57.png)
   ![image](https://user-images.githubusercontent.com/93064971/144443843-8d905bdf-d2c5-400b-8997-3ba8dd5c91dc.png)

    
    PermitRootLogin yes
    RSAAuthentication yes
    service sshd restart
    passwd
    

   * check ssh whether it is running or not

   ![image](https://user-images.githubusercontent.com/93064971/144444059-8b38253d-bb0f-499d-ab38-55dd022f37ef.png)

## 2. Rubah LXC php7 dengan ubuntu focal (destroy n create, same ip, same name)

   * Same method as number 1
   
   ![image](https://user-images.githubusercontent.com/93064971/144444774-623def0e-c285-4bca-b59e-1d4137e6a825.png)
   ![image](https://user-images.githubusercontent.com/93064971/144444831-cc1078bc-687c-4147-b111-8a7f32da186b.png)
   
   * After creating a new one, use command lxc-start to start ubuntu_7.4 and use command lxc-attach to open ubuntu_7.4. Then use install nano to edit the config.
   
   ![image](https://user-images.githubusercontent.com/93064971/144445652-46c75539-7c1d-497d-b032-cd169fa8dac2.png)
   
   * Set IP ubuntu_php7.4

    nano /etc/netplan/10-lxc.yaml

   ![image](https://user-images.githubusercontent.com/93064971/144446241-67c303c7-b9e0-4cb6-b8f2-b10b1a577398.png)
   
    netplay apply
   
   ![image](https://user-images.githubusercontent.com/93064971/144446518-8afc1bcf-845d-4143-8193-403a5600bb83.png)
  
   * Install SSH & set password

   ![image](https://user-images.githubusercontent.com/93064971/144446826-fe2c888d-eceb-49d4-b556-00e946cac84a.png)
   ![image](https://user-images.githubusercontent.com/93064971/144446850-908e6733-d66a-4e17-8d39-bef16f2582af.png)

    PermitRootLogin yes
    RSAAuthentication yes
    service sshd restart
    passwd
    

   * check ssh whether it is running or not

   ![image](https://user-images.githubusercontent.com/93064971/144446939-93f310fc-5d7b-4593-87c8-2a2c6137a0b3.png)

## 3. vm.local/

   * The first step that must be done is to install laravel using ansible. After installing, go to ansible and create a laravel folder
   ![image](https://user-images.githubusercontent.com/93064971/144447229-751b4450-53d5-485a-a480-aec13fa0af2d.png)

   * Next, create a host for lxc which will be automated later
   ![image](https://user-images.githubusercontent.com/93064971/144447362-e251cfed-c453-4515-8c8d-80416c225f94.png)

   * Make a directory and whatever will be used to run the php folder and do the installation
   ![image](https://user-images.githubusercontent.com/93064971/144447471-8b32735f-23a8-40db-89db-49a52fea96fd.png)

       ```
    ---
    - hosts: all
      become : yes
       tasks:
          - name: install nginx nginx extras
            apt:
            pkg:
            - nginx
             - nginx-extras
           state: latest
       - name: start nginx
         service:
          name: nginx
          state: started
       - name: menginstall tools
         apt:
          pkg:
             - curl
             - software-properties-common
             - unzip
          state: latest
       - name: "Repo PHP 7.4"
            apt_repository:
            repo="ppa:ondrej/php"
        - name: "Updating the repo"
          apt: update_cache=yes
        - name: Installation PHP 7.4
         apt: name=php7.4 state=present
       - name: install php untuk laravel
         apt:
            pkg:
              - php7.4-fpm
              - php7.4-mysql
              - php7.4-mbstring
              - php7.4-xml
              - php7.4-bcmath
              - php7.4-json
               - php7.4-zip
               - php7.4-common
            state: present
               ```
   * Install nginxphp.yml

   ```
   ansible-playbook -i hosts nginxphp.yml -k
   ```
   ![image](https://user-images.githubusercontent.com/93064971/144448054-4c962e03-48d2-40c2-ad8f-a450c5334706.png)

   * When the installation is complete, create a folder installcomposer.yml
   ![image](https://user-images.githubusercontent.com/93064971/144448186-3bf316d9-abf9-4e1a-aff3-f3b321cb580d.png)

               ```
                ---
                -hosts: all
                become : yes
                tasks:
                 - name: Download and install Composer
                   shell: curl -sS https://getcomposer.org/installer | php
                   args:
                    chdir: /usr/src/
                    creates: /usr/local/bin/composer
                    warn: false
                 - name: Add Composer to global path
                   copy:
                    dest: /usr/local/bin/composer
                    group: root
                    mode: '0755'
                    owner: root
                    src: /usr/src/composer.phar
                    remote_src: yes
                 - name: Composer create project
                   become_user: root
                   composer:
                    command: create-project
                    arguments: laravel/laravel landing 
                    working_dir: /var/www/html
                    prefer_dist: yes
                   environment:
                      COMPOSER_NO_INTERACTION: "1"
                 - name: mengkopi file .env.example jadi .env
                   copy:
                    dest: /var/www/html/landing/.env.example
                    src: /var/www/html/landing/.env
                    remote_src: yes
                 - name: mengganti konfigurasi .env
                   lineinfile:
                    path: /var/www/html/landing/.env
                    regexp: "{{ item.regexp }}"
                    line: "{{ item.line }}"
                    backrefs: yes
                   loop:
                    - { regexp: '^(.*)DB_HOST(.*)$', line: 'DB_HOST=10.0.3.200' }
                    - { regexp: '^(.*)DB_DATABASE(.*)$', line: 'DB_DATABASE=landing' }
                    - { regexp: '^(.*)DB_USERNAME(.*)$', line: 'DB_USERNAME=admin' }
                    - { regexp: '^(.*)DB_PASSWORD(.*)$', line: 'DB_PASSWORD= 1' }
                    - { regexp: '^(.*)APP_URL(.*)$', line: 'APP_URL=http://vm.local' }
                    - { regexp: '^(.*)APP_NAME=(.*)$', line: 'APP_NAME=landing' }
                 - name: Composer install ke landing
                   composer:
                     command: install
                     working_dir: /var/www/html/landing
                   environment:
                     COMPOSER_NO_INTERACTION: "1"
                 - name: generate php artisan
                   args:
                    chdir: /var/www/html/landing
                   shell: php artisan key:generate
                 - name: mengganti permission storage
                   file:
                    path: /var/www/html/landing/storage
                    mode: 0777
                    recurse: yes

                 ```
                 
   * Install installcomposer.yml
   
   ```
   ansible-playbook -i hosts installcomposer.yml -k
   ```
   
   ![image](https://user-images.githubusercontent.com/93064971/144448527-5ccadb7d-346d-426f-a2ca-d9b7d28311cd.png)

  * When the installation is complete, create a folder lxc_landing.dev
  
  ![image](https://user-images.githubusercontent.com/93064971/144448907-949f8200-4598-46e0-84f0-22c9ee30d51a.png)
   
   ```
           server {
                listen 80;

                root /var/www/html/landing/public;
                index index.php index.html index.htm;
                server_name lxc_landing.dev;

                error_log /var/log/nginx/landing_error.log;
                access_log /var/log/nginx/landing_access.log;

                client_max_body_size 100M;
                location / {
                        try_files $uri $uri/ /index.php$args;
                }
                location ~\.php$ {
                        include snippets/fastcgi-php.conf;
                        fastcgi_pass unix:run/php/php7.4-fpm.sock;
                        fastcgi_param SCRIPTFILENAME $document_root$fastcgi_script_name;
                }
        }
```
 
   * Create a config.yml . file
   
   ![image](https://user-images.githubusercontent.com/93064971/144449244-605e29db-8905-407c-99b1-085674a5882c.png)
   
   ```
   ---
- hosts: all
  become : yes
  vars:
    domain: 'lxc_landing.dev'
  tasks:
   - name: stop apache2
     service:
      name: apache2
      state: stopped
      enabled: no
   - name: Write {{ domain }} to /etc/hosts
     lineinfile:
      dest: /etc/hosts
      regexp: '.*{{ domain }}$'
      line: "127.0.0.1 {{ domain }}"
      state: present
   - name: ensure nginx is at the latest version
     apt: name=nginx state=latest
   - name: start nginx
     service:
      name: nginx
      state: started
   - name: copy the nginx config file 
     copy:
      src: ~/ansible/laravel/lxc_landing.dev
      dest: /etc/nginx/sites-available/lxc_landing.dev
   - name: Symlink lxc_landing.dev
     command: ln -sfn /etc/nginx/sites-available/lxc_landing.dev /etc/nginx/sites-enabled/lxc_landing.dev
     args:
      warn: false
   - name: restart nginx
     service:
      name: nginx
      state: restarted
   - name: restart php7
     service:
      name: php7.4-fpm
      state: restarted
   - name: curl web
     command: curl -i http://lxc_landing.dev
     args:
      warn: false
```

   * Install config.yml
   
   ```
   ansible-playbook -i hosts config.yml -k
   ```
   
   ![image](https://user-images.githubusercontent.com/93064971/144449339-df62635b-401d-4116-bc62-1c3564e7d006.png)

   * Check by opening vm.local. If successful, it will look like this:
   
   ![image](https://user-images.githubusercontent.com/93064971/144449503-ab3df794-5bfc-4900-8ac1-227bc6f25f91.png)

## 4. vm.local/blog

   * the first step starts by going to the ansible folder
   
   ![image](https://user-images.githubusercontent.com/93064971/144449696-806bd0c6-b4b8-4d62-87fe-ef482bfdf5c6.png)

   * Create a host for lxc which will be automated later

   ![image](https://user-images.githubusercontent.com/93064971/144449756-c2662488-16f2-451f-8cfb-cbf8c2753016.png)

   ```
   [blog]
   ubuntu_php7.4 ansible_host=lxc_php7.dev ansible_ssh_user=root ansible_become_pass=1
   ```
   
   * Create a directory for tasks, templates and handlers in the wordpress folder. Then, go to the tasks folder to install the package

   ![image](https://user-images.githubusercontent.com/93064971/144450138-5b55821f-a5ae-4208-ab49-345be8c4b19a.png)

      ```
          ---
          - hosts: all
            vars:
              domain: 'lxc_php7.dev'
            tasks:
             - name: delete apt chache
               become: yes
               become_user: root
               become_method: su
               command: rm -vf /var/lib/apt/lists/*

             - name: install requirement
               become: yes
               become_user: root
               become_method: su
               apt: name={{ item }} state=latest update_cache=true
               with_items:
                - nginx
                - nginx-extras
                - curl
                - wget
                - php7.4
                - php7.4-fpm
                - php7.4-curl
                - php7.4-xml
                - php7.4-gd
                - php7.4-opcache
                - php7.4-mbstring
                - php7.4-zip
                - php7.4-json
                - php7.4-cli
                - php7.4-mysqlnd
                - php7.4-xmlrpc
                - php7.4-curl

             - name: wget wordpress
               shell: wget -c http://wordpress.org/latest.tar.gz

             - name: tar latest.tar.gz
               shell: tar -xvzf latest.tar.gz

             - name: copy folder wordpress
               shell: cp -R wordpress /var/www/html/blog

             - name: chmod
               become: yes
               become_user: root
               become_method: su
               command: chmod 775 -R /var/www/html/blog/

             - name: copy .wp-config.conf
               copy:
                src=~/ansible/wordpress/wp.conf
                dest=/var/www/html/blog/wp-config.php

             - name: copy wordpress.conf
               copy:
                src=~/ansible/wordpress/wordpress.conf
                dest=/etc/nginx/sites-available/{{ domain }}
               vars:
                servername: '{{ domain }}'

             - name: Symlink wordpress.conf
               command: ln -sfn /etc/nginx/sites-available/{{ domain }} /etc/nginx/sites-enabled/{{ domain }}

             - name: restart nginx
               become: yes
               become_user: root
               become_method: su
               action: service name=nginx state=restarted

             - name: Write {{ domain }} to /etc/hosts
               lineinfile:
                dest: /etc/hosts
                regexp: '.*{{ domain }}$'
                line: "127.0.0.1 {{ domain }}"
                state: present

             - name: enable module php mbstring
               command: phpenmod mbstring

             - name: restart php
               become: yes
               become_user: root
               become_method: su
               action: service name=php7.4-fpm state=restarted

             - name: restart nginx
               become: yes
               become_user: root
               become_method: su
               action: service name=nginx state=restarted
          ```

   * Kemudian, masuk ke dalam templates wp.conf yang merupakan tempat configuration pada wordpress

   ![image](https://user-images.githubusercontent.com/93064971/144450457-71912f7e-35fe-4427-a469-499bb1bd2c8f.png)

          ```
                    <?php
          /**
           * The base configuration for WordPress
           *
           * The wp-config.php creation script uses this file during the installation.
           * You don't have to use the web site, you can copy this file to "wp-config.php"
           * and fill in the values.
           *
           * This file contains the following configurations:
           *
           * * MySQL settings
           * * Secret keys
           * * Database table prefix
           * * ABSPATH
           *
           * @link https://wordpress.org/support/article/editing-wp-config-php/
           *
           * @package WordPress
           */

          define( 'WP_HOME', 'http://vm.local/blog' );
          define( 'WP_SITEURL', 'http://vm.local/blog' );

          // ** MySQL settings - You can get this info from your web host ** //
          /** The name of the database for WordPress */
          define( 'DB_NAME', 'blog' );

          /** MySQL database username */
          define( 'DB_USER', 'admin' );

          /** MySQL database password */
          define( 'DB_PASSWORD', '1' );

          /** MySQL hostname */
          define( 'DB_HOST', '10.0.3.200:3306' );

          /** Database charset to use in creating database tables. */
          define( 'DB_CHARSET', 'utf8' );

          /** The database collate type. Don't change this if in doubt. */
          define( 'DB_COLLATE', '' );

          /**#@+
           * Authentication unique keys and salts.
           *
           * Change these to different unique phrases! You can generate these using
           * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
           *
           * You can change these at any point in time to invalidate all existing cookies.
           * This will force all users to have to log in again.
           *
           * @since 2.6.0
           */
          define( 'AUTH_KEY',         'put your unique phrase here' );
          define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
          define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
          define( 'NONCE_KEY',        'put your unique phrase here' );
          define( 'AUTH_SALT',        'put your unique phrase here' );
          define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
          define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
          define( 'NONCE_SALT',       'put your unique phrase here' );

          /**#@-*/

          /**
           * WordPress database table prefix.
           *
           * You can have multiple installations in one database if you give each
           * a unique prefix. Only numbers, letters, and underscores please!
           */
          $table_prefix = 'wp_';

          /**
           * For developers: WordPress debugging mode.
           *
           * Change this to true to enable the display of notices during development.
           * It is strongly recommended that plugin and theme developers use WP_DEBUG
           * in their development environments.
           *
           * For information on other constants that can be used for debugging,
           * visit the documentation.
           *
           * @link https://wordpress.org/support/article/debugging-in-wordpress/
           */
          define( 'WP_DEBUG', false );

          /* Add any custom values between this line and the "stop editing" line. */



          /* That's all, stop editing! Happy publishing. */

          /** Absolute path to the WordPress directory. */
          if ( ! defined( 'ABSPATH' ) ) {
                  define( 'ABSPATH', __DIR__ . '/' );
          }

          /** Sets up WordPress vars and included files. */
          require_once ABSPATH . 'wp-settings.php';
          ```
       
   * Go to templates wordpress.conf

   ![image](https://user-images.githubusercontent.com/93064971/144450714-9fb77d34-c8bd-4e1f-9a7d-e21d379ddc2b.png)

  ```
            server {
               listen 80;
               listen [::]:80;

               # Log files for Debugging
               access_log /var/log/nginx/wordpress-access.log;
               error_log /var/log/nginx/wordpress-error.log;

               # Webroot Directory for Laravel project
               root /var/www/html/blog;
               index index.php index.html index.htm;

               # Your  Name
               server_name lxc_php7.dev;

               location / {
                       try_files $uri $uri/ /index.php?$query_string;
               }

               # PHP-FPM Configuration Nginx
               location ~ \.php$ {
                       try_files $uri =404;
                       fastcgi_split_path_info ^(.+\.php)(/.+)$;
                       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                       fastcgi_index index.php;
                       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                       include fastcgi_params;
               }
          }
  ```
    
   * Run ansible again to install

   ![image](https://user-images.githubusercontent.com/93064971/144450894-c6c12558-0e29-4e0e-a60d-27637d94dd5f.png)

   * Open vm.local/blog to check whether wordpress can be run or not. If it can be run, then the display will change to the following:

   ![image](https://user-images.githubusercontent.com/93064971/144450971-6091eb67-e6e6-4951-b22e-069a0c61b5fb.png)
   
   ![image](https://user-images.githubusercontent.com/93064971/144450998-77c7d177-0cb5-4551-aa7c-9c9e36da2e2d.png)
   
   ![image](https://user-images.githubusercontent.com/93064971/144451026-2621acda-e365-47c7-adbc-d1310c526990.png)
   
   ![image](https://user-images.githubusercontent.com/93064971/144451049-9d22d048-3dd4-451b-af78-ae716530e264.png)
   
   <hr>
   
   ## Soal Tambahan
   
   * First, change configuration lxc_landing.dev
   
   ![image](https://user-images.githubusercontent.com/93064971/144604820-40a22204-3d22-48bd-9c8a-0fb79cfcad4a.png)

   ![image](https://user-images.githubusercontent.com/93064971/144604591-1c2e9334-70c8-4381-9379-0fcd342b3232.png)

   * Make ansible soal2
   
   ![image](https://user-images.githubusercontent.com/93064971/144605028-b8989dbc-67f3-4eee-a3ab-41856cd35fc2.png)

   ![image](https://user-images.githubusercontent.com/93064971/144605094-2bb83de1-9815-4e05-bd64-94e04cdbeaca.png)

   ```markdown
     ---
     - hosts: all
       become : yes
       tasks:
        - name: mengganti php sock
          lineinfile:
           path: /etc/php/7.4/fpm/pool.d/www.conf
           regexp: '^(.*)listen =(.*)$'
           line: 'listen = 127.0.0.1:9001'
           backrefs: yes
        - name: copy the nginx config file 
          copy:
           src: ~/ansible/laravel/lxc_landing.dev
           dest: /etc/nginx/sites-available/lxc_landing.dev
        - name: Symlink lxc_landing.dev
          command: ln -sfn /etc/nginx/sites-available/lxc_landing.dev /etc/nginx/sites-enabled/lxc_landing.dev
          args:
           warn: false
        - name: restart nginx
          service:
           name: nginx
           state: restarted
        - name: restart php7
          service:
           name: php7.4-fpm
           state: restarted
        - name: curl web
          command: curl -i http://lxc_landing.dev
          args:
           warn: false
     © 2021 GitHub, Inc.
     Terms
     Priv
     ```
     
   * Install soall.yml
   
   ![image](https://user-images.githubusercontent.com/93064971/144605420-68b4cba8-24d1-4d09-a5aa-ed6d2e282511.png)

   * Check vm.local
   
   ![laravel1](https://user-images.githubusercontent.com/93064971/144606863-721b6f5e-f254-4875-a724-81fb1f9b846b.png)

   * In the first step, do the same as the first step in laravel. Namely change the configuration file to wordpress.conf

   ![image](https://user-images.githubusercontent.com/93064971/144607687-312713af-3931-46b1-90af-e5b44477fbd7.png)

   ![image](https://user-images.githubusercontent.com/93064971/144607571-d84e44e2-4bab-4c84-8417-55215b4a8b3b.png)

   * Make ansible soal2

   ![image](https://user-images.githubusercontent.com/93064971/144607985-3034892c-5c40-4447-84f6-b9fe6d44dacc.png)

   ```
---
- hosts: all
  become : yes
  tasks:
   - name: mengganti php sock
     lineinfile:
      path: /etc/php/7.4/fpm/pool.d/www.conf
      regexp: '^(.*)listen =(.*)$'
      line: 'listen = 127.0.0.1:9001'
      backrefs: yes
   - name: copy the nginx config file 
     copy:
      src: ~/ansible/wordpress/wordpress.conf
      dest: /etc/nginx/sites-available/lxc_php7.dev
   - name: Symlink lxc_php7.dev
     command: ln -sfn /etc/nginx/sites-available/lxc_php7.dev /etc/nginx/sites-enabled/lxc_php7.dev
     args:
      warn: false
   - name: restart nginx
     service:
      name: nginx
      state: restarted
   - name: restart php7
     service:
      name: php7.4-fpm
      state: restarted
   - name: curl web
     command: curl -i http://lxc_php7.dev
     args:
      warn: false
```

   * Install soall.yml
   
   ![image](https://user-images.githubusercontent.com/93064971/144608303-6e37cc90-2d44-428f-8fa2-193b60b6211c.png)

   * Open vm.local/blog

   ![image](https://user-images.githubusercontent.com/93064971/144608442-12924e52-81d7-4a87-a6d8-c2469a046b59.png)

<hr>

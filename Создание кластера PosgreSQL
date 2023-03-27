## Создал виртуальную машину на YandexCloud, развернул кластер postgresql. Подключимся к ВМ:
               Windows PowerShell
              (C) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

              Установите последнюю версию PowerShell для новых функций и улучшения! https://aka.ms/PSWindows

              PS C:\Users\Администратор> ssh mrr-user@158.160.51.143
              The authenticity of host '158.160.51.143 (158.160.51.143)' can't be established.
              ECDSA key fingerprint is SHA256:Oe3samJlO5BqZJDWTvOJtYYh6FBEh7FDjjc1s5NDRG4.
              Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
              Warning: Permanently added '158.160.51.143' (ECDSA) to the list of known hosts.
              Enter passphrase for key 'C:\Users\Администратор/.ssh/id_ed25519':
              Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-60-generic x86_64)

               * Documentation:  https://help.ubuntu.com
               * Management:     https://landscape.canonical.com
               * Support:        https://ubuntu.com/advantage

                System information as of Mon Mar 27 07:54:37 AM UTC 2023

                System load:  1.65185546875      Processes:             143
                Usage of /:   31.5% of 14.68GB   Users logged in:       0
                Memory usage: 5%                 IPv4 address for eth0: 10.128.0.19
                Swap usage:   0%

               * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
                 just raised the bar for easy, resilient and secure K8s cluster deployment.

                 https://ubuntu.com/engage/secure-kubernetes-at-the-edge

               * Introducing Expanded Security Maintenance for Applications.
                 Receive updates to over 25,000 software packages with your
                 Ubuntu Pro subscription. Free for personal use.

                   https://ubuntu.com/pro

              Expanded Security Maintenance for Applications is not enabled.

              37 updates can be applied immediately.
              18 of these updates are standard security updates.
              To see these additional updates run: apt list --upgradable

              Enable ESM Apps to receive additional future security updates.
              See https://ubuntu.com/esm or run: sudo pro status


              The list of available updates is more than a week old.
              To check for new updates run: sudo apt update

              Last login: Wed Mar 15 14:13:55 2023 from 37.112.160.61
              mrr-user@vm-db-pg:~$


# Запустим клиента psql и подключимся к загруженной ране демо базе данных.

          mrr-user@vm-db-pg:~$ psql -h localhost -U postgres
          Password for user postgres:
          psql (15.2 (Ubuntu 15.2-1.pgdg22.04+1))
          SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, compression: off)
          Type "help" for help.

          postgres=# \dt
          Did not find any relations.
          postgres=# \connect demo;
          SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, compression: off)
          You are now connected to database "demo" as user "postgres".
          demo=# \dt
                         List of relations
            Schema  |      Name       | Type  |  Owner
          ----------+-----------------+-------+----------
           bookings | aircrafts_data  | table | postgres
           bookings | airports_data   | table | postgres
           bookings | boarding_passes | table | postgres
           bookings | bookings        | table | postgres
           bookings | flights         | table | postgres
           bookings | seats           | table | postgres
           bookings | ticket_flights  | table | postgres
           bookings | tickets         | table | postgres
          (8 rows)

          demo=#

# Подключимся к базе с помощью десктопного pgAdmin
<image src="BD_Clinic/images/pgAdmin.png">

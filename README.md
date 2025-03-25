# northwind_postgresql
Um script SQL simples que preencherá um banco de dados com o famoso exemplo northwind, adaptado para postgres.
![image](https://github.com/user-attachments/assets/09b1132a-6851-4efc-9c03-cb7d1128fcb2)
Introdução:
Manualmente
Use o arquivo sql fornecido nortwhind.sql para preencher seu banco de dados.

# With Docker and docker compose

Pré-requisito: instalar docker e docker-compose

https://www.docker.com/get-started
https://docs.docker.com/compose/install/

1. Execute docker-compose
> docker-compose up
...
... Lots of messages...
...
Creating network "northwind_psql_db" with driver "bridge"
Creating volume "northwind_psql_db" with default driver
Creating volume "northwind_psql_pgadmin" with default driver
Creating pgadmin ... done
Creating db      ... done
2. Run psql client:
Method 1: Via the docker-compose container
Open another terminal window, and type:

> docker-compose exec db psql -U postgres -d northwind

psql (13.2 (Debian 13.2-1.pgdg100+1))
Type "help" for help.

northwind=# select * from us_states;
 state_id |      state_name      | state_abbr | state_region
----------+----------------------+------------+--------------
        1 | Alabama              | AL         | south
        2 | Alaska               | AK         | north
        ...
Alternatively, you can launch bash, then psql:

docker-compose exec db /bin/bash

# You are now inside the "db" docker container
psql -U postgres northwind
Method 2: Direct access via the port 55432
The "db" docker exposes postgres on the port 55432. If you have psql on your path, you may connect to it via:

# Run this directly from your computer (this will connect to the docker db)
psql -U postgres northwind -p 55432
3. Connect PgAdmin
Access to PgAdmin at the url: http://localhost:5050

Add a new server in PgAdmin:

General Tab:
Name = db
Connection Tab:
Host name: db
Username: postgres
Password: postgres
Then, select database "northwind".

4. Stop docker-compose
Stop the server that was launched by docker compose up via Ctrl-C, then remove the containers via:

docker-compose down
5. Files & persistence
Your modifications to the postgres database(s)will be persisted in the postgresql_data docker volume, and can be retrieved once you restart docker compose up.

If you need to delete the database data, run docker-compose down -v (the database will then be repopulated from scratch when running docker-compose up).

If you need to upload any files into your db container, just copy and paste them to the files local folder. They will be available at the /files path inside the db container.

##Si seleccionamos otra versión de Ubuntu, puede que
##tengamos que modificar el fichero para adaptarlo
FROM ubuntu:14.04
MAINTAINER Apasoft Training <aapasoft.training@gmail.com>
## Añadimos la clave PGP de PostgreSQL para verificación.
## Debería coincidir con
## https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
##Añadimos el respositorio de PostgreSQL's repository.
## Llamamos a la 9,3. Si cambiamos de version es posible
##que tengamos que modificar el Dockerfile
#RUN echo "http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main" >/etc/apt/sources.list.d/pgdg.list

#RUN echo "deb http://apt.postgresql.org/pub/repos/apt/dists/precise-pgdg/main" > /etc/apt/sources.list.d/pgdg.list
##Actualizamos los repositorios de Ubuntu PostgreSQL ##
##Debemos instalar python-software-properties
###software-properties-common y PostgreSQL 9.3
RUN apt-get update && apt-get -y -q install python-software-properties software-properties-common \
&& apt-get -y -q install postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3
##Nos cambiamos al usuario postgres, que se ha creado
##al instalar postgreSQL
USER postgres
##Creamos un usuario denominado “pguser” con password
##”secret” y creamos una base de datos llamada “pgdb”
RUN /etc/init.d/postgresql start \
&& psql --command "CREATE USER pguser WITH SUPERUSER PASSWORD 'secret';" \
&& createdb -O pguser pgdb
##Nos cambiamos a usuario ROOT
USER root
##Permitimos que se puede acceder a PostgreSQL
##desde clientes remotos
RUN echo "host all all 0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf
##PErmitimos que se pueda acceder por cualquier
##IP que tenga el contenedor
RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf
##Exponemos el Puerto de la Base de Datos
EXPOSE 5432
##Creamos un directorio en /var/run y le damos permisos
##para el usuario postgres
RUN mkdir -p /var/run/postgresql && chown -R postgres /var/run/postgresql
##Creamos los volúmenes necesarios para guardar
##el backup de la configuración, logs y bases de datos
##y poder acceder desde fuera del contenedor
VOLUME ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]
##Nos cambiamos al usuario postgres
USER postgres
##Indicamos el comando a ejecutar al crear el contenedor
##Básicamente arrancar posrtgres con la configuración
##adecuada
CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]

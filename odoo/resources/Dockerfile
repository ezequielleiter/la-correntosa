## Use odoo as parent image
ARG ODOO_VERSION=latest

FROM odoo:${ODOO_VERSION}
USER root

RUN pip3 install pip --upgrade

COPY ./odoo/resources/requirements.txt /tmp/
RUN pip install -r /tmp/requirements.txt

USER odoo

# Copiar archivos de configuración de Odoo
COPY ./odoo/config/ /etc/odoo/
COPY ./odoo/addons/ /mnt/extra-addons/

# Exponer el puerto para Odoo
EXPOSE 8069

# Comando de entrada para iniciar Odoo
CMD ["odoo"]

# Usar una imagen base de Postgres
FROM postgres:alpine

# Configurar variables de entorno
ENV POSTGRES_DB=${DB_POSTGRES:-postgres}
ENV POSTGRES_PASSWORD=${DB_PASSWORD}
ENV POSTGRES_USER=${DB_USER:-odoo}
ENV PGDATA=/var/lib/postgresql/data/pgdata

# Montar volumen para almacenar datos de Postgres
VOLUME /data/${PROJECT}:/var/lib/postgresql/data/pgdata

# Exponer el puerto para Postgres
EXPOSE 5432

# Comando de entrada para iniciar Postgres
CMD ["postgres"]
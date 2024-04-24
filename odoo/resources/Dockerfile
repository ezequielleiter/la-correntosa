# Usar una imagen base de Odoo y especificar la versión
ARG ODOO_VERSION=latest
FROM odoo:${ODOO_VERSION}

# Ejecutar como usuario root para instalar dependencias
USER root

# Actualizar pip
RUN pip3 install pip --upgrade

# Copiar y instalar requisitos adicionales para Odoo
COPY ./odoo/resources/requirements.txt /tmp/
RUN pip install -r /tmp/requirements.txt

# Cambiar a usuario odoo
USER odoo

# Configurar variables de entorno para Odoo
ENV HOST=postgres
ENV USER=${DB_USER:-odoo}
ENV PASSWORD=${DB_PASSWORD}

# Copiar archivos de configuración de Odoo y addons adicionales
COPY ./odoo/config/ /etc/odoo/
COPY ./odoo/addons/ /mnt/extra-addons/

# Exponer el puerto para Odoo
EXPOSE 8069

# Copiar la imagen base de Postgres para iniciar ambos servicios en un solo contenedor
FROM postgres:alpine as postgres_base

# Configurar variables de entorno para Postgres
ENV POSTGRES_DB=${DB_POSTGRES:-postgres}
ENV POSTGRES_PASSWORD=${DB_PASSWORD}
ENV POSTGRES_USER=${DB_USER:-odoo}
ENV PGDATA=/var/lib/postgresql/data/pgdata

# Montar volumen para Postgres
VOLUME /data/${PROJECT}:/var/lib/postgresql/data/pgdata

# Copiar datos de Postgres a la imagen de Odoo
COPY --from=postgres_base /var/lib/postgresql/data/ /var/lib/postgresql/data/

# Configurar entrada de comandos para iniciar ambos servicios
CMD service postgresql start & service odoo start

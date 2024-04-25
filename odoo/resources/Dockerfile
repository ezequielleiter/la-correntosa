## Use odoo as parent image
ARG ODOO_VERSION=latest

FROM odoo:${ODOO_VERSION}
USER root

RUN pip3 install pip --upgrade

COPY ./odoo/resources/requirements.txt /tmp/
RUN pip install -r /tmp/requirements.txt

USER odoo

ENV DB_HOST="${DB_HOST}" \
    DB_PORT="${DB_PORT}" \
    DB_USER="${DB_USER}" \
    DB_PASSWORD="${DB_PASSWORD}"
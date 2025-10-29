# Creación del repositorio GitHub

Empezamos creando un nuevo repositorio y creando la jerarquia de carpetas y de ficheros.
La jerarquia es la siguiente:

En la rama principal:
  /extra-addons
  Dockerfile
  README.md

En la rama /extra-addons:
/dummy_module
.gitkeep

En la rama /dummy_module:
__init__.py
__manifest__.py

# Rellenamos el ficjero Dockerfile:

<!--

# Imagen base Odoo 17
FROM odoo:17

# (Opcional) módulos propios
COPY ./extra-addons /mnt/extra-addons

# Puerto HTTP de Odoo
EXPOSE 8069

# Puerto por defecto de PostgreSQL
ENV PGPORT=5432

# 1) Inicializa la BD indicada en $PGDATABASE si está vacía (stop-after-init)
# 2) Después arranca el servidor normalmente
#
# NOTA: usamos $PGDATABASE para que la inicialización vaya contra esa BD
# y --db-filter la fije para evitar que Odoo “coja” otra por error.
CMD ["bash","-lc", "\
  echo '==> Checking/initializing DB $PGDATABASE' && \
  odoo -d $PGDATABASE -i base --without-demo=all \
       --db_host=$PGHOST --db_port=$PGPORT \
       --db_user=$PGUSER --db_password=$PGPASSWORD \
       --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
       --stop-after-init || true; \
  echo '==> Starting Odoo server' && \
  odoo --db_host=$PGHOST --db_port=$PGPORT \
       --db_user=$PGUSER --db_password=$PGPASSWORD \
       --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
       --db-filter=$PGDATABASE \
       --dev=all"]

-->

# Render

- Abrimos una cuenta en render con la cuenta de GitHub
- Estando en el apartado de dashboard, clicamos en la parte superior derecha en "+ new", en el panel que se despliega clicamos en "Web Service"
  - Estando dentro del panel de configuración de un nuevo "Web Service" procedemos a su configuración indicando:
    - En "git provider" seleccionamos el repositorio al que queremos que se conecte.
    - Como parametros necesarios de cambio y asegurarnos son:
      - Que en "Language" sea Docker
      - Que en "Region" sea Frankfurt (EU Central)
      - Que en "Instance Type" seleccionamos el "Free"
      - Finalizamos clicando en "Deploy web service"
- Continuamos clicando en la parte superior derecha en "+ new", en el panel que se despliega clicamos en "Postgres"
  - Estando dentro del panel de configuración de un nuevo "Postgres" procedemos a su configuración indicando:
    - Indicamos en "Name" el nombre que le queremos dar
    - Que en "Region" sea Frankfurt (EU Central)
    - Que en "Instance Type" seleccionamos el "Free"
- Continuamos configurando las variables de entorno de "Postgres" en el "Web Service"
  - Entrando en el "Web Service" que hemos creado nos dirigimos al apartado de "Environment" en el panel lateral izquierdo, en este nos dirigimos a la parte inferior donde pone "Environment Variables"
  - Entramos tambien en el "Postgres" que hemos creado nos dirigimos al apartado de "Info" en el panel lateral izquierdo, en este nos dirigimos a la parte inferior donde pone "Connections"
  - Para saber la "Key" a introducir nos dirigimos al Docker del repo y le indicamos la "Key" que pone sabiendo que esta es la que hay despues de "$".
  - Para saber el "Value" a introducir estando en el apartado de "Connections" en "Postgres" y en el panel de "Info" tendremos los "Values" que le tenemos que indicar a sus respectivas "Key" en "Environment Variables" de "Web Service"

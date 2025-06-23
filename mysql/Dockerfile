FROM mysql:latest
# Add a database
ENV MYSQL_HOST=127.0.0.1
ENV MYSQL_ROOT_PASSWORD=redhat123
ENV MYSQL_DATABASE=MYUsers
ENV MYSQL_USER=admin
ENV MYSQL_PASSWORD=admin123
EXPOSE 3306
# Add the content of the sql-scripts/ directory to your image
# All scripts in docker-entrypoint-initdb.d/ are automatically
# executed during container startup
COPY ./sql-scripts/ /docker-entrypoint-initdb.d/

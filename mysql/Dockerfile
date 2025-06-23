FROM mysql:latest
# Add a database
ENV MYSQL_DATABASE=MYUsers
ENV MYSQL_ROOT_PASSWORD=redhat123
ENV MYSQL_USER=admin
ENV MYSQL_PASSWORD=admin123
ENV MYSQL_HOST=127.0.0.1
EXPOSE 3306
# Add the content of the sql-scripts/ directory to your image
# All scripts in docker-entrypoint-initdb.d/ are automatically
# executed during container startup
COPY ./sql-scripts/ /docker-entrypoint-initdb.d/

# Set the default authentication plugin (optional, as it is already set in the official image)
CMD ["--default-authentication-plugin=mysql_native_password"]

# docker
version: '3.8'

services:
  web:
    image: nginx:latest  # Или используйте ваш образ Apache+PHP, например php:apache
    container_name: web_server
    ports:
      - "8080:80"
    volumes:
      - ./html:/var/www/html  # Каталог с вашим кодом на PHP
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro  # Файл конфигурации Nginx
      - ./logs:/var/log/nginx  # Каталог для логов запросов
    depends_on:
      - db
    networks:
      - my_network

  db:
    image: mysql:latest
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: userpass
    volumes:
      - db_data:/var/lib/mysql  # Храним данные БД на хосте
    networks:
      - my_network

volumes:
  db_data:  # Volume для хранения данных MySQL

networks:
  my_network:
    driver: bridge

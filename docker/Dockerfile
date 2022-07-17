#
# Multi Dockerfile
#
# 実行コマンド例： docker build --target aws_builder --file docker/Dockerfile .
#
#
# 使用するイメージを設定
FROM nginx as base_builder
# USER root

ENV NGINX_DIR="./docker"
ENV COPY_STATIC_DIR="./static"
ENV STATIC_DIR="/static"

RUN apt-get update
RUN apt-get -y install locales && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm

# default.confファイルを0バイトに変更
# これをやらないとうまく切替わらないため実施
RUN rm /etc/nginx/conf.d/default.conf
RUN touch /etc/nginx/conf.d/default.conf

#-----------------------------------------------------------------------------
FROM base_builder as local_builder
# ローカルの default.conf をコンテナ内の /etc/nginx/conf.d/ にコピーする
COPY $NGINX_DIR/app_nginx.conf.local /etc/nginx/conf.d/app_nginx.conf
# ローカルの uwsgi_params をコンテナ内の /etc/nginx/conf.d/ にコピーする
COPY $NGINX_DIR/uwsgi_params /etc/nginx/.
# ローカルのhtmlファイル をコンテナ内の /var/www/html/ にコピーする
#COPY ./*.html /var/www/html/
#RUN apt-get install -y vim less
#RUN service nginx start

EXPOSE 80

#-----------------------------------------------------------------------------
FROM base_builder as aws_builder
# ローカルの default.conf をコンテナ内の /etc/nginx/conf.d/ にコピーする
COPY $NGINX_DIR/app_nginx.conf /etc/nginx/conf.d/.
# ローカルの uwsgi_params をコンテナ内の /etc/nginx/conf.d/ にコピーする
COPY $NGINX_DIR/uwsgi_params /etc/nginx/.
# STATIC File COPY
WORKDIR $STATIC_DIR
COPY $COPY_STATIC_DIR $STATIC_DIR
WORKDIR /

# ローカルのhtmlファイル をコンテナ内の /var/www/html/ にコピーする
#COPY ./*.html /var/www/html/
#RUN apt-get install -y vim less
#RUN service nginx start
EXPOSE 80


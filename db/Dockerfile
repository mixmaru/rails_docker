FROM postgres

# https://qiita.com/hirosemi/items/199a3c753130c12d52f1 を参考にした
RUN localedef -i ja_JP -c -f UTF-8 -A /usr/share/locale/locale.alias ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
COPY init/*.sql /docker-entrypoint-initdb.d/

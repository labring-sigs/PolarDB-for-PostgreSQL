FROM openanolis/anolisos:8

COPY PolarDB-*.rpm /tmp/
COPY polardb-entrypoint.sh /usr/local/bin/polardb-entrypoint.sh

RUN rpm -ivh /tmp/PolarDB-*.rpm \
    && (id postgres >/dev/null 2>&1 || useradd -m postgres) \
    && POLARDB_INSTALL_DIR="$(find /u01 -maxdepth 3 -type f -path '*/bin/postgres' -print -quit | sed 's#/bin/postgres##')" \
    && test -n "$POLARDB_INSTALL_DIR" \
    && if [ "$POLARDB_INSTALL_DIR" != "/u01/polardb_pg" ]; then ln -sfn "$POLARDB_INSTALL_DIR" /u01/polardb_pg; fi \
    && mkdir -p /var/lib/polardb \
    && chown -R postgres:postgres /var/lib/polardb "$POLARDB_INSTALL_DIR" \
    && chown -h postgres:postgres /u01/polardb_pg \
    && chmod +x /usr/local/bin/polardb-entrypoint.sh \
    && rm -f /tmp/PolarDB-*.rpm

ENV PATH=/u01/polardb_pg/bin:$PATH
ENV POLARDB_PORT=5432
ENV POLARDB_DATA_ROOT=/var/lib/polardb

EXPOSE 5432

ENTRYPOINT ["/usr/local/bin/polardb-entrypoint.sh"]

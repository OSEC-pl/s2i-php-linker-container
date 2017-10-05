FROM centos/s2i-base-centos7

EXPOSE 8080

ENV PHP_VERSION=7.1 \
    FOO=bar

ENV SUMMARY="Platform for building and running OSEC Linker" \
    DESCRIPTION="$PHP_VERSION as docker container with Linker stuff"

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Apache 2.4 with PHP 7.1 for Linker" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,php,php71" \
      name="osec/php-71-linker-centos7" \
      com.redhat.component="rh-php71-linker-docker" \
      version="7.1" \
      release="1" \
      maintainer="Radoslaw Kujawa <radoslaw.kujawa@localhost>"

RUN yum -y install epel-release && \
    yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    yum-config-manager --enable remi-php71 && \
    INSTALL_PKGS="php-cli php-fpm php-pdo php-mongodb php-json php-mbstring \
                  php-opcache php-odbc freetds \
                  httpd" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    rm -rf /var/cache/yum

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./root/ /

#RUN sed -i -f /opt/app-root/etc/httpdconf.sed /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf && \
#    sed -i '/php_value session.save_path/d' /opt/rh/httpd24/root/etc/httpd/conf.d/rh-php70-php.conf && \
#    head -n151 /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf | tail -n1 | grep "AllowOverride All" || exit && \
#    echo "IncludeOptional /opt/app-root/etc/conf.d/*.conf" >> /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf && \
#    mkdir /tmp/sessions && \
#    chown -R 1001:0 /opt/app-root /tmp/sessions && \
#    chmod -R a+rwx /tmp/sessions && \
#    chmod -R ug+rwx /opt/app-root && \
#    chmod -R a+rwx /etc/opt/rh/rh-php70 && \
#    chmod -R a+rwx /opt/rh/httpd24/root/var/run/httpd

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage

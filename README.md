Shamelessly stolen from https://github.com/lingej/pnp4nagios (no longer maintained)

----

# Install
I don't have a specific list of what's actually necessary, so here's what I have in my dockerfile, and it works, so there. Engineer yourself backwards from here.

    RUN yum -y -q install epel-release net-tools less wget dnf-plugins-core && \
        yum -y -q update && \
        yum -y -q upgrade && \
        yum config-manager --enable epel && \
        yum config-manager --set-enabled powertools && \
        yum -y -q install nagios nagios-plugins nagios-plugins-all nagios-plugins-nrpe nsca nsca-client cifs-utils \
                          perl-DateTime perl-Math-Round perl-Class-Load-XS perl-Math-Round perl-Time-Piece perl-Switch \
                          perl-SOAP-Lite qt5-qtbase freetds perl-DBI perl-DBD-ODBC openldap-clients php-gd sysstat sshpass \
                          autoconf which make gcc-c++ rrdtool perl-Time-HiRes perl-rrdtool php-gd php php-cli procps \
                          openssl cronie php-xml; yum clean all
                          
    RUN cd /tmp/; \
        wget --content-disposition https://github.com/mtnielsen/pnp4nagios/archive/refs/tags/0.6.26.1.tar.gz; \
        tar zxf pnp4nagios-0.6.26.1.tar.gz; \
        cd pnp4nagios-0.6.26.1; \
        ./configure; \
        make all; \
        make fullinstall; \
        mv /usr/local/pnp4nagios/share/install.php /usr/local/pnp4nagios/share/install.php.old; \
        ln -s /usr/local/pnp4nagios/ /var/www/html/pnp4nagios
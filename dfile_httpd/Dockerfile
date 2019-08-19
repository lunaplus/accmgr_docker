FROM httpd:latest

RUN apt-get update
RUN apt-get install -y curl gnupg2 dirmngr ca-certificates procps default-libmysqlclient-dev

# =============================================================================
# setup for RVM
# =============================================================================
# RUN gpgconf --kill all
# RUN gpg2 --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
# RUN gpg2 --keyserver hkp://pgp.mit.edu --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
# RUN gpg2 --keyserver hkp://keyserver.pgp.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

RUN curl -sSL https://rvm.io/mpapis.asc | gpg --import -
RUN curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

RUN curl -sSL https://get.rvm.io | bash -s stable
RUN /bin/bash -l -c "/etc/profile.d/rvm.sh && rvm requirements"

# =============================================================================
# install rubies
# =============================================================================
RUN /bin/bash -l -c "/etc/profile.d/rvm.sh && rvm install ruby-2.4 && rvm 2.4 && rvm gemset create accmgr && rvm 2.4@accmgr && gem install mysql2"
RUN /bin/bash -l -c "/etc/profile.d/rvm.sh && rvm pkg install openssl && rvm install ruby-1.8 --with-openssl-dir=/usr/local/rvm/usr"

# =============================================================================
# setup for apache
# =============================================================================
RUN echo 'IncludeOptional conf.d/*.conf' >> /usr/local/apache2/conf/httpd.conf
RUN mkdir /usr/local/apache2/conf.d
RUN sed -i '/LoadModule rewrite_module/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule cgid_module/s/#Load/Load/g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule cgi_module/s/#Load/Load/g' /usr/local/apache2/conf/httpd.conf

# optional : logging on logfile
#RUN mkdir -p /var/log/apache2/
#RUN sed -i '/ErrorLog \/proc\/self\/fd\/2/s//ErrorLog \/var\/log\/apache2\/error.log/g' /usr/local/apache2/conf/httpd.conf
#RUN sed -i '/CustomLog \/proc\/self\/fd\/1 common/s//CustomLog \/var\/log\/apache2\/access.log common/g' /usr/local/apache2/conf/httpd.conf

EXPOSE 80


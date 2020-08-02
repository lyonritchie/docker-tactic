############################################################
# Dockerfile to run Tactic Containers 
# Based on Centos 6 image
############################################################

FROM centos:8

# Install locale not included in centos docker image
# RUN yum install glibc-langpack-en
# RUN yum update 
#&& yum clean all

# Setup a minimal env
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=
# ENV HOME=/root

# set a better shell prompt
RUN echo 'export PS1="[\u@docker] \W # "' >> /root/.bash_profile

# Install dependecies
RUN yum -y update && yum install -y glibc-all-langpacks.x86_64 glibc-locale-source glibc-langpack-en

RUN yum -y --enablerepo=extras install epel-release

RUN localedef -f UTF-8 -i en_US en_US.UTF-8

# RUN localectl set-locale LANG=en_US.UTF-8

RUN yum -y install wget yum-utils gcc openssl-devel bzip2-devel libffi-devel make

# install Python 3.7+ from https://www.workaround.cz/howto-compile-install-latest-python-37-38-39-centos-7-8/
RUN wget https://www.python.org/ftp/python/3.7.7/Python-3.7.7.tgz
RUN tar xzf Python-3.7.7.tgz && cd Python-3.7.7 && \
    ./configure \
    # --prefix=/opt/python37 \
    --enable-optimizations --with-lto \
    --with-system-ffi \
    # --with-computed-gotos \
    --enable-loadable-sqlite-extensions && make -j "$(nproc)" && \
    make install
    # rm /tmp/Python-3.8.5.tgz

# RUN ln -s /opt/python38/bin/python3.8 /opt/python38/bin/python3
# RUN ln -s /opt/python38/bin/python3.8 /usr/bin/python38
# RUN ln -s /opt/python38/bin/python3.8 /opt/python38/bin/python
# RUN ln -s /opt/python38/bin/python3.8-config /opt/python38/bin/python-config
# RUN ln -s /opt/python38/bin/pydoc3.8 /opt/python38/bin/pydoc
# RUN ln -s /opt/python38/bin/idle3.8 /opt/python38/bin/idle

# RUN ln -s /opt/python38/bin/pip3.8 /opt/python38/bin/pip3
# RUN ln -s /opt/python38/bin/pip3.8 /opt/python38/bin/pip

RUN yum -y install \
    httpd \
    postgresql postgresql-server \
    openssh-server \
    # python3 \
    unzip git ImageMagick \
    # python3-dateutil \
    gcc-c++ \
    # python3-devel \
    unixODBC-devel
RUN pip3 install psycopg2-binary pillow lxml pycryptodomex six pytz jaraco.functools requests supervisor pyodbc python-dateutil

RUN localedef -i en_US -f UTF-8 en_US.UTF-8

# add ffmpeg
RUN curl -L -O https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-amd64-static.tar.xz && \
    tar xf ffmpeg-release-amd64-static.tar.xz && \
    cp ffmpeg-*-static/ffmpeg ffmpeg-*-static/ffprobe ffmpeg-*-static/qt-faststart /usr/local/bin/ && \
    rm -rf ffmpeg-*-static*

# generate host keys for sshd server
RUN /usr/bin/ssh-keygen -A

# set root passord at image launch with -e ROOT_PASSWORD=my_secure_password
# ADD bootstrap.sh /usr/local/bin/bootstrap.sh

# Clean up
# RUN dnf clean all

# initialize postgresql data files
#RUN postgresql-setup initdb
# RUN su - postgres -c "/usr/bin/initdb -D /var/lib/pgsql/data"

# RUN useradd -ms /bin/bash tactic
RUN useradd -r -d /opt/tactic tactic
RUN mkdir -p /opt/tactic/tactic

# RUN chown -R tactic /opt/tactic

# USER tactic

# get and install Tactic
RUN git clone -b 4.7-updates --depth 1 https://github.com/Southpaw-TACTIC/TACTIC.git
# RUN python3 /opt/tactic/tactic/src/install/install.py -d
# /opt/tactic/tactic/
    #&& \
    # cp TACTIC/src/install/postgresql/pg_hba.conf /var/lib/pgsql/data/pg_hba.conf && \
    # chown postgres:postgres /var/lib/pgsql/data/pg_hba.conf && \
    # su postgres -c "postgres -p 5432 -D /var/lib/pgsql/data &" && \
    # sleep 5 && \
    # sed -i -e 's|/home/apache|/opt/tactic|g' TACTIC/src/install/install.py && \
    # yes | python3 TACTIC/src/install/install.py -d && \
    # chown -R apache:apache /opt/tactic && \
    # sed -i -e 's|<python>python</python>|<python>python3</python>|' /opt/tactic/tactic_data/config/tactic-conf.xml && \
    # cp /opt/tactic/tactic_data/config/tactic.conf /etc/httpd/conf.d/ && \
    # pkill postgres && \

# RUN rpm -Uhv "http://community.southpawtech.com/downloads/TACTIC%20-%20RPM/TACTIC-4.7.0.b02-0.x86_64.rpm"

# CMD ["python3", "/opt/tactic/tactic/src/pyasm/search/upgrade/postgresql/bootstrap_load.py"]
    # rm -rf TACTIC

### INSTALL STEPS
# 1. Check for db
# 2. TEMP directories & chown to apache user:
# os.system('chown -R %s \"%s\"'\
#     %(self.tactic_apache_user, self.tmp_dir))

# os.system('chown -R %s \"%s\"'\
#     %(self.tactic_apache_user, self.tactic_site_dir))

# os.system('chown -R %s \"%s\"'\
#     %(self.tactic_apache_user, self.tactic_data_dir))

# os.system('chown -R %s \"%s/assets\"'\
#     %(self.tactic_apache_user, self.tactic_base_dir))

# os.system('chown -R %s \"%s\"'\
#     %(self.tactic_apache_user, self.tactic_src_dir))


ENTRYPOINT ["python3", "/opt/tactic/tactic/src/bin/startup_dev.py"]

# EXPOSE 80 22

# configure supervisord
# RUN mkdir -p /var/log/supervisor && \
#     mkdir -p /etc/supervisor/conf.d/
# ADD supervisord.conf /etc/supervisor/supervisord.conf

# Start Tactic stack
# CMD ["/usr/local/bin/bootstrap.sh"]


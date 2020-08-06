FROM python:3.7.8-buster


RUN apt update && apt install -y \
    apt-utils \
    unixodbc-dev \
    locales \
    locales-all \
    postgresql

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# WORKDIR /tmp

WORKDIR /

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

RUN useradd -r -d /opt/tactic tactic
# ENV TACTIC_INSTALL_DIR=/opt/tactic

RUN git clone -b 4.8 --depth 1 https://github.com/Southpaw-TACTIC/TACTIC.git
# COPY src /TACTIC

# Specify DB name, then build...
COPY tactic_linux-conf.xml /TACTIC/src/install/template/config/tactic_linux-conf.xml

# COPY tactic_linux-conf.xml /opt/tactic/tactic_data/config/tactic-conf.xml
# Temporary fix for configs (disabling)
# COPY install.py /TACTIC/src/install/install.py

RUN mkdir -p /opt/tactic/assets

WORKDIR /TACTIC/src/install

RUN python install.py -d -i False

RUN mkdir -p /opt/tactic_temp
RUN chown tactic:tactic -R /opt/tactic*
RUN chmod 755 -R /opt/tactic*

WORKDIR /opt/tactic/tactic

# This helps get past: ModuleNotFoundError: No module named 'pyasm'
ENV PYTHONPATH="/opt/tactic/tactic/src"

USER tactic
ENTRYPOINT ["python", "src/bin/startup_dev.py"]
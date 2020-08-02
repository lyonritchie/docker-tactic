FROM python:3.7.8-buster


RUN apt update && apt install -y \
    apt-utils \
    unixodbc-dev \
    locales \
    locales-all

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

COPY tactic_linux-conf.xml /opt/tactic/tactic_data/config/tactic-conf.xml

RUN mkdir -p /home/apache/assets

WORKDIR /TACTIC/src/install

RUN python install.py -d -i False

# RUN mkdir -p /opt/tactic

WORKDIR /home/apache/tactic/src

ENTRYPOINT ["python", "bin/startup_dev.py"]
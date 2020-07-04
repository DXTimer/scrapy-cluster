FROM python:2.7
MAINTAINER Madison Bahmer <madison.bahmer@istresearch.com>

# os setup
RUN apt-get update && apt-get -y install \
  python-lxml \
  build-essential \
  libssl-dev \
  libffi-dev \
  python-dev \
  libxml2-dev \
  libxslt1-dev \
  && rm -rf /var/lib/apt/lists/*

ENV APP_HOME /usr/src/app
RUN mkdir -p ${APP_HOME}
WORKDIR ${APP_HOME}

# install requirements
COPY requirements.txt ${APP_HOME}/
RUN pip install --no-cache-dir -r requirements.txt

# move codebase over
COPY . ${APP_HOME}

# override settings via localsettings.py
COPY config/docker/settings.py ${APP_HOME}/crawling/localsettings.py

# copy testing script into container
COPY scripts/run_docker_tests.sh ${APP_HOME}/run_docker_tests.sh

# set up environment variables

# run the spider
CMD ["scrapy", "runspider", "crawling/spiders/link_spider.py"]
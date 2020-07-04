FROM python:2.7
MAINTAINER Madison Bahmer scrapy-cluster-tech@istresearch.com

# os setup
RUN apt-get update

ENV APP_HOME /usr/src/app
RUN mkdir -p ${APP_HOME}
WORKDIR ${APP_HOME}

# install requirements
COPY requirements.txt ${APP_HOME}/
RUN pip install --no-cache-dir -r requirements.txt

# move codebase over
COPY . ${APP_HOME}

# override settings via localsettings.py
COPY config/docker/settings.py ${APP_HOME}/localsettings.py

# copy testing script into container
COPY scripts/run_docker_tests.sh ${APP_HOME}/run_docker_tests.sh

# set up environment variables

# run command
CMD ["python", "kafka_monitor.py", "run"]
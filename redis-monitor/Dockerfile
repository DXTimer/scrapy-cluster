# ------------------------------------------------------------------------------
# Baseline python image
# ------------------------------------------------------------------------------
FROM python:2.7 AS base
LABEL MAINTAINER="Ivan Bakalov <ibbakalov@gmail.com>"

# os setup
RUN apt-get update

ENV APP_HOME /usr/src/app
RUN mkdir -p ${APP_HOME}
WORKDIR ${APP_HOME}

# install poetry
RUN curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python

# set up environment variables
ENV POETRY_HOME="/root/.poetry"
ENV POETRY_NO_INTERACTION=1
ENV POETRY_VIRTUALENVS_IN_PROJECT=true

ENV VENV_PATH="${APP_HOME}/.venv"
ENV PATH="$POETRY_HOME/bin:$VENV_PATH/bin:$PATH"

# ------------------------------------------------------------------------------
# Project dependencies image
# ------------------------------------------------------------------------------
FROM base as deps
WORKDIR ${APP_HOME}

COPY poetry.lock pyproject.toml ./
RUN poetry install

# ------------------------------------------------------------------------------
# Final production container
# ------------------------------------------------------------------------------
FROM deps as final
WORKDIR ${APP_HOME}

# copy in our built poetry + venv
COPY --from=deps $POETRY_HOME $POETRY_HOME
COPY --from=deps $APP_HOME $APP_HOME

# move codebase over
COPY . ${APP_HOME}

# override settings via localsettings.py
COPY config/docker/settings.py ${APP_HOME}/localsettings.py

# copy testing script into container
COPY scripts/run_docker_tests.sh ${APP_HOME}/run_docker_tests.sh

# run command
CMD ["python", "redis_monitor.py"]

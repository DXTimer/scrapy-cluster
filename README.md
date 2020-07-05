# Scrapy Cluster

[![Build Status](https://travis-ci.org/istresearch/scrapy-cluster.svg?branch=master)](https://travis-ci.org/istresearch/scrapy-cluster) [![Documentation](https://readthedocs.org/projects/scrapy-cluster/badge/?version=latest)](http://scrapy-cluster.readthedocs.io/en/latest/) [![Join the chat at https://gitter.im/istresearch/scrapy-cluster](https://badges.gitter.im/istresearch/scrapy-cluster.svg)](https://gitter.im/istresearch/scrapy-cluster?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Coverage Status](https://coveralls.io/repos/github/istresearch/scrapy-cluster/badge.svg?branch=master)](https://coveralls.io/github/istresearch/scrapy-cluster?branch=master) [![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/istresearch/scrapy-cluster/blob/master/LICENSE) [![Docker Pulls](https://img.shields.io/docker/pulls/istresearch/scrapy-cluster.svg)](https://hub.docker.com/r/istresearch/scrapy-cluster/)

This Scrapy project uses Redis and Kafka to create a distributed on demand scraping cluster.

The goal is to distribute seed URLs among many waiting spider instances, whose requests are coordinated via Redis. Any other crawls those trigger, as a result of frontier expansion or depth traversal, will also be distributed among all workers in the cluster.

The input to the system is a set of Kafka topics and the output is a set of Kafka topics. Raw HTML and assets are crawled interactively, spidered, and output to the log. For easy local development, you can also disable the Kafka portions and work with the spider entirely via Redis, although this is not recommended due to the serialization of the crawl requests.

## Dependencies

Please see the ``requirements.txt`` within each sub project for Pip package dependencies.

Other important components required to run the cluster

- Python 2.7: https://www.python.org/downloads/
- Redis: http://redis.io
- Zookeeper: https://zookeeper.apache.org
- Kafka: http://kafka.apache.org

## Core Concepts

This project tries to bring together a bunch of new concepts to Scrapy and large scale distributed crawling in general. Some bullet points include:

- The spiders are dynamic and on demand, meaning that they allow the arbitrary collection of any web page that is submitted to the scraping cluster
- Scale Scrapy instances across a single machine or multiple machines
- Coordinate and prioritize their scraping effort for desired sites
- Persist data across scraping jobs
- Execute multiple scraping jobs concurrently
- Allows for in depth access into the information about your scraping job, what is upcoming, and how the sites are ranked
- Allows you to arbitrarily add/remove/scale your scrapers from the pool without loss of data or downtime
- Utilizes Apache Kafka as a data bus for any application to interact with the scraping cluster (submit jobs, get info, stop jobs, view results)
- Allows for coordinated throttling of crawls from independent spiders on separate machines, but behind the same IP Address
- Enables completely different spiders to yield crawl requests to each other, giving flexibility to how the crawl job is tackled

## Scrapy Cluster development

Docker
======

Each service houses a `config/docker/settings.py` file that overrides default configs for use in Docker.

For more information please check out the [documentation](http://scrapy-cluster.readthedocs.org)

To provision all services for development use `docker-compose up`. Note that at present `crawler` might fail to start due to its dependency on `kafka` and it's slow start-up.

To build a service container you can follow the below example:

```
$ cd crawler
$ docker build -t crawler:1.2 .
```

## Documentation

Please check out the official [Scrapy Cluster 1.2.1 documentation](http://scrapy-cluster.readthedocs.org/en/latest/) for more information on how everything works!

## Branches

The `main` branch of this repository contains the latest stable release code for `Scrapy Cluster 1.2.1`.

The `dev` branch contains bleeding edge code and is currently working towards [Scrapy Cluster 1.3](https://github.com/istresearch/scrapy-cluster/milestone/3). Please note that not everything may be documented, finished, tested, or finalized but we are happy to help guide those who are interested.

## To-do

- [ ] Migrate crawler to poetry
- [ ] Migrate kafka-monitor to poetry
- [ ] migrate redis-monitor to poetry
- [ ] Migrate rest to poetry
- [ ] Improve the test harness by having a separate `docker-compose.test.yml` for local and CI
- [ ] Fix flaky specs
- [ ] Migrate to Python 3.7+
  - [ ] Update core dependencies for Scrapy, Twisted, Kafka and Redis
  - [ ] Add code formatter
- [ ] Helm chart (will require exploritory work)
- [ ] UI - covering basic operation and observability of the jobs
- [ ] Prometheus integration
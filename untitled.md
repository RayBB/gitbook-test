# internetarchive/openlibrary readme

[![Build Status](https://camo.githubusercontent.com/035a6b3ae1de4fc8af5b239d3900ba2caaf3500073123275b85331652704d204/68747470733a2f2f7472617669732d63692e6f72672f696e7465726e6574617263686976652f6f70656e6c6962726172792e7376673f6272616e63683d6d6173746572)](https://travis-ci.org/internetarchive/openlibrary) [![Join the chat at https://gitter.im/theopenlibrary/Lobby](https://camo.githubusercontent.com/651941e50b260d8419e94b6ced10b90981727fa67f58d1170288a8967a5252b4/68747470733a2f2f6261646765732e6769747465722e696d2f7468656f70656e6c6962726172792f4c6f6262792e737667)](https://gitter.im/theopenlibrary/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[Open Library](https://openlibrary.org/) is an open, editable library catalog, building towards a web page for every book ever published.

Are you looking to get started? [This is the guide](https://github.com/internetarchive/openlibrary/blob/master/CONTRIBUTING.md) you are looking for. You may wish to learn more about [Google Summer of Code \(GSoC\)?](https://github.com/internetarchive/openlibrary/wiki/Google-Summer-of-Code) or [Hacktoberfest](https://github.com/internetarchive/openlibrary/wiki/Hacktoberfest).

## Table of Contents

* [Overview]()
* [Installation]()
* [Code Organization]()
* [Architecture]()
  * [The Frontend](https://github.com/internetarchive/openlibrary/wiki/Frontend-Guide)
  * [The Backend]()
  * [The Service Architecture](https://github.com/internetarchive/openlibrary/wiki/Production-Service-Architecture)
* [Developer's Guide]()
* [Running Tests]()
* [Contributing](https://github.com/internetarchive/openlibrary/blob/master/CONTRIBUTING.md)
* [Public APIs](https://openlibrary.org/developers/api)
* [FAQs](https://openlibrary.org/help/faq)

## Overview

Open Library is an effort started in 2006 to create "one web page for every book ever published". It provides access to many public domain and out-of-print books, which can be read online.

Here's a quick public tour of Open Library to get your familiar with the service and its offerings \(10min\)

[![archive org\_embed\_openlibrary-tour-2020 \(1\)](https://user-images.githubusercontent.com/978325/91348906-55940d00-e799-11ea-83b9-17cd4d99642b.png)](https://archive.org/embed/openlibrary-tour-2020/openlibrary.ogv)

* [Learn more about the Open Library project](https://openlibrary.org/about)
* [The Vision \(Dream\) of OpenLibrary](https://openlibrary.org/about/vision)
* [Visit the Blog](http://blog.openlibrary.org/)

## Installation

The development environment can be set up using the [Docker Instructions](https://github.com/internetarchive/openlibrary/blob/master/docker/README.md). You can also watch the [video tutorial](https://archive.org/embed/openlibrary-developer-docs/openlibrary-docker-set-up.mp4) for a more detailed explanation.

### Developer's Guide

For instructions on administrating your Open Library instance, refer to the Developer's [Quickstart](https://github.com/internetarchive/openlibrary/wiki/Getting-Started) Guide.

You can also find more information regarding Developer Documentation for Open Library in the Open Library [Wiki](https://github.com/internetarchive/openlibrary/wiki/)

## Code Organization

* openlibrary/core - core openlibrary functionality, imported and used by www
* openlibrary/plugins - other models, controllers, and view helpers
* openlibrary/views - views for rendering web pages
* openlibrary/templates - all the templates used in the website
* openlibrary/macros - macros are like templates, but can be called from wikitext

## Architecture

### The Backend

OpenLibrary is developed on top of the Infogami wiki system, which is itself built on top of the web.py Python web framework and the Infobase database framework.

* [Overview of Backend Web Technologies](https://openlibrary.org/about/tech)

Once you've read the overview of OpenLibrary Backend technologies, it's highly encouraged you read the developer primer which explains how to use Infogami \(and its database, Infobase\)

* [Infogami Developer Tutorial](https://openlibrary.org/dev/docs/infogami)

If you want to dive into the source code for Infogami, see the [Infogami repo](https://github.com/internetarchive/infogami).

## Running tests

Open Library tests can be run using pytest. Kindly look up on our [Testing Document](https://github.com/internetarchive/openlibrary/wiki/Testing) for more details

Run tests while the docker container is running

```text
cd docker/
docker-compose exec web make test
```

### Integration Tests

Integration tests use the Splinter webdriver with Google Chrome. For instructions on installation requirements and running integration tests, [see Integration Tests README](https://github.com/internetarchive/openlibrary/blob/master/tests/integration/README.md)

## License

All source code published here is available under the terms of the [GNU Affero General Public License, version 3](https://www.gnu.org/licenses/agpl-3.0.html).


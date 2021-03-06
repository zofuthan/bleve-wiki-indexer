## bleve-wiki-indexer

[Search the bleve wiki](http://wikisearch.blevesearch.com/)

This application uses the [bleve](http://www.blevesearch.com) library to build an index of the bleve [wiki](https://github.com/blevesearch/bleve/wiki).

The application monitors the specified directory for changes, and upon finiding changes it reindexes those files.  Keeping the directory up to date is the responsibility of another application.

The application exposes a search interface on port 8099.

## Building

Building this can be a bit challenging.

The first step is get a happy installation of [git2go](https://github.com/libgit2/git2go).

        $ go get -d github.com/libgit2/git2go
        $ cd $GOPATH/src/github.com/libgit2/git2go
        $ git submodule update --init
        $ make install

Next, get the bleve-wiki-indexer.

        $ go get github.com/blevesearch/bleve-wiki-indexer

NOTE: the production version is built with `-tags full` build tag.  This enables the leveldb storage engine, and the english text analyzer.  If you have problems building bleve with that build tag, please raise issues in the [bleve project](https://github.com/blevesearch/bleve).

## Running

Acquire the source to the wiki you want to index.

        $ git clone https://github.com/blevesearch/bleve.wiki.git

Run the bleve-wiki-indexer.

        $ cd $GOPATH/src/github.com/blevesearch/bleve-wiki-indexer
        $ bleve-wiki-indexer -dir /path/to/bleve.wiki

Running from the bleve-wiki-indexer source directory enures the static content is found by default.  If you want to run it from somewhere else, use the `-static` flag to point to the static content.
This will store the index in a directory `wiki.bleve` by default, but can be changed with command-line options.

The program will run continuously, monitoring for changes in the /path/to/bleve.wiki.  If you update the live wiki, then in another terminal do a `git pull` the bleve-wiki-indexer should see the changes and begin indexing them shortly.

The program exposes a web UI at:

        http://localhost:8099/

It also exposes the standard bleve.http.SearchHandler at:

        http://localhost:8099/api/search

## Status

[![Build Status](https://travis-ci.org/blevesearch/bleve-wiki-indexer.svg?branch=master)](https://travis-ci.org/blevesearch/bleve-wiki-indexer)

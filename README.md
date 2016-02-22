
# i3fmanifest

    A service for generating iiif manifests for use with the Universal Viewer and Mirador from a Bleve index.

## Requirements

+ A Data source with Dublin Core fields such as those implemented by Islandora 7.x or EPrints
+ Golang 1.6 compiler to build the service
+ Basic understanding of JavaScript for describing specific integretions and schema
+ Bleve for providing a searchable collection of manifests


## Basic idea

The general idea is to create a service that can talk with Solr and based on the schema matched generate an appropriate iiif manifest to use with the Universal Viewer and Mirador.  Solr is being used as an integration point with Islandora's Fedora 3 repository and ArchivesSpace's Fedora 4 implementation. In principle it should be possible to integrate any system aggregated through Solr that can produce basic dublin core fields.

## Approach

+ web service written in golang
    + JavaScript implementation using otto
+ JavaScript used to describe specific mappings of Solr fields to iiif manifest fields based on work done by Tom Crane for Wellcome Library
+ Configuration of service via environment variables


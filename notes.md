

# Implementation notes

These are some notes to myself to help guide implementation.

## Reference

+ [Tom Crane's Gists](https://gist.github.com/tomcrane)
    + [WDL-IxIF.md](https://gist.github.com/tomcrane/7f86ac08d3b009c8af7c) - Helpful safe extension to handling more than images.
+ [iiif](http://iiif.io) - the definition of the iiif presentation and image protocols.
    + [Presentation API 2.0](http://iiif.io/api/presentation/2.0/)
        + [Manifest description](http://iiif.io/api/presentation/2.0/#manifest)
    + [Image API 2.0](http://iiif.io/api/image/2.0/)
+ [Universal Viewer](http://github.com/UniversalViewer/universalviewer) - A viewer that can handle tiled images and potentially much more (see Tom's gist)
+ [Mirador Viewer](https://github.com/IIIF/mirador) - A comparative tiled image viewer (demo shows it hanlding both images and YouTube video content)
+ [Solr](http://lucene.apache.org/solr)
+ [Islandora](http://islandora.ca/)
+ [ArchivesSpace](http://archivesspace.org/)
+ [OSullivan](https://github.com/IIIF/osullivan) - Ruby API for manipulating iiif manifests
    + [spiiify](https://github.com/pulibrary/spiiiffy/wiki/How-to-Generate-IIIF-Manifests-from-METS) - an example of generating manifests from METS data (uses OSullivan)
+ [Creating IIIF manifests](http://lombardpress.org/creating-iiif-manifests/) - blog post on creating the manifests
    + [WittManifestTool](https://github.com/jeffreycwitt/WittManifestTool) - also Ruby
+ [linkeddata](https://github.com/linkeddata) - MIT project
    + [gold](https://github.com/linkeddata/gold) - A linked data server implementation in Golang
    + [gojsonld](https://github.com/linkeddata/gojsonld) - A json-ld server implementation in Golang
+ [Open Knowledge Foundation Labs](http://okfnlabs.org/) - [Github repos](https://github.com/okfn)
    + [Facet View](http://okfnlabs.org/projects/facetview/) - JS front end to Elastric Search and Solr
    + [bibjson](http://okfnlabs.org/bibjson/) - Bib records in JSON from Open Knowledge Foundation
    + [bibserver](http://okfnlabs.org/projects/bibserver/) - supports bibtex, MARC, BibJSON, RDF
    + [Recline JS Viewer for FF](http://okfnlabs.org/projects/recline-mozilla-csv-viewer/)
    + [Data pipe implementation](https://github.com/okfn/datapipes)

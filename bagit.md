
# CWLProv BagIt profile

The [CWLProv folder structure](./) complies with [BagIt](https://tools.ietf.org/html/draft-kunze-bagit-16) 
so that its content and completeness can be verified with any
[BagIt tool](https://en.wikipedia.org/wiki/BagIt#Tools) or libraries.


## Overview

A rough overview of the CWLProv folder structure (the _bag_), is here explained using the [revsort-run-1 example](examples/revsort-run-1):

* `bagit.txt` - bag marker for [BagIt](https://tools.ietf.org/html/draft-kunze-bagit-16)
* `bag-info.txt` - minimal bag metadata (notably the `External-Identifier`)
* `manifest-*.txt` - checksums of files under data/ (algorithms subject to change)
* `tagmanifest-*.txt` - checksums of the remaining files (algorithms subject to change)
* `metadata/manifest.json` - [Research Object manifest](https://w3id.org/bundle/#manifest) as JSON-LD. Types and relates files within bag.
* `metadata/provenance/primary.cwlprov*` - [provenance](prov.md) traces of workflow execution
* `data/` - bag payload: workflow/step input/output data files (content-addressable)
* `data/32/327fc7aedf4f6b69a42a7c8b808dc5a7aff61376` - a data item with checksum ``327fc7aedf4f6b69a42a7c8b808dc5a7aff61376`` (checksum algorithm is subject to change)
* `workflow/packed.cwl` - The ``cwltool --pack`` standalone version of the executed workflow
* `workflow/primary-job.json` - Job input for use with packed.cwl (references ``data/*``)
* `snapshot/` - Direct copies of original files used for execution, but may have broken relative/absolute paths

It is out of scope of this document to cover the details of the 
[BagIt specification](https://tools.ietf.org/html/draft-kunze-bagit-16),
but describe the CWLProv bag constraints; the _CWLProv BagIt profile_.


## BagIt files

The base directory of a _bag_ MUST contain the marker file [bagit.txt](examples/revsort-run-1/bagit.txt) which `BagIt-Version` SHOULD be `1.0` (corresponding to [draft-kunze-bagit-16](https://tools.ietf.org/html/draft-kunze-bagit-16#section-2.1.1)). `Tag-File-Character-Encoding` MUST be `UTF-8` in CWLProv.

In CWLProv the metadata file [bag-info.txt](examples/revsort-run-1/bag-info.txt) MUST be present and MUST contain an `External-Identifier` header. The headers `Bagging-Date` and `Bag-Software-Agent` SHOULD be present.


## Payload

In CWLProv the [payload](https://tools.ietf.org/html/draft-kunze-bagit-16#section-2.1.2) directory `data/` SHOULD only contain data files or structured that have been used in the workflow execution (e.g. input and output files). Other files, such as provenance traces or workflow definitions SHOULD be stored as [tag files](https://tools.ietf.org/html/draft-kunze-bagit-16#section-2.2.4) in other directories.

All checksums of `data/` files MUST be included in every [manifest file](https://tools.ietf.org/html/draft-kunze-bagit-16#section-2.1.3), e.g. 
[manifest-sha1.txt](examples/revsort-run-1/manifest-sha1.txt). CWLProv bags SHOULD include the manifest as `sha1` and `sha512`.

In CWLProv the payload files SHOULD have file paths derived from their own hashcode (content-addressable), however CWLProv consumers MUST NOT assume this, as implementations MAY use other unique filenames like UUIDs. Reasonable subdirectory structures SHOULD be used to avoid a single directory with a large amount of files. For instance [data/97/](https://github.com/common-workflow-language/cwlprov/tree/master/examples/revsort-run-1/data/97) contains [97fe1b50b4582cebc7d853796ebd62e3e163aa3f](examples/revsort-run-1/data/97/97fe1b50b4582cebc7d853796ebd62e3e163aa3f) which happens to have the SHA1 checksum `97fe1b50b4582cebc7d853796ebd62e3e163aa3f`.


## Tag files

All files outside `data/` (except `bagit.txt` and `manifest*txt`) SHOULD be listed in a corresponding [tag manifest](https://tools.ietf.org/html/draft-kunze-bagit-16#section-2.2.1) file, e.g. [tagmanifest-sha1.txt](examples/revsort-run-1/tagmanifest-sha1.txt). CWLProv bags SHOULD include the tag manifest as `sha1` and `sha512`.

CWLProv bags SHOULD include an system-independent runnable version of the executed workflow under `workflow/`.

CWLProv SHOULD contain a [CWL workflow](https://www.commonwl.org/v1.0/Workflow.html) in `workflow/packed.cwl` - which SHOULD avoid any includes or references external to the CWLProv bag folder structure. This file may be created with `cwltool --pack` or through other means. 
 
CWLProv bags MAY include direct copies of arbitrarily named workflow files used at execution times under `snapshot/`. Consumers SHOULD NOT assume these files are CWL workflows (unless so declared in the RO manifest, see below). Consumers SHOULD NOT assume that snapshot files have valid relative paths in their internal cross-referencing.

CWLPROV bags MUST include provenance traces of the workflow run under `metadata/provenance`, of which the file `primary.cwlprov.provn` MUST be present in [PROV-N](http://www.w3.org/TR/2013/REC-prov-n-20130430/) format, describing the top level workflow execution according to the [CWLProv PROV profile](prov.md). Other provenance files and formats MAY be present, in which case they SHOULD have `conformsTo` declared in the [RO manifest](ro.md).


## External Identifier

CWLProv is reusing Linked Data standards like [JSON-LD](https://json-ld.org/), [W3C PROV](https://www.w3.org/TR/prov-primer/) and [Research Object](http://www.researchobject.org/specifications/).

A challenge with Linked Data in distributed and desktop computing is how to make identifiers that are absolute URIs (and hence globally unique); e.g. for CWLProv a workflow may be executed by an engine that do not know where its workflow provenance will be stored, published or integrated in the end. 

To this end CWLProv generators SHOULD use the proposed [arcp](https://tools.ietf.org/id/draft-soilandreyes-arcp-03.html) URI scheme to map local file paths within the RO BagIt folder structure to absolute URIs for use within the [RO manifest](ro.md) and [PROV](prov.md) traces.

In this example the root URI for the bag is `arcp://uuid,4cca2dd8-3bd5-45cb-8d34-e7f346be027e/`, as declared in [bag-info.txt](examples/revsort-run-1/bag-info.txt#L5) with `External-Identifier`. 

Consumers of CWLProv bags that do not contain an arcp-based `External-Identifier` SHOULD generate a [temporary arcp base](https://tools.ietf.org/id/draft-soilandreyes-arcp-03.html#rfc.appendix.A.1) to safely resolve any relative URI references without climbing outside the CWLProv folder.

Implementations processing a CWLProv RO MAY convert arcp URIs to their local `file:///` or `http://` URIs depending on how and where the CWLProv bag was saved, for instance using the [arcp.py](http://arcp.readthedocs.io/) library.
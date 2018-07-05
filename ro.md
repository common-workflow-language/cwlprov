# CWLProv folder structure

The [CWLProv](./) folder structure is a [Research Object](http://www.researchobject.org/)
that conforms to the [RO BagIt profile](https://w3id.org/ro/bagit)
and contains [PROV](https://www.w3.org/TR/prov-overview/)
traces detailing the execution of the workflow and its steps.

The folder structure complies with [BagIt](https://tools.ietf.org/html/draft-kunze-bagit-16) 
so that its content and completeness can be verified with any 
[BagIt tool](https://en.wikipedia.org/wiki/BagIt#Tools) or libraries.

A rough overview of the CWLProv folder structure, here explained using the [revsort-run-1 example](examples/revsort-run-1):

* `bagit.txt` - bag marker for [BagIt](https://tools.ietf.org/html/draft-kunze-bagit-16)
* `bag-info.txt` - minimal bag metadata. ``The External-Identifier`` key shows which [arcp](https://tools.ietf.org/id/draft-soilandreyes-arcp-03.html) can be used as base URI within the folder bag.
* `manifest-*.txt` - checksums of files under data/ (algorithms subject to change)
* `tagmanifest-*.txt` - checksums of the remaining files (algorithms subject to change)
* `metadata/manifest.json` - [Research Object manifest](https://w3id.org/bundle/#manifest) as JSON-LD. Types and relates files within bag.
* `metadata/provenance/primary.cwlprov*` -  [PROV](https://www.w3.org/TR/prov-overview/) trace of main workflow execution in alternative PROV and RDF formats
* `data/` - bag payload, workflow/step input/output data files (content-addressable)
* `data/32/327fc7aedf4f6b69a42a7c8b808dc5a7aff61376` - a data item with checksum ``327fc7aedf4f6b69a42a7c8b808dc5a7aff61376`` (checksum algorithm is subject to change)
* `workflow/packed.cwl` - The ``cwltool --pack`` standalone version of the executed workflow
* `workflow/primary-job.json` - Job input for use with packed.cwl (references ``data/*``)
* `snapshot/` - Direct copies of original files used for execution, but may have broken relative/absolute paths


See the [CWLProv whitepaper](https://doi.org/10.5281/zenodo.1208477) for more background.

## Research Object manifest

The file [metadata/manifest.json](examples/revsort-run-1/metadata/manifest.json) follows the structure defined for [Research Object Bundles](https://w3id.org/bundle/#manifest), but 
note that `.ro/` is instead called `metadata/` as CWLProv conforms to the derived [RO BagIt profile](https://w3id.org/ro/bagit).

Some of the keys of the CWLProv manifest are explained below:

```jsonld
    "@context": [
        {
            "@base": "arcp://uuid,67f38794-d24a-435f-bd4a-0242a56a581b/metadata/"
        },
        "https://w3id.org/bundle/context"
    ]
```    

This [JSON-LD context](https://json-ld.org/) enables consumers to alternatively consume the JSON file as Linked Data with absolute identifiers. 
The key for that is the `@base` which means URIs within this JSON file are relative to the `metadata/` folder 
within this Research Object bag, and the external JSON-LD.

Output from the [cwltool --provenance]() reference implementation should follow the JSON structure shown beyond; however interested consumers may alternatively parse it as JSON-LD with a RDF triple store like [Apache Jena](https://jena.apache.org/download/) for further querying or integration.

The manifest lists which software version created the Research Object - we will hear more from this UUID later:

```jsonld
    "createdBy": {
        "uri": "urn:uuid:7c9d9e88-666b-4977-85f4-c02da08a942d",
        "name": "cwltool 1.0.20180416145054"
    }
```

Secondly the manifest lists the person who "authored the run" - that is put the workflow and inputs together with cwltool

```jsonld
    "authoredBy": {
        "orcid": "https://orcid.org/0000-0002-1825-0097",
        "name": "Stian Soiland-Reyes"
    }
```    

Note that the author of the workflow run may differ from the author of the workflow definition.

The list of aggregates are the main resources that this Research Object transports:

```jsonld
    "aggregates": [
        {
            "uri": "urn:hash::sha1:53870991af88a6d678cbeed3255bb65993c52925",
            ...
        }, 
        { "provenance/primary.cwlprov.xml",
           ...
        },
        {
            "uri": "../workflow/packed.cwl",
            "createdBy": {
                "uri": "urn:uuid:7c9d9e88-666b-4977-85f4-c02da08a942d",
                "name": "cwltool 1.0.20180416145054"
            },
            "conformsTo": "https://w3id.org/cwl/",
            "mediatype": "text/x+yaml; charset=\"UTF-8\"",
            "createdOn": "2018-04-16T18:27:09.513824"
        },
        {
            "uri": "../snapshot/hello-workflow.cwl",
            "conformsTo": "https://w3id.org/cwl/",
            "mediatype": "text/x+yaml; charset=\"UTF-8\"",
            "createdOn": "2018-04-04T13:29:55.717707"
        }
```


Beyond being a listing of file names and identifiers, this also lists formats and light-weight provenance. We note that the
CWL file is marked to conform to the https://w3id.org/cwl/ CWL specification.

Some of the files like `packed.cwl` have been created by cwltool as part of the run, while others have been created before the run "outside".
(Note that `cwltool` is currently unable to extract the original authors and contributors of the original files, this is planned for future versions).

Under `annotations` we see that the main point of this whole research object (`/` aka `arcp://uuid,67f38794-d24a-435f-bd4a-0242a56a581b/`) 
is to describe something called `urn:uuid:67f38794-d24a-435f-bd4a-0242a56a581b`:

```jsonld
    "annotations": [
        {       
            "about": "urn:uuid:67f38794-d24a-435f-bd4a-0242a56a581b",
            "content": "/",
            "oa:motivatedBy": {
                "@id": "oa:describing"
            }
        },
```

We will later see that this is the UUID for the workflow run. A workflow run is an *activity*, 
something that happens - it can't be directly saved to a file. However it can be *described* in 
different ways, in this case as CWLProv provenance:


```jsonld
           {
            "about": "urn:uuid:67f38794-d24a-435f-bd4a-0242a56a581b",
            "content": [
                "provenance/primary.cwlprov.xml",
                "provenance/primary.cwlprov.nt",
                "provenance/primary.cwlprov.ttl",
                "provenance/primary.cwlprov.provn",
                "provenance/primary.cwlprov.jsonld",
                "provenance/primary.cwlprov.json"
            ],
            "oa:motivatedBy": {
                "@id": "http://www.w3.org/ns/prov#has_provenance"
            }
```            

Finally the research object wants to highlight the workflow file:

```jsonld
        {
            "about": "workflow/packed.cwl",
            "oa:motivatedBy": {
                "@id": "oa:highlighting"
            }
        },
```


And links the run ID `67f38794..` to the `primary-job.json` and `packed.cwl`:

```jsonld
        {
            "about": "urn:uuid:67f38794-d24a-435f-bd4a-0242a56a581b",
            "content": [
                "workflow/packed.cwl",
                "workflow/primary-job.json"
            ],
            "oa:motivatedBy": {
                "@id": "oa:linking"
            }
        }
```

Note: The `oa:motivatedBy` terms used in CWLProv are subject to change.

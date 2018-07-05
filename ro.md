# CWLProv Research Object profile

The [CWLProv](./) folder structure is a [Research Object](http://www.researchobject.org/)
that conforms to the [RO BagIt profile](https://w3id.org/ro/bagit)
and contains [PROV](https://www.w3.org/TR/prov-overview/)
traces detailing the execution of the workflow and its steps.


A relevant parts of the CWLProv folder structure is here explained using the [revsort-run-1 example](examples/revsort-run-1):

* `bag-info.txt` - minimal bag metadata (notably the `External-Identifier`)
* `manifest-*.txt` - checksums of files under data/ (algorithms subject to change)
* `metadata/manifest.json` - [Research Object manifest](https://w3id.org/bundle/#manifest) as JSON-LD. Types and relates files within bag.


See the [CWLProv BagIt profile](bagit.md) for details on the BagIt structures and suggested file paths.

This document defines what elements should be present in the Research Object manifest; forming the _CWLProv Research Object profile_.


## Research Object manifest

While the [BagIt manifests](bagit.md) provides checksums of CWLProv files, they cannot include any additional information, such as file type, provenance, attribution or relations.  To this end CWLProv uses the [Research Object specifications](http://www.researchobject.org/specifications/), which reuse existing Linked Data standards like 
[OAI-ORE](https://www.openarchives.org/ore/), [JSON-LD](https://json-ld.org/), [Web Annotation Model](https://www.w3.org/TR/annotation-model/), [PROV](https://www.w3.org/TR/prov-overview/) and [PAV](http://purl.org/pav/html).

While advanced users may facilitate these underlying standards and their corresponding tooling, CWLProv is intended to be generated or consumed without any deep knowledge of their working.


The file [metadata/manifest.json](examples/revsort-run-1/metadata/manifest.json) follows the structure defined for [Research Object Bundles](https://w3id.org/bundle/#manifest). Note that `.ro/` is instead called `metadata/` as CWLProv conforms to the derived [RO BagIt profile](https://w3id.org/ro/bagit) for storing a Research Object using [BagIt](bagit.md).

The `metadata/manifest.json` file SHOULD follow the JSON structure defined here, and MUST be valid [JSON-LD](https://json-ld.org/), e.g. escaping space in file name URIs as `%20`.

Consumers of CWLProv MAY parse the RO manifest as pure JSON, alternatively as JSON-LD using tools like [Apache Jena](https://jena.apache.org/) for querying or integration.

The expected keys of the CWLProv manifest are explained below.

### Context 

The `@context` SHOULD be of the form:

```jsonld
    "@context": [
        {
            "@base": "arcp://uuid,4cca2dd8-3bd5-45cb-8d34-e7f346be027e/metadata/"
        },
        "https://w3id.org/bundle/context"
    ]
```    

This [JSON-LD context](https://json-ld.org/) enables consumers to alternatively consume the JSON file as Linked Data with absolute identifiers, and provides mapping to namespaces of the reused standards. 

The `@base` value SHOULD be based on the [arcp External-Identifier](bagit.md#External Identifier) in the `bag-info.txt`, and SHOULD be an absolute URI for the `/metadata/` folder within this bag (where this manifest is stored).


## Conforming

CWLProv research objects MUST declare `conformsTo` to indicate their conformance with this document. The value SHOULD match a published [CWLProv permalink](./#Versions).

```jsonld
  "conformsTo": "https://w3id.org/cwl/prov/0.3.0",
```


### Creator

The manifest SHOULD lists which software version created the Research Object under `createdBy`:

```jsonld
    "createdBy": {
        "uri": "urn:uuid:7c9d9e88-666b-4977-85f4-c02da08a942d",
        "name": "cwltool 1.0.20180416145054"
    }
```

Note that the `uri` here constitutes a particular execution on a particular machine. This identifier SHOULD be a UUID, but it MAY be `http` or `https` based to indicate a particular web portal installation.

### Author

The manifest SHOULD list the person who "authored the run" - e.g. who requested `cwltool` to execute the workflow with the given inputs. In a portal environment this will be the logged in user who clicked the _Run_ button.

```jsonld
    "authoredBy": {
        "orcid": "https://orcid.org/0000-0002-1825-0097",
        "name": "Stian Soiland-Reyes"
    }
```    

The author SHOULD be identified at `orcid` using [ORCID identifiers](https://orcid.org/) starting with `https://orcid.org/`. The `uri` field MAY be included, e.g. `http://portal.example.com/user/2`.

Engines SHOULD use the value of the `ORCID` environment variable if provided, ensuring the ORCID identifier format is [valid](https://support.orcid.org/knowledgebase/articles/116780-structure-of-the-orcid-identifier).

Note that the author of the _workflow run_ may differ from the author of the _workflow definition_, which can instead be indicated under aggregates.


### Aggregates

The list of `aggregates` are the main resources that this Research Object transports.

**FIXME: Rewrite this section to recommendation language**

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

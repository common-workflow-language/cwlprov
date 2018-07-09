
# CWLProv PROV profile

CWLProv uses [PROV](https://www.w3.org/TR/prov-overview/) to describe provenance traces
from workflow executions. Such PROV files SHOULD be stored under the `metadata/provenance/` folder.

The underlying model and information of the `
files under `metadata/provenance` is the same, but is made available in multiple 
serialization formats:

* primary.cwlprov.provn -- [PROV-N](https://www.w3.org/TR/prov-n/) Textual Provenance Notation 
* primary.cwlprov.xml -- [PROV-XML](https://www.w3.org/TR/prov-xml/)
* primary.cwlprov.json -- [PROV-JSON](https://www.w3.org/Submission/prov-json/)
* primary.cwlprov.jsonld -- [PROV-O](https://www.w3.org/TR/prov-o/) as [JSON-LD](https://json-ld.org) (`@context` subject to change)
* primary.cwlprov.ttl -- [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF Turtle](https://www.w3.org/TR/turtle/)
* primary.cwlprov.nt -- [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF N-Triples](https://www.w3.org/TR/n-triples/)

See the [BagIt profile](bagit.md) for details on the CWLProv folder structure, and the [Research Object profile](ro.md) on how to declare the typing of the PROV files.

CWLProv bags MUST include `primary.cwlprov.provn` conforming to this document, but MAY include other PROV formats. Conventionally provenance files that follow the PROV structures of this documents MAY be named `*.cwlprov.*`; formally this SHOULD be declared using `conformsTo` in the [RO manifest](ro.md) set to the [permalink](./#Versions), as well as their PROV format:

```jsonld
        {
            "uri": "provenance/primary.cwlprov.provn",
            "conformsTo": [
                "http://www.w3.org/TR/2013/REC-prov-n-20130430/",
                "https://w3id.org/cwl/prov/0.3.0"
            ],
            "mediatype": "text/provenance-notation; charset=\"UTF-8\""
        },
```

Other provenance files (e.g. nested workflows, tool logs) can be included under `metadata/provenance/` using arbitrary filenames, for instance `wf-af755b11-0537-4321-bf82-eadf4f6792ef.cwlprov.provn`.

The below extracts use the PROV-N syntax for brevity.

**TODO: Rewrite below to spec-style language**

## CWLPROV namespaces


Note that the identifiers must be expanded with the defined `prefix`-es when comparing across serializations.
These set which vocabularies ("namespaces") are used by the CWLProv statements:

```provn
    prefix data <urn:hash:sha1:>
    prefix input <arcp://uuid,0e6cb79e-fe70-4807-888c-3a61b9bf232a/workflow/primary-job.json#>
    prefix cwlprov <https://w3id.org/cwl/prov#>
    prefix wfprov <http://purl.org/wf4ever/wfprov#>
    prefix sha256 <nih:sha-256;>
    prefix schema <http://schema.org/>
    prefix wfdesc <http://purl.org/wf4ever/wfdesc#>
    prefix orcid <https://orcid.org/>
    prefix researchobject <arcp://uuid,0e6cb79e-fe70-4807-888c-3a61b9bf232a/>
    prefix id <urn:uuid:>
    prefix wf <arcp://uuid,0e6cb79e-fe70-4807-888c-3a61b9bf232a/workflow/packed.cwl#>
    prefix foaf <http://xmlns.com/foaf/0.1/>
```

The PROV trace SHOULD use the [arcp](https://tools.ietf.org/id/draft-soilandreyes-arcp-03.html) base URIs corresponding to the [Research Object @base](ro.md)
or [BagIt External-Identifier](bagit.md#External-Identifier).

The arcp base URI SHOULD be based on the UUID of the master workflow run activity identifier, if present.


## Account who launched cwltool

If `cwltool --enable-user-provenance` was used, the local machine acccount (e.g. Windows or UNIX user name) who executed that command  can be tracked:


    agent(id:855c6823-bbe7-48a5-be37-b0f07f20c495, [foaf:accountName="stain", prov:type='foaf:OnlineAccount', prov:label="stain"])

It is assumed that the account was under the control of the named person (in PROV terms "actedOnBehalfOf"):

```provn
    agent(id:433df002-2584-462a-80b0-cf90b97e6e07, [prov:label="Stian Soiland-Reyes", 
          prov:type='prov:Person', foaf:account='id:8815e39c-9711-4105-bf52-dbc016c8028f'])
    actedOnBehalfOf(id:8815e39c-9711-4105-bf52-dbc016c8028f, id:433df002-2584-462a-80b0-cf90b97e6e07, -)
```

However we do not have an identifier for neither the account or the person, so every `cwltool` run will yield new UUIDs. 

With `--enable-user-provenance` it is possible to associate the account with a hostname:

    agent(id:855c6823-bbe7-48a5-be37-b0f07f20c495, [cwlprov:hostname="biggie", prov:type='foaf:OnlineAccount', prov:location="biggie"])

Note that the hostname is often non-global or variable (e.g. on cloud instances or virtual machines), 
and thus may be unreliable when considering `cwltool` executions on multiple hosts.

If the `--orcid` parameter or `ORCID` shell variable is included, then the person associated 
with the local machine account is uniquely identified, no matter where the workflow was executed:

    agent(orcid:0000-0002-1825-0097, [prov:type='prov:Person', prov:label="Stian Soiland-Reyes", 
       foaf:account='id:855c6823-bbe7-48a5-be37-b0f07f20c495'])

    actedOnBehalfOf(id:855c6823-bbe7-48a5-be37-b0f07f20c495', orcid:0000-0002-1825-0097, -)

The running of `cwltool` itself makes it the workflow engine. It is the machine account who launched the cwltool (not necessarily the person behind it):

    agent(id:7c9d9e88-666b-4977-85f4-c02da08a942d, [prov:type='prov:SoftwareAgent', prov:type='wfprov:WorkflowEngine', prov:label="cwltool 1.0.20180416145054"])
    wasStartedBy(id:855c6823-bbe7-48a5-be37-b0f07f20c495, -, id:9c3d4d1f-473d-468f-a6f2-1ef4de571a7f, 2018-04-16T18:27:09.428090)


## Starting a workflow

The main job of the cwltool execution is to run a workflow, here the activity for `workflow/packed.cwl#main`:

    activity(id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.428165, -, [prov:type='wfprov:WorkflowRun', prov:label="Run of workflow/packed.cwl#main"])
    wasStartedBy(id:67f38794-d24a-435f-bd4a-0242a56a581b, -, id:7c9d9e88-666b-4977-85f4-c02da08a942d, 2018-04-16T18:27:09.428285)

Now what is that workflow again? Well a tiny bit of prospective provenance is included:

    entity(wf:main, [prov:type='prov:Plan', prov:type='wfdesc:Workflow', prov:label="Prospective provenance"])
    entity(wf:main, [prov:label="Prospective provenance", wfdesc:hasSubProcess='wf:main/step0'])
    entity(wf:main/step0, [prov:type='wfdesc:Process', prov:type='prov:Plan'])

But we can also expand the `wf` identifiers to find that we are talking about 
`arcp://uuid,0e6cb79e-fe70-4807-888c-3a61b9bf232a/workflow/packed.cwl#` - that is 
the `main` workflow in the file `workflow/packed.cwl` of the Research Object.


## Running workflow steps

A workflow will contain some steps, each execution of these are again nested activities:

    activity(id:6c7c04ea-dcc8-40d2-92a4-7705f7286756, -, -, [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main"])
    wasStartedBy(id:6c7c04ea-dcc8-40d2-92a4-7705f7286756, -, id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.430883)
    activity(id:a583b025-9a16-49ce-8515-f3249eb2aacf, -, -, [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main/step0"])
    wasAssociatedWith(id:a583b025-9a16-49ce-8515-f3249eb2aacf, -, wf:main/step0)

Again we see the link back to the workflow plan, the workflow execution of `#main/step0` in this case. 
Note that depending on scattering etc there might 
be multiple activities for a single step in the workflow definition. 

## Data inputs (usage)


This activities uses some data at the input `message`:

    activity(id:a583b025-9a16-49ce-8515-f3249eb2aacf, -, -, [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main/step0"])
    used(id:a583b025-9a16-49ce-8515-f3249eb2aacf, data:53870991af88a6d678cbeed3255bb65993c52925, 2018-04-16T18:27:09.433743, [prov:role='wf:main/step0/message'])


Data files within a workflow execution are identified using `urn:hash:sha1:` URIs derived from their sha1 checksum (checksum algorithm and prefix subject to change):

    entity(data:53870991af88a6d678cbeed3255bb65993c52925, [prov:type='wfprov:Artifact', prov:value="Hei7"])

Small values (typically those provided on the command line may be present as `prov:value`. The corresponding 
`data/` file within the Research Object has a content-addressable filename based on the checksum; but it is also 
possible to look up this independent from the corresponding `metadata/manifest.json` aggregation:

```jsonld
    "aggregates": [
        {
            "uri": "urn:hash:sha1:53870991af88a6d678cbeed3255bb65993c52925",
            "bundledAs": {
                "uri": "arcp://uuid,0e6cb79e-fe70-4807-888c-3a61b9bf232a/data/53/53870991af88a6d678cbeed3255bb65993c52925",
                "folder": "/data/53/",
                "filename": "53870991af88a6d678cbeed3255bb65993c52925"
            }
        },
```        

## Data outputs (generation)

Similarly a step typically generates some data, here `response`:

    activity(id:a583b025-9a16-49ce-8515-f3249eb2aacf, -, -, [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main/step0"])
    wasGeneratedBy(data:53870991af88a6d678cbeed3255bb65993c52925, id:a583b025-9a16-49ce-8515-f3249eb2aacf, 2018-04-16T18:27:09.438236, [prov:role='wf:main/step0/response'])
 
In the hello world example this is interesting because it is the same data output as-is, but typically the outputs will each have different checksums (and thus different identifiers).

The step is ended:

    wasEndedBy(id:a583b025-9a16-49ce-8515-f3249eb2aacf, -, id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.438482)


In this case the step output is also a workflow output `response`, so the data is also generated by the workflow activity:

    activity(id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.428165, -, [prov:type='wfprov:WorkflowRun', prov:label="Run of workflow/packed.cwl#main"])  
    wasGeneratedBy(data:53870991af88a6d678cbeed3255bb65993c52925, id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.439323, [prov:role='wf:main/response'])

## Ending the workflow

 
Finally the overall workflow `#main` also ends:

    activity(id:67f38794-d24a-435f-bd4a-0242a56a581b, 2018-04-16T18:27:09.428165, -, [prov:type='wfprov:WorkflowRun', prov:label="Run of workflow/packed.cwl#main"])
    agent(id:7c9d9e88-666b-4977-85f4-c02da08a942d, [prov:type='prov:SoftwareAgent', prov:type='wfprov:WorkflowEngine', prov:label="cwltool 1.0.20180416145054"])
    wasEndedBy(id:67f38794-d24a-435f-bd4a-0242a56a581b, -, id:7c9d9e88-666b-4977-85f4-c02da08a942d, 2018-04-16T18:27:09.445785)

Note that the end of the outer `cwltool` activity is not recorded, as cwltool is still running at the point of writing out this provenance.

Currently the provenance trace do not distinguish executions within nested workflows; it is planned that these will be tracked in separate files under `metadata/provenance/`.



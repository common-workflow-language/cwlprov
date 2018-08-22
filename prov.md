
# CWLProv PROV profile

CWLProv uses [PROV](https://www.w3.org/TR/prov-overview/) to describe provenance traces
from workflow executions. Such PROV files SHOULD be stored under the `metadata/provenance/` folder.

See the [BagIt profile](bagit.md) for details on the CWLProv folder structure, and the 
[Research Object profile](ro.md) on how to declare the typing of the PROV files.

A provenance trace MAY be present in multiple serialization format as detailed below, 
in which case their filenames SHOULD have the same basename (ignoring extensions), 
and their content SHOULD have corresponding structures and MUST use the same identifiers 
for workflow activities and data entities.

## Formats

Provenance files for the top workflow run MUST be present with a file name starting with `primary`.
CWLProv bags MUST at include `primary.cwlprov.provn` conforming to this document, but 
MAY include other PROV formats. 

Conventionally provenance files that follow the PROV 
structures of this documents MAY be named `*.cwlprov.*`; formally this SHOULD be
declared using `conformsTo` in the [RO manifest](ro.md) set to the
PROV format as well as the [CWLProv permalink](./#Versions):

```jsonld
        {
            "uri": "provenance/primary.cwlprov.provn",
            "conformsTo": [
                "http://www.w3.org/TR/2013/REC-prov-n-20130430/",
                "https://w3id.org/cwl/prov/0.4.0"
        },
```

It is RECOMMENDED to use the following `conformsTo` and `mediatype` values for their corresponding formats:

| Format | conformsTo | mediatype | extension |
| ------ | ---------- | --------- | ----------|
| [PROV-N](https://www.w3.org/TR/prov-n/) | http://www.w3.org/TR/2013/REC-prov-n-20130430/| `text/provenance-notation; charset="UTF-8"` | .provn |
| [PROV-XML](https://www.w3.org/TR/prov-xml/) | http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/| `application/xml` | .xml |
| [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF Turtle](https://www.w3.org/TR/turtle/) | http://www.w3.org/TR/2013/REC-prov-o-20130430/ | `text/turtle; charset="UTF-8"` | `.ttl` |
| [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF N-Triples](https://www.w3.org/TR/n-triples/) | http://www.w3.org/TR/2013/REC-prov-o-20130430/ | `application/n-triples` | `.nt` |
| [PROV-O](https://www.w3.org/TR/prov-o/) as [JSON-LD](https://json-ld.org) | http://www.w3.org/TR/2013/REC-prov-o-20130430/ | `application/ld+json` | `.jsonld` |
| [PROV-JSON](https://www.w3.org/Submission/prov-json/) | http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/ | `application/json` | `.json` |

It is NOT RECOMMENDED to use [RDF/XML](http://www.w3.org/TR/rdf-syntax-grammar/) (`application/rdf+xml`) or [OWL/XML](http://www.w3.org/TR/owl2-xml-serialization/) (`application/owl+xml`) for PROV-O files.

Other provenance files (e.g. nested workflows, tool logs) MAY be included under `metadata/provenance/` 
using arbitrary filenames, for instance `workflow-af755b11-0537-4321-bf82-eadf4f6792ef.cwlprov.provn`. These files SHOULD use PROV but MAY use system-specific formats, either which SHOULD be indicated with `conformsTo` and `mediatype`. 

The [revsort-run-1 example](examples/revsort-run-1) includes these provenance serializations:

* [primary.cwlprov.provn](examples/revsort-run-1/metadata/provenance/primary.cwlprov.provn) -- [PROV-N](https://www.w3.org/TR/prov-n/) Textual Provenance Notation 
* [primary.cwlprov.xml](examples/revsort-run-1/metadata/provenance/primary.cwlprov.xml) -- [PROV-XML](https://www.w3.org/TR/prov-xml/)
* [primary.cwlprov.ttl](examples/revsort-run-1/metadata/provenance/primary.cwlprov.ttl) -- [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF Turtle](https://www.w3.org/TR/turtle/)
* [primary.cwlprov.nt](examples/revsort-run-1/metadata/provenance/primary.cwlprov.nt) -- [PROV-O](https://www.w3.org/TR/prov-o/) as [RDF N-Triples](https://www.w3.org/TR/n-triples/)
* [primary.cwlprov.jsonld](examples/revsort-run-1/metadata/provenance/primary.cwlprov.jsonld) -- [PROV-O](https://www.w3.org/TR/prov-o/) as [JSON-LD](https://json-ld.org) (`@context` subject to change)
* [primary.cwlprov.json](examples/revsort-run-1/metadata/provenance/primary.cwlprov.json) -- [PROV-JSON](https://www.w3.org/Submission/prov-json/)

For brevity the below extracts use the PROV-N syntax. Note that UUIDs and hash values 
might differ from the [example PROV](examples/revsort-run-1/metadata/provenance/primary.cwlprov.provn) file.

**TODO: Rewrite below to spec-style language**

## CWLPROV namespaces


Note that the identifiers must be expanded with the defined `prefix`-es when comparing 
across serializations. These set which vocabularies ("namespaces") are used by the 
CWLProv statements:

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

The PROV trace SHOULD use the [arcp](https://tools.ietf.org/id/draft-soilandreyes-arcp-03.html) 
base URIs corresponding to the [Research Object @base](ro.md)
or [BagIt External-Identifier](bagit.md#External-Identifier) to refer to files within the bag.

The arcp base URI SHOULD be based on the UUID of the master workflow run activity identifier, if present.


## Account who launched cwltool

If `cwltool --enable-user-provenance` was used, the local machine acccount (e.g. Windows or 
UNIX user name) who executed that command  can be tracked:


    agent(id:855c6823-bbe7-48a5-be37-b0f07f20c495, [foaf:accountName="stain", prov:type='foaf:OnlineAccount', prov:label="stain"])

It is assumed that the account was under the control of the named person 
(in PROV terms `actedOnBehalfOf`):

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
`data/` file within the Research Object has a content-addressable filename based on the checksum; CWLProv consumers SHOULD
look up this from the corresponding `metadata/manifest.json` aggregation:

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

Similarly, a step typically generates some data, here `response`:

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


# Nested workflows

If a particular step is a [nested workflow](https://www.commonwl.org/user_guide/22-nested-workflows/), 
then the inner provenance of the nested workflow runs SHOULD be included in a separate CWLProv provenance file.

In the parent workflow trace, the fact that more details of the step is expanded in a different PROV file SHOULD be declared with the `prov:has_provenance` attribute:
    
    prefix id <urn:uuid:>
    prefix provenance <arcp://uuid,73eab018-7b36-4f84-a845-aca8073bd46c/metadata/provenance/>

    agent(id:a606d227-bf10-4479-8d11-823bb932bbac, 
        [prov:type='wfprov:WorkflowEngine', prov:type='prov:SoftwareAgent', 
         prov:label="cwltool 1.0.20180817162414"])

    activity(id:73eab018-7b36-4f84-a845-aca8073bd46c, 2018-08-21T15:20:35.059920, -, 
        [prov:type='wfprov:WorkflowRun', prov:label="Run of workflow/packed.cwl#main"])
    wasStartedBy(id:73eab018-7b36-4f84-a845-aca8073bd46c, -, id:a606d227-bf10-4479-8d11-823bb932bbac, 2018-08-21T15:20:35.060038)

    activity(id:e79fc8dc-6e40-4236-b22c-41fee22947a9, -, -, 
         [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main/compile"])
    wasStartedBy(id:e79fc8dc-6e40-4236-b22c-41fee22947a9, -, id:73eab018-7b36-4f84-a845-aca8073bd46c, 2018-08-21T15:20:35.163189)

    activity(id:e79fc8dc-6e40-4236-b22c-41fee22947a9, -, -, 
         [prov:has_provenance='provenance:workflow_compile.e79fc8dc-6e40-4236-b22c-41fee22947a9.cwlprov.provn',
          prov:has_provenance='provenance:workflow_compile.e79fc8dc-6e40-4236-b22c-41fee22947a9.cwlprov.ttl'
    ])

Traces of nested workflow SHOULD be stored under `metadata/provenance` (or a subdirectory there of), 
and that their `conformsTo` and `mediatype` SHOULD conform to the 
[CWLProv formats](#Formats). It is RECOMMENDED that the file name is 
derived from the activity in the parent workflow. In the example above the main workflow 
`urn:uuid:73eab018-7b36-4f84-a845-aca8073bd46c` (started by the engine) has started the activity 
`urn:uuid:e79fc8dc-6e40-4236-b22c-41fee22947a9` of step `compile`, which has a provenance 
trace in `metadata/provenance/workflow_compile.e79fc8dc-6e40-4236-b22c-41fee22947a9.cwlprov.provn` and 
`…947a9.cwlprov.ttl`.

The attribute `prov:has_provenance` SHOULD also be used for any kind of provenance 
files in other formats (e.g. log file), even for non-workflow steps. 
CWLProv consumers SHOULD check the `conformsTo` attribute in the 
[Research Object manifest](ro.md) rather than attempting to decompose the filename.

The RO manifest SHOULD include an _annotation_ with 
motivation `http://www.w3.org/ns/prov#has_provenance` to indicate
which provenance files further describe the step activity, for instance:

    {
        "about": "urn:uuid:e79fc8dc-6e40-4236-b22c-41fee22947a9",
        "content": [
            "provenance/workflow_20compile.e79fc8dc-6e40-4236-b22c-41fee22947a9.cwlprov.provn",
            "provenance/workflow_20compile.e79fc8dc-6e40-4236-b22c-41fee22947a9.cwlprov.ttl",
        ],
        "oa:motivatedBy": {
            "@id": "http://www.w3.org/ns/prov#has_provenance"
        }
    }

It is RECOMMENDED to generate separate provenance trace files for each 
nested workflow execution, e.g. as part of scatter operation or a 
workflow reused in different steps.

For nested workflows, the identifier for the step activity (here `urn:uuid:e79fc8dc-6e40-4236-b22c-41fee22947a9`) 
SHOULD re-appear in the data of nested CWLProv files as a `wfprov:WorkflowRun` activity, e.g.:

    prefix id <urn:uuid:>
    agent(id:a606d227-bf10-4479-8d11-823bb932bbac, 
        [prov:type='wfprov:WorkflowEngine', prov:type='prov:SoftwareAgent', 
         prov:label="cwltool 1.0.20180817162414"])

    activity(id:e79fc8dc-6e40-4236-b22c-41fee22947a9, 2018-08-21T15:20:35.089187, -, 
        [prov:type='wfprov:WorkflowRun', prov:label="Run of workflow/packed.cwl#compile.cwl"])
    wasStartedBy(id:e79fc8dc-6e40-4236-b22c-41fee22947a9, -, id:a606d227-bf10-4479-8d11-823bb932bbac, 2018-08-21T15:20:35.089303)

The nested workflow provenance trace
follows the same CWLProv pattern as the primary workflow trace.  Note in the example 
above that from the perspective of the nested workflow provenance, 
`e79fc8dc…` is a `wfprov:WorkflowRun` that `wasStartedBy` the engine
`a606d227…`, not a `wfprov:ProcessRun` started by the parent workflow
activity `73eab018…` as depicted in the primary provenance trace.  

It is NOT REQUIRED that the nested workflow activity `wasStartedBy`
by the same engine as the primary workflow, for instance the nested workflow
might have been executed on a different node.

The nested workflow trace SHOULD declare its relevant prospective provenance
`prov:Plan` entities, even if this would duplicate parts of the prospective
 provenance in the parent workflow provenance.

Nested workflows may have steps that themselves are nested workflows,
which SHOULD be logged in separate CWLProv provenance file, linked
with `prov:has_provenance` attribute from the intermediate provenance file.

Provenance links to paths within the CWLProv Bag MUST use the same
base URI (e.g. `arcp://uuid,73eab018-7b36-4f84-a845-aca8073bd46c/`)
as in the primary workflow provenance and research object manifest.

## Files

If a step uses or generates a _file_ with a particular filename, then this SHOULD 
be indicated as a specialization of the c content-based entity:

    prefix id <urn:uuid:>
    prefix data <urn:hash::sha1:>

    entity(data:03cfd743661f07975fa2f1220c5194cbaff48451, [prov:type='wfprov:Artifact'])
    specializationOf(id:01f2a6f5-425c-44f4-8b4b-74b7885d216a, data:03cfd743661f07975fa2f1220c5194cbaff48451)

The specializing entity SHOULD be of type `wf4ever:File` and 
SHOULD declare the filename (as seen by the CWL tool) 
without a path using the attribute `cwlprov:basename` 
corresponding to CWL's [basename](https://www.commonwl.org/v1.0/CommandLineTool.html#File)
attribute:

    prefix cwlprov <https://w3id.org/cwl/prov#>
    prefix wf4ever <http://purl.org/wf4ever/wf4ever#>
    entity(id:01f2a6f5-425c-44f4-8b4b-74b7885d216a, 
      [prov:type='wf4ever:File', prov:type='wfprov:Artifact', 
       cwlprov:basename="f.txt", cwlprov:nameroot="f", cwlprov:nameext=".txt"])

Additional attributes `cwlprov:nameroot` and `cwlprov:nameext` MAY be included 
as shown above, corresponding to `nameroot` and `nameext` fields in a 
[CWL File](https://www.commonwl.org/v1.0/CommandLineTool.html#File).

It is RECOMMENDED that such file entities, if declared, are referenced from
the corresponding `used` and `wasGeneratedBy` instead of by their content-based
entity:

  activity(id:284408d9-9f21-4c63-b1c0-fbed7d0e3180, -, -, 
    [prov:type='wfprov:ProcessRun', prov:label="Run of workflow/packed.cwl#main/step1"])
  wasGeneratedBy(id:01f2a6f5-425c-44f4-8b4b-74b7885d216a, 
    id:284408d9-9f21-4c63-b1c0-fbed7d0e3180, 2018-08-22T13:55:13.034450,
    [prov:role='wf:main/step1/file1'])

CWLProv consumers should note that the same
content (by checksum) may appear as different file entities for different steps.
In the case of no direct `wasGeneratedBy`—`use` derivations in a provenance trace,
consumers could consider a common `specializationOf` entity as a
hint of indirect provenance derivations (e.g. unpacked from an archive or 
up/downloaded through a database).


## Secondary files

A [CWL File](https://www.commonwl.org/v1.0/CommandLineTool.html#File) may have
`secondaryFiles`, additional files or directories that are associated with the 
primary file.

In CWLProv any secondary files SHOULD be declared as a `cwlprov:SecondaryFile` kind of
derivation from the primary file entity.

  entity(id:01f2a6f5-425c-44f4-8b4b-74b7885d216a, 
    [prov:type='wf4ever:File', prov:type='wfprov:Artifact', 
      cwlprov:basename="f.txt", cwlprov:nameroot="f", cwlprov:nameext=".txt"])

  wasDerivedFrom(id:f233d70d-614c-4dbc-95b9-e7a10536d866, 
    id:01f2a6f5-425c-44f4-8b4b-74b7885d216a, -, -, -, [prov:type='cwlprov:SecondaryFile'])
  entity(id:f233d70d-614c-4dbc-95b9-e7a10536d866, 
    [prov:type='wf4ever:File', prov:type='wfprov:Artifact', 
      cwlprov:basename="f.txt.idx", cwlprov:nameroot="f.txt", cwlprov:nameext=".idx"])

When the `SecondaryFile` relation is used, both the primary and secondary file 
MUST have `cwlprov:basename` declared. It is common, but not required, that
part of a secondary file's filename (here `nameroot`) correspond
with (part of) the primary file's `basename` field.

The secondary file SHOULD have a specialization of its own content-based entity:

    specializationOf(id:f233d70d-614c-4dbc-95b9-e7a10536d866, data:a3db5c13ff90a36963278c6a39e4ee3c22e2a436)

Note that the two content-based entities are not directly related in the provenance trace, 
as different manifestations as files may have different secondary files and 
different `basename` patterns.

Note that secondary files are not normally seen directly `used` or `wasGeneratedBy` by
activities dealing with their primary files, however this can
often be reasonably assumed.


### Referencing secondary files from job files

The CWLProv relations `specializationOf`, `wasDerivedFrom` 
with `wf4ever:File` entities as above can be combined with 
the `bundledAs` reference in the Research Object `metadata/manifest.json` 
and the BagIt manifest checksums to recreate a CWL `class:File` representation.

For example from the above:

    {
        "file1": {
            "class": "File",
            "location": "data/01/01f2a6f5-425c-44f4-8b4b-74b7885d216a",
            "checksum": "sha1$01f2a6f5-425c-44f4-8b4b-74b7885d216a"
            "basename": "f.txt",
            "nameroot": "f",
            "nameext": ".txt",
            "secondaryFiles": [
                {
                    "class": "File",
                    "location": "data/a3/a3db5c13ff90a36963278c6a39e4ee3c22e2a436",
                    "checksum": "sha1$a3db5c13ff90a36963278c6a39e4ee3c22e2a436"
                    "basename": "f.txt.idx",
                    "nameroot": "f.txt",
                    "nameext": ".idx",
                }
            ],
        }
    }

Note how above `data/a3/a3db…436` can be a secondary file 
for `data/01/01f2…16a` even if the actual files do not have 
any extensions and are not in the same directory.

## Directories

A [CWL Directory](https://www.commonwl.org/v1.0/CommandLineTool.html#Directory) 
represent an unordered collection of named files or directories.

In CWLProv a directory is represented as a [PROV Dictionary](https://www.w3.org/TR/prov-dictionary/)
of type `ro:Folder` using the relative filenames as keys to [CWL Files](#Files).

    entity(id:77154164-d82c-4266-82a6-78f2f6d0218c, 
      [prov:type='wfprov:Artifact', prov:type='prov:Collection', 
       prov:type='prov:Dictionary', prov:type='ro:Folder',
       cwlprov:basename="dir",
       prov:hadDictionaryMember='id:a8a6f9fb-5c51-40bb-96bb-20aa3322d294', 
       prov:hadDictionaryMember='id:0a867366-b749-43d9-a40b-565d8cf62975', 
       prov:hadDictionaryMember='id:28f0d087-308b-4c91-bb02-7548ccc59673'])

    entity(id:a8a6f9fb-5c51-40bb-96bb-20aa3322d294, 
      [prov:type='prov:KeyEntityPair', prov:pairKey="a.txt", 
       prov:pairEntity='id:c9962818-63f5-4f26-9020-f59799000602'])
    entity(id:c9962818-63f5-4f26-9020-f59799000602, 
      [prov:type='wfprov:Artifact', prov:type='wf4ever:File', cwlprov:basename="a.txt"])  
    specializationOf(id:c9962818-63f5-4f26-9020-f59799000602, data:0a4d55a8d778e5022fab701977c5d840bbc486d0)

    entity(id:0a867366-b749-43d9-a40b-565d8cf62975, 
      [prov:type='prov:KeyEntityPair', prov:pairKey="b", 
       prov:pairEntity='id:8afd9368-b1f5-4dd6-baf5-940e81164c92'])
    entity(id:8afd9368-b1f5-4dd6-baf5-940e81164c92, 
      [prov:type='wfprov:Artifact', prov:type='wf4ever:File', cwlprov:basename="b"])
    # …

Note that a directory may contain subdirectories, described in the same way:

    entity(id:28f0d087-308b-4c91-bb02-7548ccc59673,   
      [prov:type='prov:KeyEntityPair', prov:pairKey="c", 
       prov:pairEntity='id:8a6db3a0-89e5-48b7-b7d0-63960dfa2e55'])

    entity(id:8a6db3a0-89e5-48b7-b7d0-63960dfa2e55, 
      [prov:type='wfprov:Artifact', prov:type='prov:Collection',
       prov:type='prov:Dictionary', prov:type='ro:Folder',
       cwlprov:basename="c",
       prov:hadDictionaryMember='id:d5467842-7cb2-4f3f-8d71-fbd0760ca0a3'])

    entity(id:d5467842-7cb2-4f3f-8d71-fbd0760ca0a3, 
      [prov:type='prov:KeyEntityPair', prov:pairKey="d.txt", 
       prov:pairEntity='id:098e70a3-5078-440d-98ab-4fb8b674384e'])
    # …

Graphically we can imagine this as:

    ./                    entity 77154164-d82c-4266-82a6-78f2f6d0218c
      a.txt               entity c9962818-63f5-4f26-9020-f59799000602
        "Hello World"   checksum 0a4d55a8d778e5022fab701977c5d840bbc486d0
      b                   entity 8afd9368-b1f5-4dd6-baf5-940e81164c92
        …                      …
      c/                  entity 8a6db3a0-89e5-48b7-b7d0-63960dfa2e55
        d.txt             entity 098e70a3-5078-440d-98ab-4fb8b674384e
          …                    …

Note that the `prov:KeyEntityPair` are entities to indicate the binding
of a file or directory in a parent directory, and as an intermediary 
their UUID will only show up as part of describing the directory.

CWLProv clients describing directories SHOULD also provide the collection 
membership of the directory content:

    hadMember(id:77154164-d82c-4266-82a6-78f2f6d0218c, id:c9962818-63f5-4f26-9020-f59799000602)
    hadMember(id:77154164-d82c-4266-82a6-78f2f6d0218c, id:8afd9368-b1f5-4dd6-baf5-940e81164c92)
    hadMember(id:77154164-d82c-4266-82a6-78f2f6d0218c, id:8a6db3a0-89e5-48b7-b7d0-63960dfa2e55)
    
    hadMember(id:8a6db3a0-89e5-48b7-b7d0-63960dfa2e55, id:098e70a3-5078-440d-98ab-4fb8b674384e)

With the short-cut `hadMember` the filenames are not indicated. 
Note that although `cwlprov:basename` of the members might match the `prov:pairKey`, 
only the pair key is authorative as a file might be member of 
multiple directories under different names.


## Recreating a CWL directory listing

It is possible to recreate a CWL directory listing using the 
CWLProv relations above, combined with the `bundledAs` 
reference in the Research Object `metadata/manifest.json` 
and the BagIt manifest checksums to recreate a CWL `class:File` representation.

From the example above:

    {
        "dir": {
            "class": "Directory",
            "basename": "dir",
            "listing": [
                {
                    "class": "File",
                    "basename": "a.txt",
                    "location": "../data/0a/0a4d55a8d778e5022fab701977c5d840bbc486d0",
                    "checksum": "sha1$0a4d55a8d778e5022fab701977c5d840bbc486d0"
                },
                {
                    "class": "File",
                    "basename": "b",
                    "location": "../data/86/86f7e437faa5a7fce15d1ddcb9eaeaea377667b8",
                    "checksum": "sha1$86f7e437faa5a7fce15d1ddcb9eaeaea377667b8"
                },
                {
                    "class": "Directory",
                    "basename": "c",
                    listing: [
                        {
                            "class": "File",
                            "basename": "d.txt",
                            "location": "../data/35/35693c2208d9f20c04eb452737b9bfe32be747ce",
                            "checksum": "sha1$35693c2208d9f20c04eb452737b9bfe32be747ce"
                        }
                    ]
                }
            ]
        }
    }

Note that using the such listings files can be provided as a directory
structure even though the actual files are organized in a different hierarchy
within the CWLProv bag, effectively allowing the same value to be present in 
multiple directories without duplication.


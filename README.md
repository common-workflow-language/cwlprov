# cwlprov
Profile for provenance research object of a CWL workflow run.

## Quicklinks

* CWLProv Examples:
  * [revsort-run-1](examples/revsort-run-1/) - execution of [revsort.cwl](https://github.com/common-workflow-language/cwltool/blob/1.0.20180521150620/tests/wf/revsort.cwl) (CWLProv 0.3.0)
  * [RunTimeResearchObject](https://zenodo.org/record/1215611/files/RunTimeResearchObject-f0b553d37e4255a3291393948f3e308bd88ed301.zip?download=1) execution of a [sequence alignment workflow](https://github.com/FarahZKhan/scalability-reproducibility-chapter/blob/ProvCaptureDemo/CWL/workflow_simple.cwl) (CWLProv 0.2.0) - from [10.5281/10.5281/zenodo.1215611](https://doi.org/10.5281/10.5281/zenodo.1215611)
* CWLProv posters:
  * [CWLProv – Interoperable retrospective provenance capture and its challenges](https://doi.org/10.7490/f1000research.1115721.1), _BOSC 2018_ ([10.7490/f1000research.1115721.1](https://10.7490/f1000research.1115721.1))
  * [CWL+Research Object == Complete Provenance](https://doi.org/10.7490/f1000research.1114781.1), _BOSC 2017_ ([10.7490/f1000research.1114781.1](https://doi.org/10.7490/f1000research.1114781.1))
* CWLProv slides:
  * [CWLProv Retrospective provenance capture and its challenges](https://slides.com/farahzkhan/cwlprov), _BOSC 2018_
  * [Reproducible BioCompute Objects using Common Workflow Language](http://slides.com/soilandreyes/2018-03-23-bco-cwl-ro#/), _BioCompute Object_
  * [CWL Research Objects](http://slides.com/soilandreyes/2018-01-26-cwl-ro-elixir#/), _ELIXIR_
  * [Challenges in interoperable provenance capture](http://slides.com/soilandreyes/2018-01-15-interoperable-provenance#/), _Research Data Alliance_
  * [The Archive and Package (arcp) URI scheme](http://slides.com/soilandreyes/2018-03-23-arcp-uri-scheme#/)
* Papers:
  * [CWLProv - Interoperable Retrospective Provenance capture and its challenges](https://doi.org/10.5281/zenodo.1208477) _Zenodo preprint_ ([10.5281/zenodo.1208477](https://doi.org/10.5281/zenodo.1208477))


## Overview

_CWLProv_ is an informal profile to define how to record provenance of a workflow run (typically [CWL](https://www.commonwl.org/) or [Nextflow](https://github.com/edgano/researchObject-Nextflow)), captured as a research object using Linked Data standards. 

There are two parts to this profile:

* [CWLProv Research Object](ro.md), how the resources of an execution is packaged in an [RO](http://researchobject.org/)
* [CWLProv PROV](prov.md), how the workflow execution is modelled in [W3C PROV](https://www.w3.org/TR/prov-overview/).

This repository may later also include formal profiles for validation, e.g. [BagIt profile](https://github.com/bagit-profiles/bagit-profiles) of included resources, [ShEx](http://shex.io/) for manifest content, and [PROV Template](https://provenance.ecs.soton.ac.uk/prov-template-2014-06-07/) to document PROV structures.

The [CWLProv white paper](https://doi.org/10.5281/zenodo.1208477) describes the background and motivation for this profile. For the avoidance of doubt, from CWLProv 0.3.0 this repository is authorative of CWLProv specifications.


## License

This repository is distributed under [Apache License, version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

See the file [LICENSE.txt](LICENSE.txt) for details, and [NOTICE](NOTICE) for required notices.

## Contributing

CWLProv is maintained at https://github.com/common-workflow-language/cwlprov/

Feel free to raise an [issue](https://github.com/common-workflow-language/cwlprov/issues) or a [pull request](https://github.com/common-workflow-language/cwlprov/pulls) to contribute to CWLProv. Contributions are assumed to be covered by [section 5 of the Apache License](https://www.apache.org/licenses/LICENSE-2.0#contributions).

You may also want to contribute a corresponding issue or pull request in the [cwltool](https://github.com/common-workflow-language/cwltool) reference implementation.


## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in documents of this repository are to be interpreted 
as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).


## Versions

CWLProv is versioned using [Semantic Versioning](https://semver.org/spec/v2.0.0.html), following the pattern `MAJOR.MINOR.PATCH` (e.g. `1.2.0`).

To determine version compatibility we consider the packaging of a CWLProv RO as a kind of 'API'.
Examples of changes to the API:

* Major version change: Removal of resource type, change of format of PROV, removing annotations, changing namespaces
* Minor version change: Adding other resources, adding annotations, additional properties, changing entity identifier scheme, change of file paths in RO
* Patch version change: Fixing syntactical typos (e.g. invalid JSON-LD), 

You may spot that _change of file path_ is classified as minor, that is because paths can be found dynamically by following links from the manifest, its annotations and the PROV traces. This is similar to REST principles where URI templates should not be assumed, but followed from links.

The current versions of CWLProv have major `0`, indicating that disruptive changes MAY occur before the profile stabilize at `1.x.y`.

Each CWLProv version has a `w3id.org` _permalink_ that SHOULD be declared inside the [RO](ro.md) to indicate its conformance. 

* 0.3.0 [https://w3id.org/cwl/prov/0.3.0](https://w3id.org/cwl/prov/0.3.0) First implementation [merged](https://github.com/common-workflow-language/cwltool/pull/676) into cwltool. (declares `conformsTo` on `/` and PROV files)
* 0.2.0 [https://w3id.org/cwl/prov/0.2.0](https://w3id.org/cwl/prov/0.2.0) Prototype as exemplified in [https://doi.org/10.5281/zenodo.1215611](https://doi.org/10.5281/zenodo.1215611) (declares `conformsTo` as https://doi.org/10.5281/zenodo.1208477 on PROV files)
* 0.1.0 [https://w3id.org/cwl/prov/0.1.0](https://w3id.org/cwl/prov/0.1.0) Prototype as exemplified in [https://doi.org/10.5281/zenodo.1208478](https://doi.org/10.5281/zenodo.1208478) (no self-declaration of CWLProv)


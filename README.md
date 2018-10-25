# cwlprov
Profile for provenance research object of a CWL workflow run.

## Quicklinks

* CWLProv Examples:
  * [revsort-run-1](examples/revsort-run-1/) - execution of [revsort.cwl](https://github.com/common-workflow-language/cwltool/blob/1.0.20180521150620/tests/wf/revsort.cwl) (CWLProv 0.4.0)
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

There are three parts to this profile:

* [CWLProv BagIt](bagit.md), how the resources of an execution are _packaged_ using [BagIt](https://tools.ietf.org/html/draft-kunze-bagit-16)
* [CWLProv Research Object](ro.md), how the resources of an execution are _related_ in an [RO](http://researchobject.org/)
* [CWLProv PROV](prov.md), how the workflow execution _provenance_ is modelled in [W3C PROV](https://www.w3.org/TR/prov-overview/)

This repository may later also include formal profiles for computational validation, e.g. [BagIt profile](https://github.com/bagit-profiles/bagit-profiles) of included resources, [ShEx](http://shex.io/) for manifest content, and [PROV Template](https://provenance.ecs.soton.ac.uk/prov-template-2014-06-07/) to document PROV structures.

The [CWLProv white paper](https://doi.org/10.5281/zenodo.1208477) describes the background and motivation for this profile. For the avoidance of doubt, from CWLProv 0.3.0 this GitHub repository is authoritative of CWLProv specifications.


## Known implementations

* [cwltool --provenance](https://github.com/common-workflow-language/cwltool/blob/master/CWLProv.rst) (reference implementation)
* [cwlprov-py](https://github.com/common-workflow-language/cwlprov-py) (command line tool to inspect)
* [nextflow -with-prov](https://github.com/edgano/researchObject-Nextflow) (work in progress, approaching CWLProv without CWL)
* [toil](https://github.com/DataBiosphere/toil/issues/2390) (planned)

## License

This repository is distributed under [Apache License, version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

See the file [LICENSE.txt](LICENSE.txt) for details, and [NOTICE](NOTICE) for required notices.

## Contributing

CWLProv is maintained at https://github.com/common-workflow-language/cwlprov/

Feel free to raise an [issue](https://github.com/common-workflow-language/cwlprov/issues) or a [pull request](https://github.com/common-workflow-language/cwlprov/pulls) to contribute to CWLProv. Contributions are assumed to be covered by [section 5 of the Apache License](https://www.apache.org/licenses/LICENSE-2.0#contributions).

You may also want to contribute a corresponding issue or pull request in the [cwltool](https://github.com/common-workflow-language/cwltool) reference implementation, in particular 
[cwltool/provenance.py](https://github.com/common-workflow-language/cwltool/blob/master/cwltool/provenance.py) and documentation on [cwltool --provenance](https://github.com/common-workflow-language/cwltool/blob/master/CWLProv.rst) support.

For an informal CWLProv discussion with other developers, join the (relatively quiet) Gitter room [common-workflow-language/cwlprov](https://gitter.im/common-workflow-language/cwlprov), or the 
(more busy) [common-workflow-language/common-workflow-language](https://gitter.im/common-workflow-language/common-workflow-language).

### Code of Conduct

The CWL Project is dedicated to providing a harassment-free experience for everyone, regardless of gender, gender identity and expression, sexual orientation, disability, physical appearance, body size, age, race, or religion. We do not tolerate harassment of participants in any form. This code of conduct applies to all CWL Project spaces, including the Google Group, the Gitter chat room, the Google Hangouts chats, both online and off. Anyone who violates this code of conduct may be sanctioned or expelled from these spaces at the discretion of the leadership team.

For more details, see our [Code of Conduct](https://github.com/common-workflow-language/common-workflow-language/blob/master/CODE_OF_CONDUCT.md).


## Requirements Language

The key words `MUST`, `MUST NOT`, `REQUIRED`, `SHALL`, `SHALL
NOT`, `SHOULD`, `SHOULD NOT`, `RECOMMENDED`,  `MAY`, and
`OPTIONAL` in documents of this repository are to be interpreted 
as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).


## Versions


* 0.6.0 [https://w3id.org/cwl/prov/0.6.0](https://w3id.org/cwl/prov/0.6.0) Adds `metadata/logs`, no longer snapshot input files (they are also under `data/`) (introduced in cwltool [1.0.20181012180214](https://github.com/common-workflow-language/cwltool/releases/tag/1.0.20181012180214))
* 0.5.0 [https://w3id.org/cwl/prov/0.5.0](https://w3id.org/cwl/prov/0.5.0) Adds `workflow/primary-output.json` (introduced in cwltool [1.0.20180912090223](https://github.com/common-workflow-language/cwltool/releases/tag/1.0.20180912090223))
* 0.4.0 [https://w3id.org/cwl/prov/0.4.0](https://w3id.org/cwl/prov/0.4.0) Declares directories and secondary files (introduced in cwltool [1.0.20180819175200](https://github.com/common-workflow-language/cwltool/releases/tag/1.0.20180819175200))
* 0.3.0 [https://w3id.org/cwl/prov/0.3.0](https://w3id.org/cwl/prov/0.3.0) Semantic versioning of CWLProv (introduced in cwltool [1.0.20180711112827](https://github.com/common-workflow-language/cwltool/releases/tag/1.0.20180711112827))
* 0.2.0 [https://w3id.org/cwl/prov/0.2.0](https://w3id.org/cwl/prov/0.2.0) Prototype as exemplified in [https://doi.org/10.5281/zenodo.1215611](https://doi.org/10.5281/zenodo.1215611) - (Note: No semantic versioning, `conformsTo` https://doi.org/10.5281/zenodo.1208477 on PROV files)
* 0.1.0 [https://w3id.org/cwl/prov/0.1.0](https://w3id.org/cwl/prov/0.1.0) Prototype as exemplified in [https://doi.org/10.5281/zenodo.1208478](https://doi.org/10.5281/zenodo.1208478) (Note: No self-declaration of CWLProv version)


CWLProv is versioned using [Semantic Versioning](https://semver.org/spec/v2.0.0.html), following the pattern `MAJOR.MINOR.PATCH` (e.g. `1.2.0`).

To determine version compatibility we consider the packaging of a CWLProv RO as a kind of "API". Examples of changes to CWLProv:

* Major version change: Removal of resource type, change of format of PROV, removing annotations, changing namespaces, removing PROV statement patterns
* Minor version change: Adding other resources, adding annotations, additional properties, changing entity identifier scheme, change of file paths in RO, minor change of underlying syntax and package version, adding/augmenting PROV statement patterns, conformance to [PROV constraints](https://www.w3.org/TR/prov-constraints/)
* Patch version change: Fixing syntactical typos (e.g. invalid or inefficient [JSON-LD](https://json-ld.org/)), inconsistencies in textual language, adding [inferred PROV statements](http://www.w3.org/TR/prov-sem/)

This means that consumers of CWLProv can make strong assumptions on backwards and forwards compatibility:

* Major: Unsupported major versions can't be safely parsed
* Minor: Can safely parse (but not reproduce) newer versions. Parsing older versions is safe if later CWLProv additions are handled as optional.
* Patch: Differences can usually be safely ignored

_Unless a patch version is affecting the output, the declared profile SHOULD have patch version `0` even if the code was implemented with a later CWLProv._

**Tip:** You may spot that _change of file paths_ is classified as minor, that is because paths can be found dynamically by following links from the manifest, its annotations and the PROV traces. This is similar to REST principles where URI templates should not be assumed, but followed from links._

The current version of CWLProv have major `0`, indicating that [disruptive changes](https://semver.org/spec/v2.0.0.html#spec-item-4) may occur before the profile stabilize at `1.x.y`.

Each CWLProv version has a `w3id.org` _permalink_ that SHOULD be declared inside the [RO](ro.md) to indicate its conformance. 


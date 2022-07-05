## Packaging sbol-utilities
To package sbol-utilities, I checked which dependencies are available or not in conda (conda-forge or bioconda channel).

#################################################################################################################################################

*name:* **sbol-utilities v1.0a15**
*dependencies:*
- python >= 3.7
- **sbol3>=1.0b11** #(pysbol3 not in conda) 
- **sbol2>=1.4** #(pysbol2 available in conda-forge (v1.2) but need to be updated to the latest version)
- rdflib #(conda-forge ok)
- biopython #(conda-forge ok)
- graphviz #(conda-forge ok)
- **tyto>=1.0** #(not in conda)
- openpyxl #(conda-forge ok)
- **sbol_factory** >=1.0a11 #(not in conda)
- pytest #for tests (conda-forge ok)
- interrogate #for tests (conda-forge ok)

#################################################################################################################################################

First **pysbol2** was updated in *conda-forge* to **v1.4.1**: https://anaconda.org/conda-forge/pysbol2/files . The tests were also added and checked.

*Encountred problems:*
- java path not found / Use of unittest instead of pytest: https://github.com/SynBioDex/pySBOL2/issues/417#issuecomment-1099225905
- Permission denied to the feedstock of pysbol2 (Use of conda-smithy to rerender locally: https://conda-forge.org/docs/maintainer/updating_pkgs.html#dev-update-rerender) : https://github.com/conda-forge/pysbol2-feedstock/pull/4#discussion_r851213278

Secondly, a conda package was created for **pysbol3 v 1.0.1** (*conda-forge*) : https://anaconda.org/conda-forge/pysbol3.

*Encountred problems:*
- pysbol3 has a submodule required for test section : https://github.com/conda-forge/staged-recipes/pull/18571

Thirdly, **tyto** was packaged in *bioconda* channel (**v1.0b3**) : https://anaconda.org/bioconda/tyto.

*Encountred problems:*
- Tyto include non packaged submodules which was not accepted by conda-forge (https://github.com/conda-forge/staged-recipes/pull/18678) but accepted by bioconda (https://github.com/bioconda/bioconda-recipes/pull/34786#issuecomment-1121121694)

Fourthly, **sbol_factory** was packaged in *conda-forge* channel https://anaconda.org/conda-forge/sbol_factory (**v 1.0**).

*Encountred problems:*
- windows error: test failed because of CRLF endlines with filecmp function : https://github.com/SynBioDex/sbol_factory/pull/702 --> Another way of comparison was used to avoid this problem.
- Need to deploy a stable release with a stable identifier (1.0 instead of 1.0a12): https://github.com/SynBioDex/sbol_factory/pull/70

Finally, **sbol-utilities** was packaged in *bioconda* channel (**1.0a16**) : https://anaconda.org/bioconda/sbol-utilities/files.

*Encountred problems:*
- nodejs was added to the dependencies for sbol-converter + openjdk + python-graphviz
- test errors concerning genbank : https://github.com/SynBioDex/SBOL-utilities/issues/121 , it was finally dropped because it does not happen on bioconda platform, only with conda develop locally.
- javascript error using the command line "/usr/bin/env" on Bioconda: https://github.com/SynBioDex/SBOL-utilities/issues/132. The file was fixed. A patch was created (by Guillaume) to fix this error and added to the recipe.  
- test dotfile generation failed : https://github.com/SynBioDex/SBOL-utilities/issues/134 . The test was bypassed because it's not a mandatory test (in agreement with the developer) and no solution was found until now.

To add/update a recipe in conda-forge or bioconda, I used the following instructions: https://github.com/brsynth/synbiocad-galaxy-docs/releases/tag/1.0.0 (packaging_conda_forge.md and packaging_bioconda.md).
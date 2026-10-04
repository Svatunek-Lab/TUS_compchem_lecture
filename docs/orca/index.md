# ORCA (advanced)

[ORCA](https://www.faccts.de/orca/) is a full quantum chemistry program and is widely used
in organic and organometallic chemistry. For research calculations it is far more
capable and faster than the Python tools in the hands-on part: modern DFT methods,
solvation models, transition-state searches, spectroscopy and accurate wavefunction
methods such as DLPNO-CCSD(T).

ORCA does not run in Colab. You install it on your own computer or on a cluster, write a
text input file and run it from the command line.

!!! info "Licence"
    ORCA is free for academic use. Register on the
    [ORCA forum](https://orcaforum.kofo.mpg.de) to download it. Commercial use requires
    a licence from [FAccTs](https://www.faccts.de).

## In this section

| Page | What it covers |
| ---- | -------------- |
| [Running ORCA](running.md) | Installing, the structure of an input file, running a job, output files |
| [Example: optimisation + frequencies](opt-freq.md) | Optimise acetone and check that it is a minimum |

## How this connects to the hands-on part

The workflow is the same as in the Python notebooks: build a structure, choose a method
and basis set, run the calculation, read the results. You can prepare structures with
RDKit or ASE, write them to `.xyz` and use them as ORCA input. ASE can also
[run ORCA directly](https://wiki.fysik.dtu.dk/ase/ase/calculators/orca.html) as a
calculator.

<!--
To add an ORCA example:
  1. Put the input files in docs/files/orca/
  2. Copy opt-freq.md, change the text and the download links
  3. Add the page to `nav` in mkdocs.yml and to the table above
-->

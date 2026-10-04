# Example: optimisation + frequencies

<div class="nb-buttons" markdown>
[:material-download: acetone.inp](../files/orca/acetone.inp){ .md-button .md-button--primary download }
[:material-download: acetone.xyz](../files/orca/acetone.xyz){ .md-button download }
</div>

The most common calculation in computational organic chemistry: find the equilibrium
structure of a molecule and confirm that it is a true minimum.

## Why both steps?

- **Optimisation** (`Opt`) moves the atoms downhill on the potential energy surface until
  the forces are zero. That point could be a minimum, but also a saddle point.
- **Frequencies** (`Freq`) calculate the curvature of the surface at that point.
    - All frequencies real (positive): a **minimum**, i.e. a stable structure.
    - Exactly one imaginary frequency (negative in the output): a **transition state**.
    - The frequencies also give zero-point energy, enthalpy and Gibbs free energy.

## The input

```text title="acetone.inp"
--8<-- "docs/files/orca/acetone.inp"
```

```text title="acetone.xyz (starting guess)"
--8<-- "docs/files/orca/acetone.xyz"
```

The starting structure doesn't need to be good, just reasonable. It could come from
RDKit, ASE, Avogadro or a crystal structure.

## Run it

```bash
mkdir acetone && cd acetone
# put acetone.inp and acetone.xyz here
cp acetone.xyz acetone_start.xyz   # ORCA overwrites acetone.xyz with the result
$(which orca) acetone.inp > acetone.out
```

## Check the results

1. **Did it finish?** The last lines should contain `ORCA TERMINATED NORMALLY`.
2. **Did the optimisation converge?** Look for `OPTIMIZATION RUN DONE`.
3. **Is it a minimum?** Under `VIBRATIONAL FREQUENCIES` the first six values are 0
   (translations and rotations). All others must be positive.
4. **Energies.** Note the `FINAL SINGLE POINT ENERGY` and the `Final Gibbs free energy`.

!!! question "Try it yourself"
    - Open `acetone_trj.xyz` in a viewer and watch the optimisation.
    - Animate the C=O stretch in your viewer. Where is it in the IR spectrum?
    - Replace acetone with another molecule from the hands-on notebooks.

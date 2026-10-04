# Running ORCA

ORCA has no graphical interface. You write an input file (`.inp`), run ORCA on it from
the command line, and read the output file (`.out`).

ORCA is free for academic use. Installation is described in the
[ORCA installation tutorial](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/install.html).

## Windows: use WSL

On Windows we recommend running ORCA inside **WSL** (Windows Subsystem for Linux). WSL
gives you a Linux system inside Windows, so you can follow the Linux instructions on this
page and in the ORCA documentation.

1. **Install WSL.** Right-click the Start button → **Terminal (Admin)** (or
   **PowerShell (Admin)**) and run:

    ```powershell
    wsl --install
    ```

    Restart the computer. Ubuntu then opens and asks you to choose a user name and
    password.

2. **Open WSL** later via the Start menu → **Ubuntu**.

3. **Install ORCA inside WSL:** download the **Linux** version of ORCA and follow the
   Linux installation instructions.

### Copying files between Windows and WSL

=== "With File Explorer"

    In File Explorer, click **Linux** at the bottom of the left sidebar → **Ubuntu** →
    `home` → *your user name*. Drag and drop files there like in any other folder.

    From inside WSL you can also open the current folder in File Explorer:

    ```bash
    explorer.exe .
    ```

=== "From the command line"

    Your Windows drives are under `/mnt/` in WSL. For example, to copy an input file
    from your Windows Downloads folder into the current WSL folder:

    ```bash
    cp /mnt/c/Users/<windows-user-name>/Downloads/n2_sp.inp .
    ```

## macOS

You run ORCA in the **Terminal** app: Applications → Utilities → Terminal, or press
++cmd+space++ and type "Terminal".

The Terminal is a window where you type commands instead of clicking. A few things that
help:

- **Go to a folder:** type `cd ` (with a space), drag the folder from Finder into the
  Terminal window and press ++enter++. Terminal is now "in" that folder.
- **Show the current folder in Finder:** `open .`
- **List the files in the current folder:** `ls`

Download the **macOS** version of ORCA that matches your Mac: Apple Silicon (M1, M2, …)
or Intel. Apple menu → **About This Mac** shows which one you have.

## Linux

Open a terminal, e.g. with ++ctrl+alt+t++.

---

**The steps below are the same on Linux, macOS and WSL.**

## 1 · Check that ORCA is found

```bash
orca
```

ORCA should print a short message that it needs an input file. If you get
"command not found", the ORCA folder is not on your `PATH`. Either fix that (see the
installation tutorial) or call ORCA with its full path, e.g. `~/orca/orca`.

## 2 · Go to the folder with your input file

Use a separate folder for each calculation; ORCA writes several files next to the input.

```bash
mkdir -p ~/calcs/n2_sp
cd ~/calcs/n2_sp
```

Copy `n2_sp.inp` into this folder, then check that it is there with `ls`.

## 3 · Run ORCA

```bash
orca n2_sp.inp > n2_sp.out
```

`> n2_sp.out` writes the output into a file instead of onto the screen. The command
returns when the calculation is finished.

??? info "Running on several cores"
    With `%pal nprocs 4 end` in the input, ORCA runs in parallel. This needs OpenMPI,
    and ORCA must be called with its **full path**:

    ```bash
    $(which orca) n2_sp.inp > n2_sp.out
    ```

    See [Running a calculation in parallel](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/parallel.html).

??? info "Long calculations"
    To keep a calculation running after you close the terminal, and to see the output
    while it runs:

    ```bash
    nohup orca n2_sp.inp > n2_sp.out &
    tail -f n2_sp.out      # Ctrl+C stops watching, not the calculation
    ```

## 4 · Check the output

Open `n2_sp.out` in a text editor. Useful lines to search for:

| Search for | Meaning |
| ---------- | ------- |
| `ORCA TERMINATED NORMALLY` | The calculation finished without errors (at the very end) |
| `FINAL SINGLE POINT ENERGY` | Total energy in Hartree |
| `OPTIMIZATION RUN DONE` | A geometry optimisation converged |

From the command line:

```bash
grep "FINAL SINGLE POINT ENERGY" n2_sp.out
```

## More

- [Your first ORCA calculation](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/first_calc.html)
- [Input and output](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/input_output.html)
- [Graphical user interfaces](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/GUI.html) for viewing structures and results

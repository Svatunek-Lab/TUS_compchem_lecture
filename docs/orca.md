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
++cmd+space++ and type "Terminal". The Terminal is a window where you type commands
instead of clicking. A few Mac-specific things that help:

- **Go to a folder:** type `cd ` (with a space), drag the folder from Finder into the
  Terminal window and press ++enter++. Terminal is now "in" that folder.
- **Show the current folder in Finder:** `open .`
- **List the files in the current folder:** `ls`

Download the **macOS** version of ORCA that matches your Mac: Apple Silicon (M1, M2, …)
or Intel. Apple menu → **About This Mac** shows which one you have.

## 0 · Open the command line

All following steps are typed into a command line (terminal) and are the same on
every system.

| System | How to open the command line |
| ------ | ---------------------------- |
| Windows | Start menu → **Ubuntu** (this is WSL, see above) |
| macOS | ++cmd+space++ → type "Terminal" → ++enter++ |
| Linux | ++ctrl+alt+t++, or search for "Terminal" |

You type a command and press ++enter++ to run it. The basic commands you need:

| Command | What it does |
| ------- | ------------ |
| `pwd` | Show which folder you are in |
| `ls` | List the files in the current folder |
| `cd foldername` | Go into a folder |
| `cd ..` | Go up one folder |
| `cd ~` | Go to your home folder |
| `mkdir foldername` | Create a new folder |

## 1 · Find the full path of ORCA

ORCA should always be started with its **full path** (for example
`/home/yourname/orca/orca`), not just `orca`. Parallel runs do not work otherwise.

The full path is the folder where you unpacked ORCA, followed by `/orca`. If you are
not sure, go to that folder and use `pwd`:

```bash
cd ~/orca        # the folder where you unpacked ORCA
pwd              # prints e.g. /home/yourname/orca  (macOS: /Users/yourname/orca)
```

Test it by running ORCA with its full path and no input file:

```bash
/home/yourname/orca/orca
```

ORCA should print a short message that it needs an input file. If you get
"No such file or directory", the path is wrong.

!!! tip "Shortcut"
    If the ORCA folder is on your `PATH` (see the installation tutorial), `which orca`
    prints the full path, and `$(which orca)` inserts it into a command for you.

## 2 · Go to the folder with your input file

Use a separate folder for each calculation; ORCA writes several files next to the input.

```bash
mkdir -p ~/calcs/n2_sp
cd ~/calcs/n2_sp
```

Copy `n2_sp.inp` into this folder, then check that it is there with `ls`.

## 3 · Run ORCA

```bash
/home/yourname/orca/orca n2_sp.inp > n2_sp.out
```

Replace `/home/yourname/orca/orca` with your full path from step 1. In the exercises
this is written as `/path/to/orca`.

`> n2_sp.out` writes the output into a file instead of onto the screen. The command
returns when the calculation is finished.

??? info "Long calculations"
    To keep a calculation running after you close the terminal, and to see the output
    while it runs:

    ```bash
    nohup /path/to/orca n2_sp.inp > n2_sp.out &
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

## Cores and memory

The exercise inputs run on one core, which is enough for small molecules. For larger
molecules you can tell ORCA to use several cores and how much memory it may use. Add
these two lines to the input, below the `!` line:

```text
%pal nprocs 4 end
%maxcore 2000
```

| Line | Meaning |
| ---- | ------- |
| `%pal nprocs 4 end` | Run on 4 CPU cores |
| `%maxcore 2000` | Memory ORCA may use **per core**, in MB. Here: 4 × 2000 MB = 8 GB in total |

!!! tip "Shortcut: the `PAL` keyword"
    Instead of the `%pal` block you can put the number of cores on the keyword line:
    `PAL4` is the same as `%pal nprocs 4 end`. `PAL2` to `PAL8` (and some larger values,
    e.g. `PAL16`) work this way.

    ```text
    ! B3LYP def2-SVP Opt PAL4
    %maxcore 2000
    ```

How to choose the numbers:

1. **Cores:** at most the number of cores your computer has. Find it with
   `nproc` (Linux/WSL) or `sysctl -n hw.ncpu` (macOS).
2. **Memory:** keep cores × maxcore at about **three quarters of your RAM**, since ORCA
   sometimes uses more than it is given. With 16 GB RAM and 4 cores: `%maxcore 3000`.

!!! warning "Parallel runs need two things"
    - ORCA must be started with its **full path** (step 1).
    - **OpenMPI** must be installed, in the version named in the
      [ORCA installation tutorial](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/install.html).

    Without these, parallel runs fail with an MPI error. Calculations on one core
    work without OpenMPI.

More: [Running a calculation in parallel](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/parallel.html),
[Memory settings](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/memory.html).

## More

- [Your first ORCA calculation](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/first_calc.html)
- [Input and output](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/input_output.html)
- [Graphical user interfaces](https://www.faccts.de/docs/orca/6.1/tutorials/first_steps/GUI.html) for viewing structures and results

# Getting Started: Python, Conda and Jupyter

This guide gets your computer ready for the notebook `xarray_tutorial_waccmx.ipynb`.
You do not need any experience with the terminal or Python. Work through the steps in order.

**How to use this guide:** grey boxes contain commands. Copy one line at a time, paste it into
the terminal (right-click and choose Paste, or press `Ctrl+Shift+V`), then press `Enter`.
A line starting with `#` is a comment: it is there to explain, so you do not need to type it.

To open a terminal on Linux, look for "Terminal" in the applications menu.

---

## 1. What are conda, Miniconda and Anaconda?

- **Python packages** are add-ons that give Python new abilities. For example, `numpy` does
  maths on big arrays, `xarray` reads labelled climate data, `dask` lets xarray open many
  large files at once without loading them into memory, `matplotlib` draws plots and
  `netCDF4` opens the `.nc` files that atmosphere models write.
- **conda** is a program that downloads and installs Python and these packages for you.
- **Miniconda** is a small download that contains conda and Python. **Anaconda** is the same
  thing plus hundreds of extra packages already installed. Either one works.
- An **environment** is a separate, self-contained box holding one version of Python and a set
  of packages. Think of it like a pencil case for one subject: what you put in your physics
  pencil case does not mess up your art pencil case. If something breaks, you can throw that
  one box away and make a new one without affecting anything else.

---

## 2. Install Miniconda

You should be able to access anaconda by running `module load miniforge`. If you can do this, you can skip to step 3.

Otherwise, install your own copy of Miniconda in your home folder (no special permissions needed):

```bash
# Go to your home folder
cd ~

# Download the installer (about 150 MB)
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Run the installer
bash Miniconda3-latest-Linux-x86_64.sh
```

During installation:

1. Press `Enter` to read the licence. Press `q` (or keep pressing space) to get to the end.
2. Type `yes` to accept the licence.
3. Press `Enter` to accept the suggested install location (`~/miniconda3`).
4. When asked whether to initialise conda (`conda init`), type `yes`.

When it finishes, **close the terminal and open a new one.** This lets the terminal find conda.

You can then delete the installer file:

```bash
rm ~/Miniconda3-latest-Linux-x86_64.sh
```

---

## 3. Check that it worked

```bash
conda --version
```

You should see something like `conda 24.x.x`. Any version number means it worked.

You will also notice `(base)` at the start of your terminal prompt, for example:

```
(base) student@machine:~$
```

The word in brackets is the name of the **environment that is currently active**. `base` is the
default one that comes with conda. It is best not to install your own packages into `base`;
we will make a new environment instead.

---

## 4. Create an environment for the tutorial

This one command creates an environment called `xarray_tutorial` and installs everything the
notebook needs. `-c conda-forge` tells conda to use *conda-forge*, a large, free,
community-run collection of packages.

```bash
conda create -n xarray_tutorial -c conda-forge python=3.12 numpy xarray dask netcdf4 matplotlib jupyterlab ipykernel
```

Conda will show a list of packages and ask `Proceed ([y]/n)?`. Type `y` and press `Enter`.
This can take a few minutes.

**Alternative:** the same list of packages is saved in the file `environment.yml` in this
folder. Instead of the long command above you can run:

```bash
cd ~/xarray_tutorial
conda env create -f environment.yml
```

**Activate** (switch on) the environment:

```bash
conda activate xarray_tutorial
```

Your prompt should now start with `(xarray_tutorial)` instead of `(base)`.
You need to activate it **every time you open a new terminal** before working on the tutorial.

To **deactivate** (switch it off and go back to `base`):

```bash
conda deactivate
```

---

## 5. Installing an extra package later

If you need another package, first activate the environment, then install it from conda-forge.
For example, to add `cartopy` (for drawing maps):

```bash
conda activate xarray_tutorial
conda install -c conda-forge cartopy
```

Only if conda says it cannot find a package, use `pip` (Python's own installer) instead,
with the environment still active:

```bash
pip install name-of-package
```

---

## 6. Useful conda commands

| Command | What it does |
|---|---|
| `conda env list` | List all your environments (the active one has a `*`) |
| `conda list` | List the packages in the active environment |
| `conda activate NAME` | Switch to environment `NAME` |
| `conda deactivate` | Switch off the current environment |
| `conda remove -n NAME --all` | Delete environment `NAME` completely |

For example, to delete the tutorial environment and start again from step 4:

```bash
conda deactivate
conda remove -n xarray_tutorial --all
```

---

## 7. Start Jupyter Lab and open the notebook

Jupyter Lab is a program that runs in your web browser and lets you run Python code in small
pieces called **cells**.

```bash
conda activate xarray_tutorial
cd ~/xarray_tutorial
jupyter lab
```

A browser window should open by itself. If it does not, the terminal will print a web address
starting with `http://localhost:8888/...`: hold `Ctrl` and click it, or copy it into your browser.
**Leave the terminal open** while you work; closing it stops Jupyter.

Then:

1. In the file list on the left, double-click `xarray_tutorial_waccmx.ipynb`.
2. Check the **kernel** (the Python that runs your code). It is shown in the top-right corner of
   the notebook. If asked to choose one, pick **Python 3 (ipykernel)**. Because you started
   Jupyter from inside the `xarray_tutorial` environment, this is the right one.
3. Click on a cell and press **`Shift+Enter`** to run it and move to the next one.
   Run the cells in order, from the top.

When you are finished, save the notebook (`Ctrl+S`), close the browser tab, go back to the
terminal and press `Ctrl+C` (then `y` if asked) to stop Jupyter.

**Using VS Code instead?** Open the notebook in VS Code, click **Select Kernel** in the top-right
corner, choose **Python Environments**, and pick the one called `xarray_tutorial`.
(You may need to install the "Python" and "Jupyter" extensions in VS Code first.)

---

## 8. Troubleshooting

**`conda: command not found`**
The terminal does not know about conda yet. Close the terminal and open a new one. If it still
fails, run the line below, then close and reopen the terminal again:

```bash
~/miniconda3/bin/conda init
```

(If you are using the shared Leeds Anaconda, run `module load anaconda` first instead.)

**`ModuleNotFoundError: No module named 'xarray'`** (or `numpy`, `netCDF4`, ...)
Python is running in the wrong environment. In the terminal, check your prompt says
`(xarray_tutorial)`; if not, run `conda activate xarray_tutorial` and start `jupyter lab` again.
In the notebook, check the kernel in the top-right corner. If the package really is missing,
install it as shown in step 5.

**Conda is stuck on "Solving environment..."**
Working out which package versions fit together can take several minutes. Be patient. Make sure
you included `-c conda-forge` in the command, and avoid mixing packages from different channels.
If it is still stuck after about 15 minutes, press `Ctrl+C`, delete the environment (step 6)
and try the `conda create` command again.

**Still stuck?** Copy the full error message and send it to your supervisor. The exact wording
of the error is very helpful.

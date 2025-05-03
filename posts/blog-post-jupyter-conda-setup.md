# Fixing Jupyter Kernel Issues for Python and R in HPC Cluster Environment


## 🧩 Introduction

When working with Jupyter notebooks in a cluster environment, managing Conda environments and registering Python and R kernels can be tricky. Here's a practical guide based on my experience solving kernel conflicts and making both Python and R work seamlessly.

## 🛠️ Environment Setup

### Create a Conda Environment

```bash
conda create -n conda_env python=3.11 r-base=4.4 -y
conda activate conda_env
```

### Install Python Kernel

```bash
python -m ipykernel install --user --name conda_env --display-name "Python_conda_env"
```

Check if the kernel is registered:

```bash
jupyter kernelspec list
```

## 📦 Install Required Packages

Make sure the required Jupyter components are installed:

```bash
jupyter --version
```

If any key packages (like `notebook`, `jupyterlab`, etc.) are missing, install them:

```bash
conda install notebook jupyterlab ipykernel jupyter_client jupyter_core
```

## 🧪 Install and Register R Kernel

Check if R is installed:

```bash
conda list r-base
```

If installed, run:

```bash
R -e "IRkernel::installspec(user = TRUE, name = 'conda_env_r', displayname = 'R_conda_env')"
```

## 🔍 Verify Kernels were added

You should now see both Python and R kernels registered under `~/.local/share/jupyter/kernels/`.

```bash
# List all registered Jupyter kernels
jupyter kernelspec list
```
Each registered Jupyter kernel has a kernel.json file that defines how Jupyter launches it. To inspect and verify it:

```bash
# Check the kernel.json file to make sure it's pointing to the correct Python or R executable path
cat ~/.local/share/jupyter/kernels/conda_env/kernel.json
```
You should see something like:

```json
{
  "argv": [
    "/home/yourusername/.conda/envs/conda_env/bin/python",
    "-m",
    "ipykernel_launcher",
    "-f",
    "{connection_file}"
  ],
  "display_name": "Python_conda_env",
  "language": "python"
}

```

## 🧹 Kernel Conflicts and Fixes

### Remove Conflicting Kernels

If a kernel is broken or duplicated, unregister it:

```bash
jupyter kernelspec list
jupyter kernelspec uninstall conda_env_broken
# or
jupyter kernelspec remove conda_env_broken
```

### Fix R Library Path Issues

Sometimes R in conda conflicts with cluster R. Make sure your environment points to the right libraries:

```bash
export LD_LIBRARY_PATH=$HOME/.conda/envs/conda_env/lib:$LD_LIBRARY_PATH
```

## ✅ Final Check

To confirm that both kernels are ready:

```bash
jupyter kernelspec list
```

## ✅ Summary

```bash
# Add Python Kernel
conda activate conda_env
python -m ipykernel install --user --name conda_env --display-name "Python_conda_env"
cat ~/.local/share/jupyter/kernels/conda_env/kernel.json

# Check if R is installed
conda list r-base

# Register R Kernel
R -e "IRkernel::installspec(user = TRUE, name = 'conda_env_r', displayname = 'R_conda_env')"
```


---

## ✅ Summary of Actions

- Installed Jupyter and required kernels  
- Registered Python and R kernels  
- Resolved R conflicts using `LD_LIBRARY_PATH`  
- Cleaned up unwanted or broken kernels  
- Verified kernel registration with `jupyter kernelspec list`  

---

**✍️ Written by:** Eman  

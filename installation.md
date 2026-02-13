# Installation Instruction

Start by cloning the repo:
```bash
git clone --recursive git@github.com:pengsongyou/openscene.git
cd openscene
```

You need [conda](https://docs.conda.io/en/latest/miniconda.html) (e.g. Miniconda). **Without sudo**, install Miniconda in your home directory and add it to your `PATH` (see [Miniconda](https://docs.conda.io/en/latest/miniconda.html)), then continue below.

---

## Recommended setup (no sudo, Python 3.10, PyTorch 2.1 + CUDA 12.1)

**Step 0: Create conda environment (Python 3.10)**

```bash
conda create -n openscene python=3.10 -y
conda activate openscene
```

Without sudo, get OpenEXR and build tools from conda (skip if you already have them):

```bash
conda install -c conda-forge openexr -y
conda install -c conda-forge cxx-compiler -y
```

**Step 1: Install PyTorch 2.1.0 (CUDA 12.1)**

Install PyTorch first so other packages do not override it:

```bash
pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 --index-url https://download.pytorch.org/whl/cu121
```

Pin NumPy to 1.x (PyTorch 2.1 and MinkowskiEngine can crash with NumPy 2.x):

```bash
pip install "numpy<2"
```

**Step 2: Install MinkowskiEngine**

Without sudo, use conda for OpenBLAS. Install ninja for faster build, then MinkowskiEngine:

```bash
conda install openblas-devel -c anaconda -y
pip install ninja
pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps
```

If you hit build errors, set `MAX_JOBS=1` or see the [MinkowskiEngine installation page](https://github.com/NVIDIA/MinkowskiEngine#installation).

**Step 3: Install remaining dependencies**

```bash
pip install -r requirements.txt
```

**Step 4 (optional):** For multi-view feature fusion with OpenSeg:

```bash
pip install tensorflow
```

---

### Package upgrade / conflict notes

- **PyTorch**: Installed in Step 1 (`torch==2.1.0`, `torchvision==0.16.0`, `torchaudio==2.1.0`). Installing these first avoids `requirements.txt` or other pip installs from downgrading/upgrading them. If you later run `pip install <package>` and it tries to change PyTorch, either skip that dependency for that package or re-run the Step 1 command.
- **open3d**: Works with PyTorch 2.x; `pip install -r requirements.txt` may upgrade it; usually no conflict.
- **opencv-python**: Compatible with PyTorch 2.1; no change needed.
- **CLIP** (from `requirements.txt`): Installed from GitHub; if you see import or version errors with PyTorch 2.1, consider pinning a compatible commit or using a PyTorch 2–compatible fork.
- **MinkowskiEngine**: Built against the current PyTorch and CUDA in the env; do not upgrade/downgrade PyTorch after installing it without reinstalling MinkowskiEngine.
- **NumPy**: Keep `numpy<2` so PyTorch and MinkowskiEngine do not hit "NumPy 1.x vs 2.x" runtime errors. If you already have NumPy 2.x, run `pip install "numpy<2"` before building MinkowskiEngine.

---

<!-- PREVIOUS INSTRUCTIONS (COMMENTED OUT)
First of all, you have to make sure that you have all dependencies in place.
The simplest way to do so, is to use [anaconda](https://www.anaconda.com/).

You can create an anaconda environment called `openscene` as below. For linux, you need to install `libopenexr-dev` before creating the environment.

```bash
sudo apt-get install libopenexr-dev # for linux
conda create -n openscene python=3.8
conda activate openscene
```

Step 1: install PyTorch (we tested on 1.7.1, but the following versions should also work):

```bash
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
```

Step 2: install MinkowskiNet:

```bash
sudo apt install build-essential python3-dev libopenblas-dev
```
If you do not have sudo right, try the following:
```
conda install openblas-devel -c anaconda
```
And now install MinkowskiNet:
```
pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps \
                           --install-option="--force_cuda" \
                           --install-option="--blas=openblas"
```
If it is still giving you error, please refer to their [official installation page](https://github.com/NVIDIA/MinkowskiEngine#installation).


Step 3: install all the remaining dependencies:
```bash
pip install -r requirements.txt
```

Step 4 (optional): if you need to run multi-view feature fusion with OpenSeg (especially for your own dataset), remember to install:
```bash
pip install tensorflow
```
END PREVIOUS INSTRUCTIONS -->

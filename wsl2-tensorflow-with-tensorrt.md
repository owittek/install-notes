# Installation of Tensorflow in WSL2 (Arch) with with Tensorrt

## Requirements

- WSL2 (Use whatever)
- Nvidia GPU drivers
- [Micromamba](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html) / [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

## Versions (19.02.2023)

- tensorflow: 2.11.0
- cudnn: 8.4.1.50
- cudatoolkit: 11.8.0
- tensorrt: 8.5.3.1

## Installation (Example with Micromamba & zsh)

1. Create .mambarc / .condarc in $HOME 

Required if you don't want to type '-c conda-forge' during installation

```zsh
echo "channels:\n  - conda-forge" > $HOME/.mambarc
```

2. Create tf environment & complete conda-forge installs

```zsh
micromamba env create --name tf python=3.10
micromamba activate tf
micromamba install cudatoolkit cudnn
```

3. Set LD_LIBRARY_PATH

The first export prevents you from having to exit and re-enter the environment

```zsh
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

4. Pip installations

```zsh
pip install tensorflow
pip install tensorrt
```

5. Symlink tensorrt libs to $CONDA_PREFIX/lib/

```zsh
cd $CONDA_PREFIX/lib
ln -s $CONDA_PREFIX/lib/python3.1/site-packages/tensorrt/libnvinfer.so.8 libnvinfer.so.7
ln -s $CONDA_PREFIX/lib/python3.1/site-packages/tensorrt/libnvinfer_plugin.so.8 libnvinfer_plugin.so.7
```

### Done!

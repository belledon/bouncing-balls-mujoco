# Bouncing-ball intuitive physics

A Jupyter notebook that infers the physical properties of a bouncing ball from noisy observations of its position. It pairs a [Gen](https://www.gen.dev/) generative model with the [MuJoCo](https://mujoco.readthedocs.io/) physics engine and runs particle-filter inference over the ball's bounciness and mass.

## Requirements

- **Julia 1.10 or newer.** Tested on 1.10, 1.11, 1.12 and 1.13, on macOS (Apple silicon) and Linux (x86_64).
- **Jupyter**, either JupyterLab/Notebook or the VS Code Jupyter extension.

No Python packages and no GPU are needed.

### Installing Julia and Jupyter with Homebrew

On macOS, both come from Homebrew:

```bash
brew install julia jupyterlab
```

## Install

Register a Julia kernel with Jupyter. This is a one-time, per-machine step, and is separate from the notebook's own environment:

```bash
julia -e 'using Pkg; Pkg.add("IJulia"); using IJulia; installkernel("Julia")'
```

Then, from the repository directory, install the notebook's dependencies:

```bash
julia --project=. -e 'using Pkg; Pkg.instantiate()'
```

This step is optional, because the notebook's first cell runs the same thing, but doing it here keeps the first notebook run from sitting silently for several minutes. Expect it to take a few minutes and roughly 1 GB of disk on a fresh machine, most of it precompilation.

## Run

```bash
jupyter lab bouncing-balls-mujoco.ipynb
```

If you don't have Jupyter installed, IJulia can install and launch one for you:

```bash
julia -e 'using IJulia; notebook(dir=pwd())'
```

Then run the cells top to bottom. The first cell activates the environment in this directory, so the notebook works regardless of which directory Jupyter was started from.

## About the environment files

`Project.toml` lists the dependencies and their compatible versions.

`Manifest-v1.12.toml` pins exact versions, and is only used by Julia 1.12. On any other version, Julia ignores it and resolves versions from `Project.toml` instead, writing a local `Manifest.toml`, which is git-ignored. In testing, 1.10, 1.11 and 1.13 resolved to the same package versions as the pinned manifest.

## Troubleshooting

**Jupyter reports a missing kernel, or the kernel fails to start.** The notebook asks for a kernel named `julia-1.12`. On other Julia versions, pick your Julia kernel from the kernel menu. If no Julia kernel appears, or it fails with a `No such file or directory` error pointing at a Julia binary, the registered kernel is stale, for instance after a Julia upgrade. Re-run the `installkernel` command above and restart Jupyter.

**`Pkg` reports an unregistered package or a local `path`.** Check `git status` for changes to `Project.toml`. A `startup.jl` that adds packages to the active project will modify this one whenever the notebook's kernel starts. Revert with `git checkout Project.toml Manifest-v1.12.toml`.

# Add new package to conda-forge


1. Fork `conda-forge/staged-recipes` to your own repository: https://github.com/conda-forge/staged-recipes.  
A repository exists at: [Github brsynth](https://github.com/brsynth/staged-recipes).  
This repository will be used later.

2. Clone your forked repo:

    ```bash
    git clone git@github.com:brsynth/staged-recipes.git
    ```

3. Update and synchronize the repo:

    ```bash
        git checkout master
        git remote add upstream git@github.com:conda-forge/staged-recipes.git
        git pull --rebase upstream main
        git push origin master --force
    ```

4. Create a new branch from the staged-recipes master branch:

    ```bash
        git checkout -b <branch-name>
    ```
Branch name can follow this convention: `<package_name>_condaforge`

5. Create a folder + meta file in : `recipes/package_name/meta.yaml`

    Example of recipe: https://github.com/brsynth/staged-recipes/blob/rpbasicdesign_condaforge/recipes/rpbasicdesign/meta.yaml

    Example for generating sha256 code for a package:

    First, the package need to be tagged with the corresponding version.

    ```bash
    wget -O- https://github.com/brsynth/package_name/archive/0.3.1.tar.gz | shasum -a 256
    ```

    conda-forge recommand to use `noarch: python` if it's possible because it's produce only one build that can be served on all platforms for all Python versions. To make it work, you need to add constraints for python version (ex: `python >=3.6`) in the `host` and `run` section.

6. Submit changes:

    ```bash
    git add recipes/package_name/meta.yaml
    git commit -m "Add recipe for package 0.3.1"
    git push origin -u package_condaforge
    ```

7. Creating a pull request:

    Git will give you a url for creating the pull request, which can be clicked to be taken to github. You can write a short description of the recipe.

    Wait until your pull request passed all checks in github. Then, you need to wait a reviewer. If it takes so long (few days), you can ping @conda-forge/staged-recipes or bring the pull request up in their Gitter chat room at https://gitter.im/conda-forge/conda-forge.github.io.

    Finally the recipe will be merged and published in conda-forge in https://anaconda.org/

8. Test locally for staged recipes:

    Docker and shyaml must be installed on your system.

    You need to define an environment variable named CONFIG. Its value must be the name of one of the three YAML configuration files in the .ci_support directory (either linux64, osx64, or win64).

    ```bash
    cd staged-recipes
    CONFIG=linux64 ./.scripts/run_docker_build.sh
    ```


## Maintain a package already added to conda-forge:


Once the package is available in conda-forge, you will receive an email that you’ve been automatically subscribed to a repository on GitHub `conda-forge/package_name-feedstock`.

All maintainers are given push access to the feedstocks that they maintain.

Example: https://github.com/conda-forge/rpbasicdesign-feedstock

* Forking the feedstock:

Fork https://github.com/conda-forge/package_name-feedstock

* Clone your feedstock:

```bash
git clone https://github.com/<your-github-id>/<feedstock>.
```

* Syncing your fork with conda-forges feedstock:

```bash
git checkout master
git remote add upstream https://github.com/conda-forge/<feedstock>
git fetch upstream
git rebase upstream/master
```

* Creating your changes in a new branch:

```bash
git checkout -b <branch-name>
```

Make your changes in the recipe folder.

```bash
git add <changed-files>
git commit -m <commit-msg>
git push origin <branch-name>
```

* Propose a pull request:

Git will give you a url for creating the pull request, which can be clicked to be taken to github.

* Testing changes locally:

Docker and shyaml need to be installed on your system.

To build and test updates, go to the root feedstock directory and run:

```bash
python build-locally.py
```

You can specify which config to use, see `.ci_support/linux_python3.6.yaml`:

```bash
python build-locally.py linux_python3.6
```

For more informations about maintaining and updating packages, see: https://conda-forge.org/docs/maintainer/updating_pkgs.html

## Credit

- Kenza Bazi-Kabbaj

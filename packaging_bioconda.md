# Add new package to bioconda

1. Fork `bioconda/bioconda-recipes` to your own repository: https://github.com/bioconda/bioconda-recipes. This action was already done and must be made only once.

2. Clone your forked repo:

    ```bash
    git clone git@github.com:brsynth/bioconda-recipes.git
    ```

3. Update and synchronize the repo if needed:

    ```bash
        git checkout master
        git remote add upstream git@github.com:bioconda/bioconda-recipes.git
        git pull --rebase upstream master
        git push origin master --force
    ```

4. Create a new branch from the bioconda-recipes master branch:

    ```bash
        git checkout -b <branch-name>
    ```

5. Create a folder + meta file recipe in : `bioconda-recipes/recipes/package_name/meta.yaml`

    Example of recipe: https://github.com/brsynth/bioconda-recipes/blob/liv_utils_bioconda/recipes/liv_utils/meta.yaml

    **NOTE** : During a Bioconda package build, the stringent `mulled-build` test runs the tests listed in the `test` section of the recipe’s `meta.yaml` file in an environment which does not have the dependencies listed in the `test: requires:` section. Because of this, all tests in that section must only rely on runtime dependencies or the build will fail. Tests that rely on the test-time dependencies listed in `test: requires:` should be put in the `run_test.sh` file in the same directory as the `meta.yaml` file. This file is picked up and run by the standard build that occurs before the `mulled-build`, but is not passed on to and run by the `mulled-build`.

    Example of `run_test.sh` : https://github.com/brsynth/bioconda-recipes/blob/liv_utils_bioconda/recipes/liv_utils/run_test.sh

    Example for generating sha256 code for a package:

    First, the package need to be tagged with the corresponding version.

        ```bash
        wget -O- https://github.com/brsynth/package_name/archive/0.3.1.tar.gz | shasum -a 256
        ```

    **NOTE** : The `bioconda channel` is a Conda channel providing bioinformatics related packages for `Linux` and `Mac OS` only.

6. Submit changes:

    ```bash
    git add recipes/package_name/meta.yaml
    git commit -m "Add recipe for package 0.3.1"
    git push origin -u branch_name
    ```

7. Creating a pull request:

    Git will give you a url for creating the pull request, which can be clicked to be taken to github. You can write a short description of the recipe.

    Once your PR is passing tests and ready to be merged, please issue the `@BiocondaBot please add label` command.

    If it takes so long (few days), you can bring the pull request up in their Gitter chat room at https://gitter.im/bioconda/.

8. Testing your recipe locally:

    Using the `Circle CI Client`:

    First, install `circleci` locally: https://circleci.com/docs/2.0/local-cli/

        ```bash
        # Ensure the build container is up-to-date
        docker pull quay.io/bioconda/bioconda-utils-build-env:latest

        # Run the build locally from bioconda-recipes directory
        circleci build
        ```

    See https://bioconda.github.io/contributor/building-locally.html for more tests possibilities.

## Maintain a package already added to bioconda:

Bioconda has the ability to automatically update packages and submit a PR on your behalf. Note that the auto-updater does not yet know how to monitor changed dependencies, so it is important to verify that the updated recipe reflects all the changes. If you have to update dependencies, then you need to proceed this way:

1. Creating your changes in a new branch:

    ```bash
    git checkout -b <branch-name>
    ```
2. Make your changes in `recipes/package_name/` folder

3. Submit changes:

    ```bash
    git add recipes/package_name/meta.yaml
    git commit -m "Update recipe for package 0.3.1"
    git push origin -u branch_name
    ```
    **NOTE** : Make sure to mention "Update" in the commit message.

4. Creating a pull request:

    Git will give you a url for creating the pull request, which can be clicked to be taken to github. You can write a short description of the recipe.

    Once your PR is passing tests and ready to be merged, please issue the `@BiocondaBot please add label` command.

    If it takes so long (few days), you can bring the pull request up in their Gitter chat room at https://gitter.im/bioconda/.

Another way is to update your package with `bioconda-utils`:

* Install and update recipe with bioconda-utils

```bash
conda install -c conda-forge -c bioconda bioconda-utils
```

**NOTE** : Use mamba instead of conda if you have troubles with bioconda-utils installation.

```bash
# From bioconda-recipes directory
bioconda-utils autobump recipes/ config.yml --packages <my-package-name>
```

See https://bioconda.github.io/contributor/updating.html for more informations.

## Credit

- Kenza Bazi-Kabbaj

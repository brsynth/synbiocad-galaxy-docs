# PLANEMO

Command-line utilities to assist in developing Galaxy and Common Workflow Language artifacts - including tools, workflows, and training materials.


## Planemo installation

```bash
python3 -m venv planemo
. planemo/bin/activate
pip install -U git+git://github.com/galaxyproject/planemo.git
```

## Planemo utilities

### Lint the developed tool for XML validity

```bash
planemo lint wrap.xml --fail_level error
```

### Run specified tool’s tests within Galaxy

First, put your data tests in `/galaxy/test-data` directory. You can test your tool with `--biocontainers` option (recommended by Galaxy IUC community) if you have docker installed in your Galaxy environment. If not, you can ignore this option.

```bash
planemo test TOOL_PATH --biocontainers
```

Planemo will output an html test summary `tool_test_output.html`.

See : https://planemo.readthedocs.io/en/latest/commands/test.html for more informations.

You can also specify the test data directory directly:

```bash
planemo test TOOL_PATH --biocontainers --test_data DIRECTORY_DATA_TEST
```

### Publishing in the toolshed

- **Configure the target Tool Shed**

Edit (/root/.planemo.yml) file (generated with config_init) to configure the account by providing the key or shed username and password.

```bash
planemo config_init
```

```bash
## Planemo Global Configuration File.
## Everything in this file is completely optional - these values can all be
## configured via command line options for the corresponding commands.
## [...]
sheds:
     toolshed:
           key: "xxxxxxxxxmytoolshedAPIkeyxxxxxxx"
           #email: "<TODO>"
           #password: "<TODO>"
     testtoolshed:
           #key: "<TODO>"
           #email: "<TODO>"
           #password: "<TODO>"
     local:
           #key: "<TODO>"
           #email: "<TODO>"
           #password: "<TODO>"
```

- **Configure the repository**

From a **directory** containing tools (wrap.xml), test-data directory, etc… the **shed_init** command can be used to bootstrap a new **.shed.yml** file.

```bash
planemo shed_init --name=<name>
                  --owner=<shed_username>
                  --description=<short description>
                  [--remote_repository_url=<URL to .shed.yml on github>]
                  [--homepage_url=<Homepage for tool.>]
                  [--long_description=<long description>]
                  [--category=<category name>]*
```

Repo example: https://github.com/galaxyproject/planemo/tree/master/tests/data/repos/multi_repos_flat_configured

Shed_yml example: https://galaxy-iuc-standards.readthedocs.io/en/latest/best_practices/shed_yml.html

- **Check**

```bash
planemo shed_lint --tools --urls --ensure_metadata --fail_level error
```

- **Create repository**

```bash
#--shed_target : default is main toolshed.
planemo shed_create --shed_target testtoolshed
```

You can check the uploaded repository in Galaxy testtoolshed: https://testtoolshed.g2.bx.psu.edu/ or in Galaxy main toolshed: https://toolshed.g2.bx.psu.edu/. It's recommended to start with the testtoolshed before publishing in the main toolshed.

- **Updating a Repository**

If modifications are required these can be **reviewed** using the **shed_diff** command:

```bash
planemo shed_diff --shed_target testtoolshed
```

Then, update the repo with **shed_update**:

```bash
planemo shed_update --check_diff --shed_target testtoolshed
```

- **Testing the repository**

Once tools and required dependency files have been published to the tool shed, the actual shed dependencies can be automatically and installed and tool tests ran using the command:

```bash
planemo shed_test --shed_target testtoolshed
```

Make sure the test toolshed is enabled is Galaxy config file: config/tool_sheds_conf.xml

See these links for more informations:

https://galaxyproject.org/toolshed/publish-tool/

https://galaxy-iuc-standards.readthedocs.io/en/latest/best_practices.html

https://galaxy-iuc-standards.readthedocs.io/en/latest/best_practices/integration_checklist.html

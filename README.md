# sm-ansible-action
Ansible action that caches requirements using virtualenv and prepare for private repo login

## Usage

You can use the [SM Ansible GitHub Action](https://github.com/searchmetrics/sm-ansible-action) in a [GitHub Actions Workflow](https://help.github.com/en/articles/about-github-actions) by configuring a YAML-based workflow file, e.g. `.github/workflows/ansible.yml`, with the following:

```yaml
name: ansible
on: [push]

jobs:
  test_action:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - run: python3 --version

      - name: ansible-action
        id: test-action
        uses: actions/searchmetrics/sm-ansible-action@v0.0.1
        with:
          python-version: '3.9'
          python-requirements: ./requirements.txt
          netrc-token: ${{ secrets.GITHUB_TOKEN }}

      - name: testing ansible bin output
        run: ${{ steps.test-action.outputs.ansible-bin }} --version
      ## You can also use virtual env
      - name: activating venv
        run: source ${{ steps.test-action.outputs.activate-script }}

      - name: testing separate commands ansible
        run: ansible --version

      - name: testing ansible galaxy install
        run: ansible-galaxy collection install community.kubernetes
        
      - name: testing ansible galaxy install
        run: ansible-playbook super-play.yml
      
      - name: deactivate venv
        run: deactivate
```
## Inputs

This action supports the following inputs:

| Key                        | Required | Description                                                                                                                                                                |
| -------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `python-version`                 | Optional | python version for setup-python action. Defaults to 3.9                                                            |
| `netrc-token`        | Optional | netrc token for private repos: Defaults to none                           |
| `python-requirements`            | Optional | requirements file to install ansible and other libs. Defaults to requirements.txt from the action root |path`.                                                                                     |

## Exported vars
All variables from Setup-python action plus:


ANSIBLE_PYTHON_INTERPRETER=python3

## Outputs

This action supports the following inputs:

| Key                        |  Description |
| -------------------------- | --------------------------- |
| `venv-dir`                 |  virtualenv dir |
| `ansible-bin`        |  virtualenv ansible bin |
| `ansible-playbook-bin`        |  virtualenv ansible playbook bin |
| `ansible-lint-bin`            |  virtualenv ansible-lint bin | 
| `ansible-galaxy-bin`                 |  virtualenv ansible-galaxy bin |
| `pip-bin`        |  virtualenv pip bin  |
| `activate-script`            |  virtualenv activate script` |

## Todo
* Review doc
* Improve netrc configuration to avoid other git operations issues
  
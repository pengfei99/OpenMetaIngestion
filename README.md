# OpenMetaIngestion

In OpenMetadata, `everything is handled via the API`, and `Data structures (Entity definitions) are at the heart` of the solution.
OpenMetadata provides three api:
- python
- Java
- Go

In this repo, we will focus on the `Python API`

## 1. Install the dev env

### Step1 Install pyenv

```shell
# install dependencies to run the auto installation script
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python3-openssl git 

# download the script and run it
curl https://pyenv.run | bash

# after the above command, you need to add the env var in the output of the above command in to your .bashrc
# below is an example
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

# you need to reload your bashrc or restart your shell
source ~/.bashrc

# show the pyenv version
pyenv versions

# list all available version that can be installed  
pyenv install --list

# install a specific version
pyenv install 3.11

# activate a python version as the current python, there are two mode: global and local
pyenv global 3.11
pyenv local 3.11

# after changing the python version, you can test it with
python -V
```

### Create a pyenv-virtualenv

```shell
# create a virtual env
python -m venv $VENV_PATH

# activate virtual env
source $VENV_PATH/bin/activate

# deactivate
deactivate
```

## 2. Install the OM ingestion python sdk

```shell
pip install "openmetadata-ingestion~=1.4.0.1"
```

## 3. Basic terms

You can get a full list of the terms which open metadata uses [here](https://docs.open-metadata.org/v1.4.x/main-concepts/metadata-standard/schemas/entity)

To fully understand the ingestion process, we need to understand some basic terms: 

- Entity: A basic unit to describe a dataset(e.g. table, database, etc.). They can have hierarchy relation between them.
               For example, to create a table, we must follow the hierarchy `DatabaseService -> Database -> Schema -> Table`.

- Entity type: https://docs.open-metadata.org/v1.4.x/main-concepts/metadata-standard/schemas/entity/type


### 2.1 A real word example

Imagine we have a `database server` called `server1` which contains `four databases`, in the database `db1`, we 
have a schema called `schema1`, in `schema1`, we have four tables `tab1,tab2,tab3,tab4`. 

To create this, we need to create an entity `server1` of type `DatabaseService` which contains an entity `db1` of 
type `Database`, which contains an entity `schema1` of type `Schema`, which contains four entity `tab1,tab2,tab3,tab4`
of type `Table`.

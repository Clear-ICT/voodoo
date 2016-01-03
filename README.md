## Welcome to Voodoo

Some black magic for your Odoo projects! With Voodoo, it is you who controls the Odoo, not the reverse ;-)

[![Voodoo by Akretion](https://s3.amazonaws.com/akretion/assets/voodoo.png)](http://akretion.com)

### Install on Ubuntu

#### Prerequisite

You need a 64 bits architecture


#### Install or upgrade your Docker:

The official docker documentation proposes the following script to install the last version of docker on Ubuntu

```
wget -qO- https://get.docker.com/ | sh
```
You can have more information on the installation on https://docs.docker.com/v1.6/installation/ubuntulinux/
in the section Docker-maintained Package Installation, please do not forget to install the latest version if
you choose to install it manually


* Give non root access to Docker if not done already:
```
  sudo gpasswd -a ${USER} docker
  sudo service docker restart # use docker.io instead of docker in Ubuntu 14.04
```

**you may have to LOGOUT and LOG BACK IN AGAIN for the group change to take effect!**

* A simple test to see if you have non root access is to list your docker images with
```
   docker images # make sure you don't need sudo anymore here
```


#### Install Voodoo:

```
  sudo pip install git+https://github.com/akretion/voodoo.git --upgrade
```


###  Quick Start

Create a new project

```
    voodoo new my_project
```

Enter in the project and run voodoo

```
   cd my_project
   voodoo run
```

A new docker with odoo inside have been launch. Now let's start Odoo

First run ak build to build you project with anybox buildout recipe
```
   ak build
```

Then launch ak run to start odoo
```
   ak run
```

Go to http://localhost:8069 Odoo is here !


### Voodoo Command

Voodoo extends docker-compose so it can do everything docker-compose can do. For more information about extra commands you can read the docker-compose documentation: http://docs.docker.com/compose


#### voodoo new
will start a new custom project by cloning a default project template.
The most important files are buildout.cfg, buildout.dev.cfg, voodoo.yml
As you can guess Voodoo is based on Anybox Recipe you can read all the documentation here [Anybox recipe documentation](http://pythonhosted.org/anybox.recipe.openerp/)

The voodoo.yml is like the docker-compose.yml from docker-compose. For more information read the docker-compose documentation and read the section voodoo.yml

#### voodoo run
will run your project and you will have a bash session directly in the odoo container

#### voodoo open
will open a new session to an existing running project. For example you may need on terminal for running odoo and a second terminal for doing some psql. 
Use: open a new terminal tab/window from your voodoo running session (you're now in the same folder), then just run "voodoo open":  you have a new session inside the container. You can open as many voodoo tab that you want for your convenience.

#### voodoo service [add...] SERVICE_NAME
will modify the voodoo.yml file of the project and link the odoo container to the new service. The service has to be set up in the service.yml file of this module

#### List of all command

```
    Usage:
      voodoo [options] [COMMAND] [ARGS...]
      voodoo -h|--help

    Options:
      --verbose                 Show more output
      --version                 Print version and exit
      -f, --file FILE           Specify an alternate compose file
                                (default: voodoo.yml)
      -p, --project-name NAME   Specify an alternate project name
                                (default: directory name)

    Commands:
      build     Build or rebuild services
      help      Get help on a command
      kill      Kill containers
      logs      View output from containers
      new       Create a new project
      open      Open a new session inside the docker
      port      Print the public port for a port binding
      ps        List containers
      pull      Pulls service images
      rm        Remove stopped containers
      run       Run a one-off command
      scale     Set number of containers for a service
      start     Start services
      stop      Stop services
      restart   Restart services
      up        Create and start containers
      chefdk    Chef Development Kit wrapper
```


### ak Command

TODO improve me

```
    Usage:
      ak [COMMAND] [ARGS...]
      ak --help

    Commands:
      run           Run Odoo
      debug         Run Odoo in debug mode
      build         Build Your Project using frozen.conf if existing or buildout.dev.cfg
                    If you have an existing frozen.cfg and you want to force the update
                    just add --update as arguments "ak build --update"
      freeze        Freeze all dependency for your project
      console       Open a Odoo Shell
      code_test     Run flake8 and pylint tests on the module specified
                    If no modules provided, test all files in 'modules' directory
      server_test   Start Odoo with test enables (that launch unittest and yaml)
      psql          Open a Psql shell
      load          Load a database from a file args. ak load [file] [dbname]
      reload        Drop and load a database from a file args. ak reload [file] [dbname]
      dump          Dump a database ak dump [file] [dbname]
      help          Get help on a command

   Args:
      -o            ak build option to build in offline mode
```


### Voodoo.yml

For configuring your voodoo you have two file.
One in ~/.voodoo/config.yml and one in your project voodoo.yml

#### Config.yml
For now there is two option for you default config

shared_eggs [True or False]: If true the ~/.voodoo/shared/eggs will be mounted as eggs folder in your docker
This mean that the eggs will be shared between you voodoo project saving some download and space

shared_odoo [string or False]: If a string is set as shared odoo git repo will be created in ~/.voodoo/shared/shared_odoo/your_string. This give you the posibility to share an odoo git repository between some container. If False an new odoo repository will be created.

odoo_repo_list [dict]: {'repo_name': 'repo_location'} : The list of the odoo repositories that can be used. Indeed you maybe want to use OCA repo for a project and the Odoo S.A. repo for another or even a specific repo. Ex: {'oca': 'https://github.com/oca/ocb.git'}

Note : to improve the performance when downloading odoo, an odoo project is downloaded in ~/.voodoo/shared/odoo. Then when you start a new project the new odoo part is created from this local repository.


Options 'shared_eggs' and 'shared_odoo' can be overrided in the project voodoo.yml. For that you just have to set those options in the voodoo section.
For the repository you want to use, add:
    used_odoo_repo: 'name_of_the_repo'
The name of the repo has to be in the odoo repo list in the file config.yml.

#### Service.yml

list of the available services that can be linked with odoo container. Defines also the default parameters of thoses services.

### FAQ

see wiki https://github.com/akretion/voodoo-cli/wiki


### Other apps available
pgcli

[![Pgcli](https://github.com/amjith/pgcli/raw/master/screenshots/image02.png)](https://github.com/amjith/pgcli)


### Usages

So several usages are possible: from the Odoo project repository to a complete development server for your platform. Please read the section that matches your use case!

## Use as a simple Odoo project repository managed by Anybox recipe (without using Docker)

You can clone a voodoo branch to start your project as simple convenience repo for your project. With the buildout.cfg file you can pin exactly your shared branches dependencies. You also keep the project specific modules under revision control in the modules folder.

For further details, please simply refer to [Anybox recipe documentation](http://docs.anybox.fr/anybox.recipe.openerp/trunk/)


Note that the Docker workdir is your repo that is shared with Docker, so you won't loose your source changes nor loose time copying files.

Your databases are also persisted in your repo folder in the .db hidden folder. But you can always trash all project databases by simply removing that folder.

## Roadmap
- udpate/fix documentation
- add pre-copy of odoo repo in background for generating quickly a new project (mv .odoocache)
- review voodoo new (maybe it will better to have a template folder un voodoo cli)
- share vim info...
- generate automatically ssh key if missing
- debian package
- add bash completion
- add dynamic port

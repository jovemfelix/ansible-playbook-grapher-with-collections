# Ansible Collections Gaphical using ansible-playbook-grapher

## Steps to Reproduce

### Creating a collection skeleton

```shell
ansible-galaxy collection init my_namespace_jovemfelix.my_collection --init-path ./collections/ansible_collections
```

### Creating a Demo Role skeleton

```shell
ansible-galaxy init ./collections/ansible_collections/my_namespace_jovemfelix/my_collection/roles/ping
```



### Folder Structure

```shell
.
├── 00-playbook_hello.yml									# playbook demo
├── README.md															# this file
├── ansible.cfg														# customized ansible to use local inventory
├── collections
│   └── ansible_collections
│       └── my_namespace_jovemfelix
│           └── my_collection
│               ├── README.md							# default generated 
│               ├── docs									# default generated
│               ├── galaxy.yml						# default generated
│               ├── plugins								# default generated
│               │   └── README.md					# default generated
│               └── roles
│                   └── ping							# removed folder and filed not customized
│                       ├── README.md
│                       ├── meta
│                       │   └── main.yml
│                       └── tasks
│                           └── main.yml	# tasks to be executed
└── inventory
    └── local
        └── hosts

12 directories, 10 files
```

### List Current Roles

> I removed any other **global** roles just for brevity

```shell
ansible-galaxy collection list -p ./collections

# /Users/renato/poc/ansible-playbook-grapher-with-collections/collections/ansible_collections
Collection                            Version
------------------------------------- -------
my_namespace_jovemfelix.my_collection 1.0.0
```



## Run Playbook

```shell
ansible-playbook 00-playbook_hello.yml

PLAY [Ansible Hello World Role] *******************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************************************************************************************
[WARNING]: Platform darwin on host 127.0.0.1 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible/2.10/reference_appendices/interpreter_discovery.html for more information.
ok: [127.0.0.1]

TASK [my_namespace_jovemfelix.my_collection.ping : ping at server] ********************************************************************************************************************************************************************
ok: [127.0.0.1]

PLAY RECAP ****************************************************************************************************************************************************************************************************************************
127.0.0.1                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```



## Error while Running ansible-playbook-grapher

```shell
sudo ansible-playbook-grapher 00-playbook_hello.yml
Password:
Traceback (most recent call last):
  File "/usr/local/bin/ansible-playbook-grapher", line 8, in <module>
    sys.exit(main())
  File "/usr/local/lib/python3.9/site-packages/ansibleplaybookgrapher/cli.py", line 190, in main
    cli.run()
  File "/usr/local/lib/python3.9/site-packages/ansibleplaybookgrapher/cli.py", line 46, in run
    grapher = PlaybookGrapher(data_loader=loader, inventory_manager=inventory, variable_manager=variable_manager,
  File "/usr/local/lib/python3.9/site-packages/ansibleplaybookgrapher/grapher.py", line 154, in __init__
    self.playbook = Playbook.load(playbook_filename, loader=data_loader, variable_manager=variable_manager)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/__init__.py", line 51, in load
    pb._load_playbook_data(file_name=file_name, variable_manager=variable_manager)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/__init__.py", line 109, in _load_playbook_data
    entry_obj = Play.load(entry, variable_manager=variable_manager, loader=self._loader, vars=vars)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/play.py", line 116, in load
    return p.load_data(data, variable_manager=variable_manager, loader=loader)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/base.py", line 235, in load_data
    self._attributes[target_name] = method(name, ds[name])
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/play.py", line 195, in _load_roles
    role_includes = load_list_of_roles(ds, play=self, variable_manager=self._variable_manager,
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/helpers.py", line 392, in load_list_of_roles
    i = RoleInclude.load(role_def, play=play, current_role_path=current_role_path, variable_manager=variable_manager,
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/role/include.py", line 60, in load
    return ri.load_data(data, variable_manager=variable_manager, loader=loader)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/base.py", line 222, in load_data
    ds = self.preprocess_data(ds)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/role/definition.py", line 95, in preprocess_data
    (role_name, role_path) = self._load_role_path(role_name)
  File "/usr/local/lib/python3.9/site-packages/ansible/playbook/role/definition.py", line 203, in _load_role_path
    raise AnsibleError("the role '%s' was not found in %s" % (role_name, ":".join(searches)), obj=self._ds)
ansible.errors.AnsibleError: the role 'my_namespace_jovemfelix.my_collection.ping' was not found in /Users/renato/poc/ansible-playbook-grapher-with-collections/roles:/Users/renato/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/Users/renato/poc/ansible-playbook-grapher-with-collections

The error appears to be in '/Users/renato/poc/ansible-playbook-grapher-with-collections/00-playbook_hello.yml': line 5, column 7, but may
be elsewhere in the file depending on the exact syntax problem.

The offending line appears to be:

  roles:
    - my_namespace_jovemfelix.my_collection.ping
      ^ here
```



## Solved with

```shell
# build the package
$ ansible-galaxy collection build ./collections/ansible_collections/my_namespace_jovemfelix/my_collection
Created collection for my_namespace_jovemfelix.my_collection at /Users/renato/poc/ansible-playbook-grapher-with-collections/my_namespace_jovemfelix-my_collection-1.0.0.tar.gz

# install
$ ansible-galaxy collection install my_namespace_jovemfelix-my_collection-1.0.0.tar.gz
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Installing 'my_namespace_jovemfelix.my_collection:1.0.0' to '/Users/renato/.ansible/collections/ansible_collections/my_namespace_jovemfelix/my_collection'
my_namespace_jovemfelix.my_collection (1.0.0) was installed successfully

# list collections
$ ansible-galaxy collection list -p ./collections

# /Users/renato/.ansible/collections/ansible_collections
Collection                            Version
------------------------------------- -------
my_namespace_jovemfelix.my_collection 1.0.0

# /Users/renato/poc/ansible-playbook-grapher-with-collections/collections/ansible_collections
Collection                            Version
------------------------------------- -------
my_namespace_jovemfelix.my_collection 1.0.0

# play again 
$ sudo ansible-playbook-grapher 00-playbook_hello.yml

Graphing Play #1: Ansible Hello World Role (1) ****************************************************************************************************************************************************************************************

Done graphing Play #1: Ansible Hello World Role (1) ***********************************************************************************************************************************************************************************

fThe graph has been exported to 00-playbook_hello.svg
```

> File generated [00-playbook_hello.svg](00-playbook_hello.svg)



## My Enviroment Info

```shell
# ansible version
ansible-playbook --version
ansible-playbook 2.10.8
  config file = /Users/renato/poc/ansible-playbook-grapher-with-collections/ansible.cfg
  configured module search path = ['/Users/renato/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.9/site-packages/ansible
  executable location = /usr/local/bin/ansible-playbook
  python version = 3.9.4 (default, Apr  5 2021, 01:50:46) [Clang 12.0.0 (clang-1200.0.32.29)]

# grapher version
sudo ansible-playbook-grapher --version
ansible-playbook-grapher 0.11.0 (with ansible 2.10.8)
```



## Reference

* How to run an Ansible playbook locally - https://gist.github.com/jovemfelix/fbbcfed3ab24e9a724256bfd1be058f5
* ansible-playbook-grapher - https://github.com/haidaraM/ansible-playbook-grapher
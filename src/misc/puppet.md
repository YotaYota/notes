# Puppet

IaC
multiple servers
rollback or new state
configuration management tool
can also be used as a deployment tool for software

Main server - Client

Server:
Manifests
Templates
Files

Client:
Agent
Facter

Master - slave architecture

Client sends certificate with client id to server. Server signs it and sends back to server.
Facter sends state of the client to master. Based on facts, server compiles manifests into catalogs , catalogs are sent to clients.
The agent executes manifests on its machine. A report of changes is sent back to master.


## Manifests and Modules

Puppet code is composed primarily of **resource declarations**. A resource describes something about the state of the system

```pp
resource_type { 'resource_name'
  attribute => value
  ...
}
```

To list all default resource types available to Puppet
```
puppet resource --types
```

Puppet programs are called **manifests**. The default main manifest in Puppet installed via apt is `/etc/puppet/manifests/site.pp`.

In Puppet, **classes** are code blocks that can be called in a code elsewhere.

A **class definition**
```pp
class example_class {
  ...
  code
  ...
}
```

A **class declaration** occurs when a class is called in a manifest. A class declaration tells Puppet to evaluate the code within the class. Class declarations come in two different flavors: *normal* and *resource-like*.

A **normal class declaration** occurs when the include keyword is used in Puppet code
```pp
include example_class
```

Using **resource-like class declarations** allows you to specify class parameters, which override the default values of class attributes.

A **module** is a collection of manifests and data (such as facts, files, and templates), and they have a specific directory structure. To add a module to Puppet, place it in the `/etc/puppet/modules` directory..

## Templates

- Templates go into *templates/* directory of a module.
- A templare file is referenced by `<module>/<template file>` (note *templates/* omitted)
- Template file can be either *.erb* (embedded ruby) or *.epp* (embedded puppet) format

```pp
file { '/etc/something.conf':
    content => template('something/something.conf.erb'),
}
```

for *.epp* files, use `epp` instead of `template`.

Validate .erb
```
erb -P -x -T '-' example.erb | ruby -c
```
- `-P` ignore lines starting with `%`
- `-x` outputs template's Ruby script
- `-T '-'` sets trim mode to be consistent with puppet's behavior.

### ERB

An ERB template has its own local scope, and its parent scope is set to the class or defined type that evaluates the template.

All variables in the current scope (including global variables) are passed to templates as Ruby instance variables, which begin with “at” signs (`@`).

- `<% expression %>` non-printing tag
- `<%= expression %>` expression-printing tags
- `<%# expression %>` comment tag

**Note**: A hyphen in a tag (`-`) strips leading or trailing whitespace when printing the evaluated template.

**Note**: Text outside a tag is treated as literal text, but is subject to any tagged Ruby code surrounding it.

## Test locally

```
.
├── manifests
│   └── site.pp
└── modules
    └── mymod
        ├── manifests
        │   └── init.pp
        └── templates
            └── some_template.erb
```

```pp
# manifests/site.pp
node default {
  include mymod
}
```

```sh
puppet apply --modulepath=./modules manifests/site.pp
```

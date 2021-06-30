# Conventions

## make

All repositories that generate artifacts or can be tested
should contain a **Makefile**.

The following commands have an agreed upon functionality:

- all
- build
- clean
- help

### all
The default command of the Makefile, should build the
entire repo if contains any changes, and generate the required artifacts.

### build
Force the build of artifacts 


### clean
Should clean the repo so all generated files are removed,
and the folder is in clean state, similar to a newly cloned
repo.

### help 
List the possibly commands for the Makefile and describes
each of the commands.




## Folder conventions

This section describes our naming convention for 
additional folders in repositories.

### /docs

This folder should contain all .md files, pictures
and other files that are used to document the repository.

### /chart

For helm charts, look in: [Helm](../helm/index.md)

### /artifacts

Common folder for all artifacts generated as part of the make process.

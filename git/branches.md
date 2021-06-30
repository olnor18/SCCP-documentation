# Branching conventions

Below is a description of the general git workflow to follow when developing new features. The project has an emphasis on continuous integration and thus we favor frequent pull-requests and smaller features. 

The only branches allowed in a repository are the `main` branch, `feature`- and `hot-fix` branches.

## Feature development 
When creating a new branch for developing a feature name the branch `feature/<descriptive-name>` to indicate the purpose of the branch. In cases where something faulty has made it onto the main branch `hot-fix/<name>` may be used instead. 

A feature branch has the lifetime of a single feature, after a feature has been merged onto the main branch the feature branch must be deleted and any additional features should be developed on a new branch. 

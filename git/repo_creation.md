# Create a new Github repository

This is a guide to create a new Github repository. It must be done in the specified order and no steps can be skipped skipped.

Under the [organization homepage](https://github.com/distributed-technologies) click the "New" buttion to start creating a new repository under the distributed-technologies organization.

## Create repository 

- **Input a repository name.** The name should be all lowercase and with a dash as seperator.

- **Add a description.** Should be no longer than one sentence.

- **Make public**

- **Add README**

- **Add Apache Licence 2.0**

## Change default settings

Go to the "Settings" menu in the repository overview.

### Change options
In the **Options** tab under the **Merge button** section:
- uncheck **Allow merge commit** - uncheck **Allow rebase merging**. 

The only checked option should be **Allow squash merging**.

### Add branch rule

In the **Branches** tab under the **Branch protection rules** section:
- click the "Add rule" button.
- fill in "main" in the **Branch name pattern** input field.
- check **Require pull request reviews before merging**
     - check **Dismiss stale pull request approvals when new commits are pushed**
- check **Require status checks to pass before merging**
- check **Require linear history**
- check **Include administrators**

The settings are saved when the boxes are checked and unchecked, which is why there is no "Save" button.

### Change access settings
In the **Manage access** tab under the **Manage access** section: 
- click "Invite teams or people"
- search and select **distributed-technologies/developers** in the serachfield dropdown.
    - Choose the role **Write**.
    - Click **Add distributed-technologies/developers to this repository**
- Remove yourself as admin by clicking the trashcan icon in the row containing your Github username and confirming in the popup.


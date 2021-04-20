# Create a new Github repository

This is a guide to create a new Github repository. It must be done in the specified order and no steps can be skipped.



## Create repository 

Under the [organization homepage](https://github.com/distributed-technologies) click the "New" buttion to start creating a new repository under the distributed-technologies organization.

- **Input a repository name.** The name should be all lowercase and with a dash as seperator.

- **Add a description.** Should be no longer than one sentence.

- Select **Public**.

- Check **Add a README file**.

- Add **Apache Licence 2.0.**.

- Click **Create repository** button.

## Change default settings

Go to the **Settings** menu in the topbar.

### Change options
In the **Options** tab under the **Merge button** section:
- Uncheck **Allow squash merging**.
- Uncheck **Allow rebase merging**.

The only checked option should be **Allow merge commit**.

### Add branch rule

In the **Branches** tab under the **Branch protection rules** section:
- Click the "Add rule" button.
- Fill in "main" in the **Branch name pattern** input field.
- Check **Require pull request reviews before merging** and:
     - Check **Dismiss stale pull request approvals when new commits are pushed**.
- Check **Require status checks to pass before merging**.
- Check **Require linear history**.
- Check **Include administrators**.
- Click **Create** button.

### Change access settings
In the **Manage access** tab under the **Manage access** section: 
- Click "Invite teams or people"
- Search and select **distributed-technologies/developers** in the searchfield dropdown and:
    - Choose the role **Write**.
    - Click **Add distributed-technologies/developers to this repository**.
- Remove yourself as admin by clicking the trashcan icon in the row containing your Github username and confirming in the popup.


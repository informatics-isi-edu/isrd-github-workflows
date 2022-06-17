# isrd-github-workflows


> :warning: As described in the [setup](#setup) section, the workflows defined here use the GitHub personal access token to operate. Unfortunately, at the time of writing this, the GitHub doesn't allow us to define a more granular control than `admin:org`, and to make sure the token can modify the organization's projects, we have to grant this permission.
> 
> **[Because of the security implications](#security-issues), we advise against using these workflows for the time being.**


The workflows that can be used by other repositories in this organization.

The following are the list of available workflows:

- Workflows to automate GitHub Projects: These workflows are implemented to allow automation of certain tasks related to GitHub Projects. More details can be found [here](docs/user-docs/github-projects.md). The workflows are:
  - `add-existing-issues-to-project`: As the name suggests, it will automate the addition of existing issues to a GitHub Project. 
  - `add-new-item-to-project`: Automate the addition of newly created issues or pull requests to a GitHub Project.


## Setup

To use the automated workflows, a GitHub personal access token needs to be set at the organization level for the workflows to work in any repositories. The default `GITHUB_TOKEN` provided for the workflow run by GitHub does not have the necessary permissions to pull issues/requests from repositories and add them as project items. An admin of the organization needs to create this personal access token and add it as a secret at the organization level, so that it can be accessed by any repository in the organization.

The following are steps to do it:

1. Navigate to the user profile **settings** by clicking the profile button and then settings.
2. In the settings page, navigate to the **Developer settings** option in the left-side options panel. In the developer settings, select **Personal access tokens**. 
3. Click on **Generate new token** and enter your GitHub account password if prompted. 
4. Add a **Note** for your convenience and set the **Expiration** option as 'No expiration'.
5. Provide the following **permissions**: `repo`, `workflow`, `admin:org`, `admin:repo_hook`. After selecting these permissions, click on **Generate token**.
6.  **Copy** the generated personal access token as it would not be visible later.
7.  Navigate to the organization page and then click on the **settings** tab.
8.  In the left-side options panel, click on **Secrets** dropdown and select **Actions**.
9.  On the Actions Secrets page, click on **New organization secret**.
10.  **Name** the secret as `GH_TOKEN` and paste the copied personal access token in the **value** field. Give the **Repository access** to public repositories.
11.  Create the secret and the secret should now be available to the workflows in the organization.

### Security Issues

<!-- TODO -->



## Help and Contact

Please direct questions and comments to the [project issue tracker](https://github.com/informatics-isi-edu/isrd-github-workflows/issues) at GitHub.

## License

isrd-github-workflows is made available as open source under the Apache License, Version 2.0. Please see the [LICENSE file](LICENSE) for more information.

## About Us

isrd-github-workflows is developed in the [Informatics Systems Research Division (ISRD)](http://isrd.isi.edu/) at the [USC Information Sciences Institute](http://www.isi.edu).
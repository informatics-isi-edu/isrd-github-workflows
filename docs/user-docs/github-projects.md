# GitHub Projects

This wiki page will go over what GitHub Projects is and how it can be used as a replacement for ZenHub.

## Table of contents

- [Introduction](#introduction)
- [Project vs. Project (beta)](#project-vs-project-beta)
- [Using Projects (beta)](#using-projects-beta)
  * [Setting up a project](#setting-up-a-project)
  * [Adding existing issues to the project](#adding-existing-issues-to-the-project)
  * [Adding new issues to the project](#adding-new-issues-or-pull-requests-to-the-project)
  * [Keeping open/closeness of issue in sync with the status](#keeping-open-closeness-of-issue-in-sync-with-the-status)


## Introduction

There are two versions of GitHub Projects that we can use:
- [Project](https://docs.github.com/en/issues/organizing-your-work-with-project-boards/managing-project-boards/about-project-boards): A Kanban-style board
- [Project (beta)](https://docs.github.com/en/issues/trying-out-the-new-projects-experience/about-projects): Allows planning and tracking work across repositories with custom fields and multiple views.

For the sake of this documentation, we will refer to these as Project beta and non-beta.

## Project vs. Project (beta)

Project (beta) is basically the non-beta version with more customization and features. Since it's still in the beta stage, it doesn't have the same levels of APIs, but it has more features. In the following, we will focus on the difference between these tools:

- The non-beta only offers a Kanban board, while you have as many custom views as you want with the beta. The views can either be kanban-style or a list.
- Since non-beta can be linked to repositories, it offers an easier way to add issues to a project.
- The beta automatically creates the swimlanes based on a predefined status that can be modified. With non-beta, you have to create your lanes.
- The automation in non-beta can be done through "presets". The presets are "To do", "In Progress", and "Done". In contrast, beta uses "workflow" for automation. 

Based on this, the only advantage of non-beta is the easier UI for adding issues to the project. Everything else shows that beta is the better option. That's why we will focus on the beta version for the rest of the document.

## Using Projects (beta)

We can summarize the flow of adapting GitHub Projects for a repository in the following. We will mainly focus on how you can use GitHub's UI features to do the tasks. But since our goal is to make this process easier, we will also mention how the automation may be achieved by using GitHub API or Action.

### Setting up a project

To create a project, you have to navigate to the organization page and click on create. This will create an empty project with predefined status. 

The first thing that we should do is head to the "settings" page. In here, you can,
- Change the name of the project
- Manage user access. There are multiple levels of access that you can change:
  - private/public: Whether people outside the organization can see it or not. 
  - Base role: The base access level for people in your organization should be.
  - Customized access: Modify access for individual people or teams in your organization.
- Modify/Add custom fields. We recommend the following changes:
  - Add or modify the predefined "status" options.
  - Add a new "Text" custom field and call it "Epic" to mimic ZenHub's epic feature.

> Currently, this process cannot be automated.

### Adding existing issues to the project

After setting up the project, you should start adding existing issues to the project. To do so, you can either use the GUI,
- Navigate to any of the views in your project and search for the issue.
- Navigate to the issues page of a repository and bulk add issues to the project as demonstrated in [here](https://github.blog/changelog/2022-04-07-the-new-github-issues-april-7th-update/).

Instead of using the GUI, you can also use the `add-existing-issues-to-project` workflow. The steps to use the automation are as follows:

1. Download the workflow file from [here](https://github.com/Test-Org-Nikhil/github-workflows/blob/main/.github/workflows/add-existing-issues-to-project.yml). In the workflow file update the values for the environment variables as stated:
    i.  Set *REPO_LINK* to the repository link with issues: '/repos/*organization name*/*your repo*/issues'.
    ii. Set *ORGANIZATION* to the name of the organization.
    iii. Set the *PROJECT_NUMBER* to your project board number.
2. Upload the add *add-existing-issues-to-project.yml* file in the repository containing the issues. The YAML file should be uploaded to the following location: '/*your_repo*/.github/workflows/'. Please create the *.github* and *workflows* directories if not present already.
3. Once the workflow file is uploaded, navigate to the Actions tab in the repository.
4. From the left-panel select the workflow named - *'Add existing issues in a repository to the project board'*. From the dropdown click on Run workflow.
5. The workflow shall take some time depending on the number of issues present in the repository. If the workflow is executed successfully, you will see a blue checkmark along side the workflow and the issues will be then present in the project board.


### Adding new issues or pull requests to the project

When you create a new issue, it won't automatically add to the project, and you have to add it manually.

To automate this, we've prepared a "GitHub workflow" that can be included in each repository. This workflow will add any newly opened issues to the project. The steps for using the automation are as follows:

1. Download the workflow file from [here](https://github.com/Test-Org-Nikhil/test-repo/blob/main/.github/workflows/add-new-project-item.yml). In the workflow file update the *CALLER_PROJECT_NUMBER* variable to the project number of your board.
> There are currently two places where the change needs to be done for the variable - on lines 16th and 26th.

2. Upload the add *add-new-project-item.yml* file in your repository. The YAML file should be uploaded to the following location: '/*your_repo*/.github/workflows/'. Please create the *.github* and *workflows* directories if not present already.


### Keeping open/closeness of issue in sync with the status

After the issues are added to the project, we have to make sure their status is in sync with the state of the issue. There are two sides to this:

- Closing issue should change status to Done: This can be done by activating the "Item closed" built-in workflow. To find this, navigate to the "Workflows" page in the project and ensure the "item closed" is on.
- Changing status to Done should close the issue. This is currently not supported. We will look into whether this can be automated as well. 


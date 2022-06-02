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

In this section we summarize the flow of adapting GitHub Projects for a repository in the following. We will mention how you can achieve each task through GitHub UI as well as automated tools (if available).

### Setting up a project

To create a project, 

1. Navigate to [the organization's Projects page](https://github.com/orgs/informatics-isi-edu/projects?type=beta) and click on create. This will create an empty project with predefined status. 
2. Click on **New project** button and make sure to select the **Beta** version. This will create the project and navigate to its page.
3. Click on the **...** on top right of the page and select **Settings**.
4. Update the **Project name** and the rest of settings if you see fit.
5. Click on **Manage access** on the side panel.
6. There are multiple levels of access that you can change:
   - private/public: Whether people outside the organization can see it or not. 
   - Base role: The base access level for people in your organization should be.
   - Customized access: Modify access for individual people or teams in your organization.
7. In the **Settings** page you can also modify the custom fields. We recommend the following changes:
   - Related to the **Status** field:
      - Add any other extra options that you would like.
      - Based on the current implementation of GitHub Projects, the value of this field can be empty. In most cases, the default "Done" field means the same as empty. That's why we recommend removing the "Done" field.
    - Add a new "Text" custom field and call it "Epic" to mimic ZenHub's epic feature.
  
> Currently, this process cannot be automated.

### Adding existing issues to the project

After setting up the project, you should start adding existing issues to the project. To do so, you can either use the GUI or our custom workflow.

To manually achieve this, you can either:
- Navigate to any of the views in your project and search for the issue.
- Navigate to the issues page of a repository and bulk add issues to the project as demonstrated in [here](https://github.blog/changelog/2022-04-07-the-new-github-issues-april-7th-update/).

To use the `add-existing-issues-to-project` workflow:

1. Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.
2. Download the workflow file from [here](https://github.com/informatics-isi-edu/isrd-github-workflows/blob/main/examples/add-existing-issues.yml) into the `.github/workflows` directory. 
3. In the workflow file update the values for the environment variables as stated:
    - Set `REPO_NAME` to the name of your repository, e.g. `REPO_NAME: chaise`
    - Set the `PROJECT_NUMBER` to your project board number, e.g. `PROJECT_NUMBER: 3`
4. Navigate to the **Actions** tab in the repository.
5. From the left-panel select the workflow named **Add existing issues in a repository to the project board**. 
6. Click on the **Run Workflow** which will open a dropdown. And then click on the **Run workflow** button in the dropdown
  <!-- TODO add screenshot -->
7. The workflow shall take some time depending on the number of issues present in the repository. If the workflow is executed successfully, you will see a blue checkmark along side the workflow and the issues will be then present in the project board.

Using the above automated workflow adds only the open issues to the project board with *no status* if the built-in workflow to move newly added project items to a specific status is not enabled. The closed issues are not fetched and added to the project board using this workflow.


### Adding new issues or pull requests to the project

When you create a new issue, it won't automatically add to the project, and you have to add it manually.

To automate this, you may use the `add-new-project-item` workflow. To do so,

1. Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist.
2. Download the workflow file from [here](https://github.com/informatics-isi-edu/isrd-github-workflows/blob/main/examples/add-new-project-item.yml) into the `.github/workflows` directory. 
3. In the workflow file update the `PROJECT_NUMBER` variable on lines 16th and 26th.

Now any newly created issue or PR will trigger this workflow, which will make sure the newly added item is part of the project. The new issue or PR will get added to the project board with *no status* if the built-in workflow to move newly added project items to a specific status is not enabled.


### Keeping open/closeness of issue in sync with the status

After the issues are added to the project, we have to make sure their status is in sync with the state of the issue. There are two sides to this:

#### 1. Closing issue should change status to Done

To automate this,

1. Navigate to the project page.
2. Click on the **...** on top right of the page and select **Workflows**.
3. Make sure **Item closed** default workflow is enabled and has the proper `Status:Done` value.


#### 2. Changing status to Done should close the issue

This cannot currently be automated.


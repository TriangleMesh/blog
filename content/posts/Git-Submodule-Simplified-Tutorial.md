+++
title = 'Git Submodule Simplified Tutorial'
date = 2024-07-26T00:54:14+08:00
# draft = true
+++
For clarity, we will refer to the local main module as the main module, the main module's GitHub repository as the main repository, the local submodule as the submodule, and the submodule's GitHub repository as the sub-repository.
### Adding Submodules and Sub-repositories
1. Clone the main repository to your local machine:
    ```
    git clone <main_repo_url>
    ```
2. Add the submodule within the main module:
    ```
    git submodule add <submodule_url> <submodule_repo_name>
    ```
    - If the submodule is successfully added, you will see a submodule folder inside the main module's directory and a `.gitmodules` file. Opening `.gitmodules`, you'll find the stored information about your submodule and sub-repository.
3. Push the local files of the main module to GitHub
### Updating Submodules and Sub-repositories
Updates to the main repository and sub-repository do not automatically synchronize, so manual synchronization is required. Here are the two common scenarios:
#### Updating the Main Repository Based on the Sub-repository
1. If changes have been made to the submodule and pushed to the sub-repository, update the submodule in the main module with the following command:
    ```
    git submodule update --remote
    ```
    This will synchronize the local files with the latest submodule files.
2. Push the updates to the main repository
#### Updating the Sub-repository Based on Main Repository
1. In the local submodule directory, use the following command to update the submodule:
    ```
    git submodule update
    ```
    This will update the local files based on the files committed in the main repository.
2. Push the updates to the sub-repository
### Removing Submodules and Sub-repositories
1. Deinitialize the submodule:
    ```
    git submodule deinit <submodule-name>
    ```
2. Remove the submodule directory:
    ```
    git rm -r <submodule-name>
    ```
3. Delete the submodule-related files in the `.git` directory (from the main module directory):
    ```
    rm -rf .git/modules/<submodule-name>
    ```
    **Note**: You can use the `-f` option to force deletion if necessary.
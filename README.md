# LeetSyncMaster

**LeetSyncMaster** is a GitHub Action that automatically collects and organizes LeetCode submissions into a GitHub repository.

Here's a sample repository created using LeetSyncMaster: [leetcode-solutions](https://github.com/shubhamwagh/leetcode-solutions)

## Features

- Fully automated collection of all your latest accepted submissions with minimal effort setup (currently only supports Python3, C++, MySQL and Bash)
- Single commit for each submission stamped with the original submission date for building rich and accurate contributions graph
- Automated git pushes to the remote repository with every action triggers
- Auto-generate README.md based on retrieved submissions


## Setup

1. Retrieve LeetCode cookies

   - Login to LeetCode
   - Open `Web Developer Tools`
   - Find `https://leetcode.com` from `Storage > Cookies`
   - Copy values for cookies `LEETCODE_SESSION` and `csrftoken`

2. Create a GitHub repository for LeetSyncMaster

3. Create [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) for LeetCode API access

   - Open `Settings > Secrets and variables > Actions` from the repository
   - Click `New repository secret`
   - Create a new secret `LEETCODE_SESSION` using `LEETCODE_SESSION` cookie value
   - Create a new secret `LEETCODE_CSRF_TOKEN` using `csrftoken` cookie value

4. Give workflows write permissions in the repository for git pushes

   - Open `Settings > Actions > General` from the repository
   - Select `Read and write permissions` from `Workflow permissions` and save

5. Create [GithHub Action](https://docs.github.com/en/actions/quickstart)

   - Create a workflow directory `.github/workflows/`
   - Create a workflow file, e.g., `sync_leetcode.yml`
   - Copy the following YAML contents into the `sync_leetcode.yml` file:

     ```yml
     name: LeetSyncMaster
     on: 
       workflow_dispatch:
       schedule:
           - cron: '0 0 * * *'  # Run the workflow daily at midnight
     jobs:
       build:
         runs-on: ubuntu-latest
         steps:
           - name: Run LeetSyncMaster
             uses: shubhamwagh/LeetSyncMaster@v1.2.0
             with:
               GITHUB_TOKEN: ${{ github.token }}
               LEETCODE_SESSION: ${{ secrets.LEETCODE_SESSION }}
               LEETCODE_CSRF_TOKEN: ${{ secrets.LEETCODE_CSRF_TOKEN }}
     ```

6. Trigger GitHub action

   - Open `Actions > LeetSyncMaster` from the repository
   - Click `Run workflow > Run workflow`

## Inputs

- `GITHUB_TOKEN` - **required**. GitHub token used in pushing submissions to the repository
- `LEETCODE_SESSION` - **required**. LeetCode session used in accessing LeetCode API
- `LEETCODE_CSRF_TOKEN` - **required**. LeetCode CSRF token used in accessing LeetCode API

---

#### Credits

This LeetSyncMaster GitHub Action is adapted from [dos-m0nk3y/LeetCode-Synchronizer](https://github.com/dos-m0nk3y/LeetCode-Synchronizer). Special thanks to the original author for creating this tool.


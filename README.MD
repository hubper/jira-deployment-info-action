# Jira deployment info action

Note: this repo is currently experimental and is used only for personal usage!
Unfortunately, Github doesn't have support for private actions yet. See https://github.com/github/roadmap/issues/74

## What it will do

- It will tag an commit (i.e. 'production-deployment'), indicating which commit has last been deployed to a certain environment
- It will extract Jira issue number from all commit messages between the latest tag and the latest commit on the specified branch
- It will upload this deployment info to Jira
- It will create a new tag on the latest commit
- this process will repeat itself on every run

## Usage

Somewhere inside your job / workflow

```yaml
steps:
  - name: Deploy to Production
    run: |
      echo "Deploying to Production..."
- name: Push Production Deployment Info to Jira
  uses: hubper/jira-deployment-info-action@main
  with:
    cloud-instance-base-url: https://my-company.atlassian.net
    environment-name: "Production"
    environment-type: "production"
    branch-name: master

  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    JIRA_CLIENT_ID: ${{ secrets.JIRA_CLIENT_ID }}
    JIRA_CLIENT_SECRET: ${{ secrets.JIRA_CLIENT_SECRET }}
```

### Variables

| with                    | required | description                                                                                   |
| ----------------------- | -------- | --------------------------------------------------------------------------------------------- |
| cloud-instance-base-url | yes      | the base url of your Jira cloud instance. i.e. `https://my-company.atlassian.net`             |
| environment-name        | yes      | The name that will be displayed in the UI of Jira                                             |
| environment-type        | yes      | Sets the type of environment. Choose one of `development`, `testing`, `staging`, `production` |
| branch-name             | no       | defaults to `master`. The branch to analyze commits on and set / retrieve tags.               |

### Env

| env                | required | description                           |
| ------------------ | -------- | ------------------------------------- |
| GITHUB_TOKEN       | yes      | best stored inside your repo-secrets  |
| JIRA_CLIENT_ID     | yes      | best stored inside your repo-secrets  |
| JIRA_CLIENT_SECRET | yes      | best stored inside your repo-secretsÌ |

## Development

- Clone this repo
- Run `npm install`
- Edit the code inside `index.ts`
- Unfortunately it's pretty hard to write tests for this action
- Upon committing a husky hook gets triggered, which will result in transpiling and bundling the action right-away (gh-actions requires that your action-repo is a standalone runnable, so a `npm install` shouldn't be needed when running the action)

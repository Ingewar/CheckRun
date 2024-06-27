# checkrun.yml

# This file is used to configure the behavior of a GitHub Actions check run.
# Check runs are used to run automated tests, linting, or other code analysis tools on your codebase.

name: My Check Run
on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      # Add your own steps here to run tests, linters, or other code analysis tools

      - name: Publish check run
        uses: actions/github-script@v4
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const { context } = require('@actions/github');

            const octokit = context.octokit;
            const repo = context.repo;

            const checkRun = await octokit.checks.create({
              ...repo,
              name: 'My Check Run',
              head_sha: context.sha,
              status: 'completed',
              conclusion: 'success',
              output: {
                title: 'Check Run Completed',
                summary: 'All checks passed successfully',
              },
            });

            console.log('Check run created:', checkRun);

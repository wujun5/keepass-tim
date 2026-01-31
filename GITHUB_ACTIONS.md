# GitHub Actions for KeePassXC ARM64 Builds

This project includes GitHub Actions workflows to automatically build KeePassXC for ARM64 Linux systems.

## Enabling GitHub Actions

GitHub Actions should be enabled by default on most repositories. If they aren't working:

1. Go to your repository on GitHub
2. Click on the "Actions" tab
3. If prompted to enable workflows, click "I understand my workflows, go ahead and enable them"

## Available Workflows

- `build-arm64.yml`: Builds KeePassXC for ARM64 architecture

## Using the Workflows

The workflow will automatically run when:
- You push changes to the `main` or `master` branch
- You open a pull request targeting those branches

## Manual Trigger

To manually trigger a workflow:
1. Go to the "Actions" tab in your repository
2. Select the "Build KeePassXC for ARM64" workflow
3. Click "Run workflow" and select the branch

## Results

After a successful build, you'll find the compiled artifacts in the "Artifacts" section of the workflow run. These include:
- Compiled KeePassXC binaries for ARM64
- Installation packages

## Requirements

For the workflow to work properly:
- The repository must have the Dockerfile and build scripts in place
- GitHub Actions must be enabled for the repository
- The workflow file must be in the `.github/workflows/` directory

## Troubleshooting

If the workflow fails:
1. Check the workflow logs for specific error messages
2. Verify that all required files exist in the repository
3. Ensure the repository has sufficient permissions
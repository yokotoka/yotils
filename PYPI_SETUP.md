# PyPI OIDC Trusted Publishing Setup Guide

This guide explains how to configure PyPI's OIDC trusted publishing for the `yotils` package.

## Prerequisites

1. You must have a PyPI account (create one at https://pypi.org/account/register/)
2. You must be the owner or maintainer of the `yotils` project on PyPI

## Step-by-Step Setup Instructions

### 1. Create the Project on PyPI (First Time Only)

If this is your first release, you need to create the project on PyPI first:

1. Go to https://pypi.org/manage/account/publishing/
2. Scroll to the "Add a new pending publisher" section
3. Fill in the form with the following details:

   **Form Fields:**
   - **PyPI Project Name**: `yotils`
   - **Owner**: `yokotoka`
   - **Repository name**: `yotils`
   - **Workflow name**: `publish-to-pypi.yml`
   - **Environment name**: `pypi`

4. Click "Add"

This creates a "pending publisher" that will be activated when you first publish the package.

### 2. Configure Trusted Publisher (If Project Already Exists)

If the `yotils` package already exists on PyPI:

1. Go to https://pypi.org/manage/project/yotils/settings/publishing/
2. Scroll to the "Add a new publisher" section
3. Fill in the form with the following details:

   **Form Fields:**
   - **Owner**: `yokotoka`
   - **Repository name**: `yotils`
   - **Workflow name**: `publish-to-pypi.yml`
   - **Environment name**: `pypi`

4. Click "Add"

### 3. Publish a Release

Once the trusted publisher is configured, you can publish to PyPI by pushing a tag:

```bash
# Ensure you're on the main branch
git checkout main
git pull

# Create and push a version tag (e.g., v0.1.0)
git tag v0.1.0
git push origin v0.1.0
```

The GitHub Actions workflow will automatically:
1. Build the package using `uv`
2. Publish to PyPI using OIDC authentication (no API tokens needed!)

### 4. Verify the Publication

After the workflow completes:
1. Check the GitHub Actions run at: https://github.com/yokotoka/yotils/actions
2. Verify the package on PyPI at: https://pypi.org/project/yotils/

## Important Notes

- **No API tokens required**: OIDC trusted publishing uses GitHub's identity to authenticate with PyPI
- **Secure by default**: Only the specified repository, workflow, and environment can publish
- **Tag format**: Tags must match the pattern `v*.*.*` (e.g., `v0.1.0`, `v1.2.3`, `v2.0.0-beta1`)
- **Main branch only**: The workflow is configured to run when tags are pushed (tags are typically created from main)

## Troubleshooting

### "Trusted publisher not found"
- Verify the repository owner, name, workflow name, and environment name match exactly
- Check that you've configured the publisher on PyPI

### "Permission denied"
- Ensure you're an owner or maintainer of the `yotils` project on PyPI
- Verify the GitHub Actions workflow has the correct permissions

### "Invalid version"
- Ensure the version in `yotils/__init__.py` follows semantic versioning
- Check that the tag name matches the version (e.g., tag `v0.1.0` for version `0.1.0`)

## Additional Resources

- [PyPI Trusted Publishers Documentation](https://docs.pypi.org/trusted-publishers/)
- [GitHub OIDC Documentation](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)

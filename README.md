# n8n Workflow Backup Automation

Automated backup solution for n8n workflows to GitHub repository using n8n itself. This workflow automatically retrieves all your n8n workflows and saves them as JSON files to a GitHub repository.

## Overview

This project provides an automated backup pipeline that:
- Retrieves all workflows from your n8n instance
- Checks if workflow files already exist in the GitHub repository
- Updates existing workflow files or creates new ones
- Commits changes with timestamped messages
- Maintains a complete backup history in Git

## Workflow Architecture

The backup workflow consists of 13 nodes organized in the following pipeline:

1. **Manual Trigger** - Initiates the backup process
2. **Date Handling** - Captures and formats current timestamp
3. **GitHub Integration** - Lists existing files in the repository
4. **n8n API Integration** - Retrieves all workflows from your n8n account
5. **Data Processing** - Converts workflows to binary format and processes individually
6. **Conditional Logic** - Checks if each workflow already exists
7. **Git Operations** - Updates or uploads workflow files with commit messages

## Prerequisites

Before setting up this workflow, you need:

- An active n8n instance (self-hosted or cloud)
- A GitHub account with a repository for backups
- GitHub OAuth2 credentials or Personal Access Token
- n8n API credentials

## Setup Instructions

### 1. GitHub Configuration

1. Create a new GitHub repository for your workflow backups (or use an existing one)
2. Generate a GitHub Personal Access Token:
   - Go to GitHub Settings > Developer settings > Personal access tokens
   - Create a token with `repo` scope (full control of private repositories)
3. In n8n, create a GitHub OAuth2 credential or HTTP Request credential with your token

### 2. n8n API Configuration

1. Obtain your n8n API key:
   - For n8n cloud: Go to Settings > API > Create API Key
   - For self-hosted: Configure API access in your n8n settings
2. Create an n8n credential in your n8n instance with the API key

### 3. Import the Workflow

1. Copy the contents of `backup-gR9p5-a8pXQESBh3ciBbA.json`
2. In your n8n instance, go to Workflows > Import from File
3. Paste the JSON or upload the file
4. Configure the following nodes with your credentials:
   - **List files from repository [GITHUB]** - Add your GitHub credential
   - **Retrieve workflows [N8N]** - Add your n8n API credential
   - **Update file [GITHUB]** - Add your GitHub credential
   - **Upload file [GITHUB]** - Add your GitHub credential

### 4. Configure Repository Details

Update the following settings in the GitHub nodes:
- **Repository Owner** - Your GitHub username or organization
- **Repository Name** - The name of your backup repository

## Usage

### Manual Execution

1. Open the workflow in n8n
2. Click the "Execute Workflow" button
3. The workflow will:
   - Fetch all workflows from your n8n instance
   - Check which ones already exist in GitHub
   - Update or create files accordingly
   - Commit with timestamp (e.g., "backup-19-01-2026/14:30")

### Automated Scheduling (Recommended)

To enable automatic backups:

1. Replace the "Execute workflow" trigger node with a "Schedule Trigger" node
2. Configure your preferred schedule:
   - **Daily backups**: `0 2 * * *` (runs at 2 AM daily)
   - **Hourly backups**: `0 * * * *` (runs every hour)
   - **Every 6 hours**: `0 */6 * * *`
3. Activate the workflow by toggling the switch in the top-right corner

## Commit Message Format

Commits are automatically created with the following format:
```
backup-DD-MM-YYYY/H:MM
```

Example: `backup-19-01-2026/14:30`

## Workflow Features

### Smart File Handling
- **Existing files**: Updates the file content and creates a new commit
- **New files**: Creates the file in the repository

### Batch Processing
- Processes each workflow individually to handle failures gracefully
- Continues backing up remaining workflows even if one fails

### Git History
- Maintains complete backup history with timestamps
- Easy to track changes and restore previous versions
- View diffs between backup versions

## Troubleshooting

### Workflow Not Executing

- Verify the workflow is **active** (check the toggle in top-right)
- Check that all credentials are properly configured
- Ensure your n8n instance can reach the n8n API and GitHub API

### GitHub Authentication Errors

- Verify your GitHub token has `repo` scope
- Check that the repository owner and name are correct
- Ensure your token hasn't expired

### n8n API Errors

- Verify your API key is valid
- Check that your n8n instance URL is correct
- Ensure API access is enabled in your n8n instance

### No Workflows Being Backed Up

- Check that you have workflows in your n8n instance
- Verify the "Retrieve workflows [N8N]" node is configured correctly
- Check execution logs for error messages

## File Structure

```
Test_backup/
├── backup-gR9p5-a8pXQESBh3ciBbA.json  # Main workflow definition
├── example.json                        # Placeholder file
└── README.md                           # This documentation
```

As workflows are backed up, they will appear as:
```
Test_backup/
├── workflow-name-1.json
├── workflow-name-2.json
└── ...
```

## Security Considerations

- **Never commit credentials** - Credentials are stored in n8n, not in the workflow JSON
- **Repository visibility** - Consider using a private repository for workflow backups
- **Token permissions** - Use minimum required permissions (repo scope only)
- **Regular rotation** - Rotate API tokens and credentials periodically

## Enhancements & Future Improvements

Potential enhancements to consider:

- Add error notification nodes (email/Slack/webhook)
- Implement workflow filtering by tags or name patterns
- Add backup retention policy (cleanup old commits)
- Include execution statistics and logging
- Add backup verification checks
- Implement differential backups (only backup changed workflows)
- Create a backup inventory/metadata file

## Credits

Workflow concept inspired by [@igorzuevich](https://www.youtube.com/@igorzuevich)

## License

This project is provided as-is for personal and commercial use.

## Support

For issues or questions:
- Check the n8n documentation: https://docs.n8n.io
- Visit the n8n community forum: https://community.n8n.io
- Review GitHub API documentation: https://docs.github.com/en/rest

---

**Last Updated**: January 2026

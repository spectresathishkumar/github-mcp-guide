# Setting Up GitHub MCP

This guide will walk you through the process of setting up GitHub Mission Control Panel (MCP) for your development workflow.

## Prerequisites

1. GitHub account with appropriate permissions
2. Development environment with necessary tools
3. Basic understanding of GitHub workflows

## Step-by-Step Setup

### 1. Generate GitHub Access Token

1. Go to GitHub Settings > Developer settings > Personal access tokens
2. Click "Generate new token (classic)"
3. Select the following scopes:
   - repo (full control of private repositories)
   - workflow (update GitHub Action workflows)
   - admin:org (read and write org data)
   - user (read and write user data)
4. Generate and securely store your token

### 2. Configure Environment

```bash
# Set up environment variables
export GITHUB_TOKEN=your_token_here
export GITHUB_USERNAME=your_username
```

### 3. Install Required Dependencies

```bash
# Using npm
npm install @octokit/rest
npm install @actions/github

# Using yarn
yarn add @octokit/rest
yarn add @actions/github
```

### 4. Basic Configuration

```javascript
const { Octokit } = require('@octokit/rest');

// Initialize Octokit with your token
const octokit = new Octokit({
  auth: process.env.GITHUB_TOKEN
});

// Basic configuration object
const config = {
  owner: process.env.GITHUB_OWNER,
  repo: process.env.GITHUB_REPO,
  defaultBranch: 'main'
};
```

## Setting Up Automation

### 1. Pull Request Review Automation

```javascript
// Setup PR review automation
async function setupPRAutomation() {
  // Configure PR review settings
  const reviewConfig = {
    requiredReviewers: 2,
    assignByOwnership: true,
    automaticAssignment: true
  };
  
  // Initialize PR handlers
  await setupPRHandlers(reviewConfig);
}
```

### 2. Issue Management

```javascript
// Setup issue automation
async function setupIssueAutomation() {
  // Configure issue templates
  const issueConfig = {
    templates: ['bug', 'feature', 'documentation'],
    autoLabeling: true,
    defaultAssignees: ['teamLead']
  };
  
  // Initialize issue handlers
  await setupIssueHandlers(issueConfig);
}
```

## Verification

Test your setup with these basic operations:

```javascript
async function verifySetup() {
  try {
    // Test authentication
    const { data: user } = await octokit.users.getAuthenticated();
    console.log('Authenticated as:', user.login);
    
    // Test repository access
    const { data: repo } = await octokit.repos.get({
      owner: config.owner,
      repo: config.repo
    });
    console.log('Repository access confirmed:', repo.full_name);
    
    return true;
  } catch (error) {
    console.error('Setup verification failed:', error);
    return false;
  }
}
```

## Security Best Practices

1. **Token Management**
   - Never commit tokens to version control
   - Use environment variables
   - Regularly rotate tokens
   - Use minimal required permissions

2. **Access Control**
   - Implement role-based access
   - Regular access audits
   - Monitor API usage

3. **Error Handling**
   - Implement proper error handling
   - Log security events
   - Monitor failed operations

## Next Steps

1. Review the [PR Review Automation](pr-review-automation.md) guide
2. Explore [Common Use Cases](use-cases.md)
3. Implement [Best Practices](best-practices.md)

## Troubleshooting

### Common Issues

1. **Authentication Failures**
   - Verify token permissions
   - Check token expiration
   - Validate environment variables

2. **Rate Limiting**
   - Implement rate limit handling
   - Use conditional requests
   - Cache responses when possible

3. **Permission Issues**
   - Verify repository access
   - Check organization permissions
   - Review token scopes

## Support

For additional support:
1. Check GitHub's API documentation
2. Review MCP documentation
3. Contact your team's GitHub administrator
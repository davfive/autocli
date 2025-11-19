# GitHub Copilot Agent FAQ

## Why was my previous agent deleted?

GitHub Copilot agents are session-based and can end for several reasons:

### Common Reasons for Agent Session Termination

1. **Task Completion**: The agent completed its assigned task and the session naturally ended.

2. **Timeout**: Agent sessions have time limits. If a session runs too long without activity or completion, it may be terminated automatically.

3. **Error or Failure**: If the agent encounters an unrecoverable error or fails to make progress, the session may be terminated.

4. **User Action**: You or another user may have manually stopped or cancelled the agent session.

5. **Resource Constraints**: The system may terminate sessions due to resource limitations or high demand.

6. **New Session Started**: Starting a new agent session on the same issue/task may terminate the previous one.

### What Happens to Agent Work?

- **Committed Changes**: Any changes that were committed and pushed to the branch are preserved.
- **Uncommitted Changes**: Local changes that weren't committed may be lost when the session ends.
- **PR Updates**: Pull request descriptions and comments created by the agent remain.
- **Branch**: The branch created by the agent persists and can be continued by a new agent or manually.

### How to Continue Work from a Previous Agent

1. The branch and any pushed commits remain available
2. You can start a new agent session on the same branch/PR
3. The new agent will see all committed work from the previous session
4. Review the commit history and PR description to understand what was completed

### Best Practices

- Review and commit work frequently during long tasks
- Break large tasks into smaller, manageable pieces
- Check PR descriptions and commit messages to understand agent progress
- Save important findings or decisions in commit messages or PR comments

## Additional Resources

For more information about GitHub Copilot:
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [GitHub Support](https://support.github.com/)

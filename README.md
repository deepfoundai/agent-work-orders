# Agent Work Orders

Issue-as-Work-Order system for autonomous agents in the deepfoundai ecosystem.

## How It Works

1. **Create Issue** using the `agent-task` template
2. **Add "Work" label** to trigger the workflow
3. **GitHub Action** parses issue and sends to AWS EventBridge
4. **Agents** receive work orders and execute tasks
5. **Auto-close** when agents publish completion events

## Architecture

```
GitHub Issues → GitHub Actions → EventBridge → Lambda Agents → CloudWatch
```

## Setup

### Required GitHub Secrets

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY` 
- `AWS_REGION` (defaults to `us-east-1`)

### Supported Agents

- DevOpsAutomation
- CostSentinel
- CreditReconciler
- FalInvoker
- MRRReporter
- PromptCurator
- RoutingManager
- DocRegistry

## Usage

1. Create new issue using `.github/ISSUE_TEMPLATE/agent-task.yml`
2. Select target agent from dropdown
3. Fill in work specification with clear requirements
4. Set deadline and add any dependencies
5. Add "Work" label to trigger processing
6. Monitor CloudWatch logs for agent activity
7. Issue auto-closes when work completes

## Example Work Order

Create a GitHub issue with:

- **Target Agent**: DevOpsAutomation
- **Environment**: prod
- **Deadline**: 2025-06-28T23:59
- **Work Specification**: 
  ```
  Deploy J-01 Job Status API:
  1. Create DynamoDB table Jobs-prod
  2. Deploy GetJobFn Lambda function
  3. Update existing jobs API
  4. Add unit tests with 80% coverage
  ```

## Monitoring

- **Processing Status**: Check issue comments for EventBridge confirmation
- **Agent Logs**: Monitor CloudWatch `/aws/lambda/cc-agent-{name}-prod`
- **Completion**: Issues auto-close with completion summary
- **Failures**: Check workflow logs and agent error messages

## Troubleshooting

### Work Order Not Processing
- Check if "Work" label is applied
- Verify GitHub secrets are configured
- Check workflow logs in Actions tab

### Agent Not Responding
- Check CloudWatch logs for the specific agent
- Verify EventBridge event was published
- Check if agent is healthy in admin dashboard

### Issue Not Auto-Closing
- Check if agent published completion event
- Verify auto-close workflow is running every 5 minutes
- Look for completion logs with issue number pattern

## Contributing

1. Use the standard issue template for consistency
2. Be specific and actionable in work specifications
3. Set realistic deadlines based on task complexity
4. Include all necessary context and dependencies

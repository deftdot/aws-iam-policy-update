# aws-iam-policy-update
![Github Action](https://flat.badgen.net/badge/Github/Action/green?icon=github)
![MIT license](https://flat.badgen.net/badge/License/MIT/green)

Github action that updates existing IAM Policy with a New Version Policy JSON.

This allows a Self service approach to organization users to Update AWS Policies without the need to contact the Cloud Owner in the organization, but simply open a PR.
Once the PR is approved the Action updates the policy automaticly.

## Usage
This action rely on the connection to AWS to be already established, this can be done by setting manually the environment variables: 
"AWS_ACCESS_KEY_ID", "AWS_SECRET_ACCESS_KEY", "AWS_DEFAULT_REGION". 
Or by the Using AssumeRole Action prior to running this Action. (As in the example below)

## Inputs
| Name | Description | Required |
| ---- | ----------- | -------- |
| policy-arn | The ARN of the policy you want to update  | yes |
| policy-file | Path to the New Policy JSON file | yes |


## Examples
```yaml
# Using Assume Role
jobs:
  updatepolicy:
    name: Update the AWS Policy
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: configure aws credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: ${{ secrets.ASSUME_ROLE_ARN }}
          role-session-name: aws_session
          aws-region: us-east-1
      
      - name: modify the policy
        uses: deftdot/aws-iam-policy-update@v0.1
        with:
          policyarn: ${{ secrets.POLICY_ARN }}
          policyfile: 'policies/policy.json' #Assume the policy JSON located at {repo_root}/policies/policy.json
```

```yaml
# Using AWS Credentials in secrets
jobs:
  updatepolicy:
    name: Update the AWS Policy
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}
      
      - name: modify the policy
        uses: deftdot/aws-iam-policy-update@v0.1
        with:
          policyarn: ${{ secrets.POLICY_ARN }}
          policyfile: 'policies/policy.json' #Assume the policy JSON located at {repo_root}/policies/policy.json
```

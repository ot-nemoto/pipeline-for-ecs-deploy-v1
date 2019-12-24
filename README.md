# pipeline-for-ecs-deploy-v1


### Generate new token

CloudFormationとGitHubを連携させるためにアクセストークンを生成

```sh
curl -u "your github username" \
     -d '{"scopes":["repo"],"note":"pipeline-for-ecs-deploy-v1"}' \
     https://api.github.com/authorizations
  # {
  #   ...
  #   "token": "774d8f6c********************************",
  #   ...
  # }
```

生成したトークンをSSMパラメータストアに登録

```sh
GITHUB_OAUTH_TOKEN=774d8f6c********************************
aws ssm put-parameter \
    --name pipeline-for-ecs-deploy-v1-github-oauth-token \
    --value ${GITHUB_OAUTH_TOKEN} \
    --type String
  # {
  #     "Tier": "Standard",
  #     "Version": 1
  # }
```

### Deploy

```sh
aws cloudformation create-stack \
    --stack-name pipeline-for-ecs-deploy-v1 \
    --capabilities CAPABILITY_NAMED_IAM \
    --template-body file://template.yaml
```

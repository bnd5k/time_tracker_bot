# README
This is Sinatra app that runs a Slack bot. It's meant to be used on AWS Lambda.

If you'd like to use this repo, clone it, change the remote, and add your `env`
file (see `env.sample` for an example).

# Prerequisites

If you have already configured your local machine for AWS dev work, then you can skip this section.

1. Create an AWS account.  See this tutorial: [https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account)
1. Create IAM user with privileges for S3, Lambda, and API gateway.
1. Install and configure the AWS CLI.  If you’re on a mac, just run
“brew install awscli”. For configuration instructions (adding your creds), see 
the documentation at [https://github.com/aws/aws-cli](https://github.com/aws/aws-cli).
1. Install the SAM CLI. If you’re on a mac, just run “brew install aws-sam-cli”.
For more info, see the documentation at [https://github.com/awslabs/aws-sam-cli](https://github.com/awslabs/aws-sam-cli).

# Usage

1. Clone the repo
2. Install the dependencies for local development and for deployment
```
$ bundle install
...
$ bundle install --deployment
```
3. If you need to run the app locally, open a terminal window and run 
`rackup app/config.ru` from the root directory of this repo.

4. Create your AWS S3 bucket. In the example below, we'll name the bucket `time-tracker-app`.
```
$ aws s3 mb s3://time-tracker-app
```
5. Package the app
```
$ sam package --template-file template.yaml --output-template-file packaged-template.yaml --s3-bucket time-tracker-bot
```
6. Deploy the app
```
$ sam deploy --template-file packaged-template.yaml --stack-name time-tracker-bot --capabilities CAPABILITY_IAM
```
7. In Slack, create a new app at [https://api.slack.com/apps](https://api.slack.com/apps).
8. Configure your Slack app to allow Incoming Webhooks. Copy the Webhook URL 
to your `.env` file.  This will be your `SLACK_WEBHOOK_URL`.
9. Configure your Slack app to use a Slash Command. The Request URl for your
Slash Command will be the API Gateway URL for your Sinatra App.
In your AWS console, head to Lambda > Applications > time-tracker-bot. Scroll
down to “Resources” and click on “SinatraAPI.” This will launch the AWS API
Gateway.  Find “Dashboard” on the side menu and click it.  At the top of the
page, find the URL for your app.
10. In Slack, type your Slash Command and it should result in a clickable button link.

# AWS Lambda Start Server [![Build Status](https://travis-ci.org/SamVerschueren/aws-lambda-start-server.svg?branch=master)](https://travis-ci.org/SamVerschueren/aws-lambda-start-server) [![Coverage Status](https://coveralls.io/repos/SamVerschueren/aws-lambda-start-server/badge.svg?branch=master&service=github)](https://coveralls.io/github/SamVerschueren/aws-lambda-start-server?branch=master)

> Easily start EC2 servers automatically at a certain time


## Usage

Download the [zip](https://github.com/SamVerschueren/aws-lambda-start-server/releases) file and deploy it as a lambda function.

After you deployed the lambda function, you navigate to `Event sources` and choose for the `Scheduled Event`. After naming your event, you can provide the following schedule expression.

```
cron(45 7 ? * MON-FRI *)
```

This expression wil launch the servers every weekday, from monday to friday, at 7:45 AM.


### Tags

Tagging your instance is very important in order for the lambda function to work properly. The lambda function will retrieve all the instances that have a tag with a key `LaunchGroup`, and a value being the name of the cronjob rule you provided.

For instance, if you named your cronjob rule `OfficeHoursLauncher`, make sure you add a tag with key-value `LaunchGroup=OfficeHoursLauncher` to all the instances you want to launch at the provided schedule. You can add as many schedules as you want.

### Manual invocation

You can also manually invoke the lambda function to start your servers. Open the lambda function in the AWS Management Console. Click `Actions` and `Configure test event`. Provide the name of the launchgroup you want to start, for instance `OfficeHoursLauncher` and click `Save and test`.


### IAM Roles

Make sure your lambda function is able to describe and start instances.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "MyStatementId",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:StartInstances"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```


## Related

- [aws-lambda-stop-server](https://github.com/SamVerschueren/aws-lambda-stop-server) - The same function but for stopping the server.


## License

MIT © Sam Verschueren

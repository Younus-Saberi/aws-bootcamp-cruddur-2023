# Week 0 — Billing and Architecture


## Lucid Chart and Architecture Diagram

![image](https://user-images.githubusercontent.com/73644685/219707212-858d64c7-75e8-4d09-81b6-dfe21803a831.png)

[Here is my LucidChart Link](https://lucid.app/lucidchart/604bf756-af26-4d9b-b514-1ba5bcee22f5/edit?viewport_loc=-393%2C-422%2C2431%2C1242%2C0_0&invitationId=inv_0ecfc60c-0ee5-4203-b6e2-1afabe76bf1a)


### Install and Verify AWS CLI 

I was not able to use Gitpod or Github Codespaces due to browser issues.
So I decided to use a local enviroment.

In order to prove that I am able to use the AWS CLI.
I am providing the instructions I used for my configuration of my local machine on windows.

I did the following steps to install AWS CLI.

I installed the AWS CLI for Windows 10 via command in **Command Prompt**:

I followed the instructions on the [AWS CLI Install Documentation Page](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

<!-- ![Installing AWS CLI](assets/installing-windows-aws-cli.png) -->

```
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

I attempted to run the command by typing in `aws` but I recieved an error

```
C:\Users\Younus>aws
'aws' is not recognized as an internal or external command,
operable program or batch file.
```

I was able to resolve the error by closing command prompt, and opening it again.

<!-- ![Proof of Working AWS CLI](assets/proof-of-aws-cli.png) -->

### Create a Budget

I created my own Budget for $1 because I cannot afford any kind of spend.
I did not create a second Budget because I was concerned of budget spending going over the 2 budget free limit.

![Image of The Budget Alarm I Created](assets/budget-alarm.png) 

### Recreate Logical Architectural Deisgn

## Lucid Chart and Architecture Diagram

![image](https://user-images.githubusercontent.com/73644685/219707212-858d64c7-75e8-4d09-81b6-dfe21803a831.png)

[Here is my LucidChart Link](https://lucid.app/lucidchart/604bf756-af26-4d9b-b514-1ba5bcee22f5/edit?viewport_loc=-393%2C-422%2C2431%2C1242%2C0_0&invitationId=inv_0ecfc60c-0ee5-4203-b6e2-1afabe76bf1a)

## Example of Referncing a file in the codebase

Example of me of referencing a file in my repo
[week-1-again/aws/json/alarm-config.jso](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1-again/aws/json/alarm-config.json)



## Code Example

```json
{
  "AlarmName": "DailyEstimatedCharges",
  "AlarmDescription": "This alarm would be triggered if the daily estimated charges exceeds 1$",
  "ActionsEnabled": true,
  "AlarmActions": [
      "arn:aws:sns:ca-central-1:***REMOVED***:billing-alarm"
  ],
  "EvaluationPeriods": 1,
  "DatapointsToAlarm": 1,
  "Threshold": 1,
  "ComparisonOperator": "GreaterThanOrEqualToThreshold",
  "TreatMissingData": "breaching",
  "Metrics": [{
      "Id": "m1",
      "MetricStat": {
          "Metric": {
              "Namespace": "AWS/Billing",
              "MetricName": "EstimatedCharges",
              "Dimensions": [{
                  "Name": "Currency",
                  "Value": "USD"
              }]
          },
          "Period": 86400,
          "Stat": "Maximum"
      },
      "ReturnData": false
  },
  {
      "Id": "e1",
      "Expression": "IF(RATE(m1)>0,RATE(m1)*86400,0)",
      "Label": "DailyEstimatedCharges",
      "ReturnData": true
  }]
}
```

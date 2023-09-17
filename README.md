
## Demo Live View

https://dev.d22wq2d6siim5h.amplifyapp.com/


## Screenshots

![Architecture Diagram]([https://github.com/yyhao0422/end-to-end_webapp/blob/master/architecturediagram.png])



## Content

## Get Started

1. Create an static website with HTML (index.html)
2. Zip the index.html file

## Host web app at AWS Amplify
1. Drop the Zip file and deploy.
2. A Domain URL will given.

## Create Lambda Function
1. Select `Author from scratch` 
2. Runtime `Python 3.9`
3. Create Function
4. Replace the function code into

```
# import the JSON utility package
import json
# import the Python math library
import math

# import the AWS SDK (for Python the package name is boto3)
import boto3
# import two packages to help us with dates and date formatting
from time import gmtime, strftime

# create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
# use the DynamoDB object to select our table
table = dynamodb.Table('PowerOfMathDatabase')
# store the current time in a human readable format in a variable
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

# define the handler function that the Lambda service will use an entry point
def lambda_handler(event, context):

# extract the two numbers from the Lambda service's event object
    mathResult = math.pow(int(event['base']), int(event['exponent']))

# write result and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': str(mathResult),
            'LatestGreetingTime':now
            })

# return a properly formatted JSON object
    return {
    'statusCode': 200,
    'body': json.dumps('Your result is ' + str(mathResult))
    }

```

## Create a RestAPI in API Gateway
1. Create Post Method
2. Choose Lambda function that created just now.
3. Save it.
4. Click the `action` button
4. Enable `CORS` in the Post Method
5. Deploy API
6. Copy Invoke URL in clipboard.

## Create a dynamodb table
1. Create a table with default settings
2. Enter `ID` as Partition Key.
2. Copy DynamoDB ARN in clipboard.

## Add Lambda Execution Role
1. Create Inline policy for your Lambda Function
2. Paste below policy and replace your Table ARN.
```
{
"Version": "2012-10-17",
"Statement": [
    {
        "Sid": "VisualEditor0",
        "Effect": "Allow",
        "Action": [
            "dynamodb:PutItem",
            "dynamodb:DeleteItem",
            "dynamodb:GetItem",
            "dynamodb:Scan",
            "dynamodb:Query",
            "dynamodb:UpdateItem"
        ],
        "Resource": "YOUR-TABLE-ARN"
    }
    ]
}

```

## Final
1. Update your HTML code by adding the Invoke URL for the RestAPI.
2. Test your App.

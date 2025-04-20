# Step Functions

### Running virtualenv
```
pip install virtualenv

virtualenv venv
source venv/bin/activate
which python #to verify
```

### 🛠️ Step Function Plan (Orchestration)
```
[Step Function]
   |
   +--> Lambda #1: Move file from raw → staged
   |
   +--> Lambda #2: Extract metadata → publish to Kafka
   |
   +--> Lambda #3: Notify Spring Boot (file acknowledged)
   |
   +--> Lambda #4: Send Email via SES

```


1. Create a Step Function
Use the AWS Console or aws-cli

Type: Standard

Name: FileProcessingWorkflow

Define the state machine using Amazon States Language (JSON)

2. Refactor Your Current Move Lambda
Rename current Lambda to lambda_move_file

Remove the S3 trigger (we’ll trigger it via Step Function now)

Modify it to take input like:

# Step Functions
AWS Step Functions is a serverless orchestration service that helps you coordinate multiple AWS services (like Lambda, ECS, Glue, etc.) into automated workflows. Think of it like a flowchart, but for your backend logic—each “step” represents a task or decision.


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


### Flow:
1. Step Function > Create your own 
   1. State machine name : `DataPipelineFileProcessingWorkflow`
   2. Machine Type : `Standard`
   3. Permissions: 

2. Code 
   - Apply `file_processing_workflow.asl.json` code
3. Role will be created when you create the Workflow. [`StepFunctions-DataPipelineFileProcessingWorkflow-role-uzf31ftie`]
4. Open the State Machine (Step Function you created) > Start Execution and provide the Input for testing:
   '''
      {
         "bucket": "data-pipeline-kushal8-file-watcher-dev",
         "key": "raw/example2.csv"
      }
   '''
   


Step	Lambda Name	Description
✅ 1. Move file	lambda_move_file	Already done and tested
🔜 2. Extract	lambda_extract_metadata	Reads file (from staged/) and sends metadata to Kafka
🔜 3. Notify App	lambda_notify_springboot	Calls Spring Boot API to mark file as acknowledged
🔜 4. Email	lambda_send_email	Sends SES email confirmation



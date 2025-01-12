# EventBridge Rule to Stop EC2 Instance

This project demonstrates how to create an AWS EventBridge rule to stop an EC2 instance automatically using a scheduled cron expression and a Lambda function.

## Steps to Set Up

### 1. Prerequisites
- An active AWS account.
- An EC2 instance that you want to stop.
- IAM permissions to manage EC2 instances and EventBridge rules.
- A Lambda function set up to stop EC2 instances.

---

### 2. Schedule the EventBridge Rule
1. **Go to AWS Management Console**:
   - Open **Amazon EventBridge**.

2. **Create a Rule**:
   - Click on **Create Rule**.
   - Provide a name (e.g., `StopEC2InstanceRule`).
   - Choose **Schedule** as the rule type.

3. **Set the Cron Expression**:
   - Use the following cron expression to schedule the event for 2 minutes from now:
     ```
     cron(46 8 * * ? *)
     ```
     *(Update the time to match your current UTC time + 2 minutes)*

4. **Add Target**:
   - Select **AWS Lambda Function** as the target.
   - Choose the Lambda function that stops the EC2 instance.

5. **Save and Activate**:
   - Click **Create Rule**.

---

### 3. Lambda Function for Stopping EC2
Use the following Python code for your Lambda function:

```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2', region_name='your-region')  # Replace with your AWS region
    instance_id = 'your-instance-id'  # Replace with your EC2 instance ID

    try:
        response = ec2.stop_instances(InstanceIds=[instance_id])
        print(f"Stopping EC2 instance {instance_id}: {response}")
    except Exception as e:
        print(f"Error stopping EC2 instance {instance_id}: {str(e)}")
```

#### Lambda Function Setup:
- Assign the Lambda function an IAM role with the `ec2:StopInstances` permission.
- Replace `your-region` and `your-instance-id` with your AWS region and EC2 instance ID.

---

### 4. Testing
- Once the EventBridge rule triggers, the Lambda function will execute and stop the specified EC2 instance.

---

### 5. Notes
- Ensure the Lambda function is in the same region as your EC2 instance.
- Verify the cron expression is set for the correct UTC time.
- The rule can be edited or disabled at any time in the **EventBridge Console**.




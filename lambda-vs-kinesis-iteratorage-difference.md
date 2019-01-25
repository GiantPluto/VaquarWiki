**IteratorAge  is fall under AWS Lambda Metrics (Monitoring and Troubleshooting Lambda Applications):**

**IteratorAge :**
 Emitted for stream-based invocations only (functions triggered by an
 Amazon DynamoDB stream or Kinesis stream). Measures the age of the
 last record for each batch of records processed. Age is the difference
 between the time Lambda received the batch, and the time the last
 record in the batch was written to the stream.



The **AWS::Lambda::EventSourceMapping** resource creates a mapping between an event source and an AWS Lambda function. Lambda reads items from the event source and triggers the function. 

Each event source mapping identifies the type of events to publish and the Lambda function to invoke when events occur. The specific Lambda function then receives the event information as a parameter and your Lambda function code then processes the event.

Except for the poll-based AWS services (Amazon Kinesis Data Streams and DynamoDB streams or Amazon SQS queues), other supported AWS services publish events and can also invoke your Lambda function (referred to as the push model). 




  

    EventSourceMapping:
        DependsOn: Policy
        Type: 'AWS::Lambda::EventSourceMapping'
        Properties:
          BatchSize: !Ref BatchSize
          Enabled: true
          EventSourceArn: {'Fn::ImportValue': !Sub '${TableModule}-StreamArn'}
          FunctionName: {'Fn::ImportValue': !Sub '${LambdaModule}-Arn'}
          StartingPosition: !Ref StartingPosition
    Outputs:
      ModuleId:
        Value: 'lambda-event-source-dynamodb-stream'
    	
    	


> IteratorAge is the delay detection afforded by Lambda’s new latest metric ,strictly for stream-based invocations and works by
> determining the age of the last record for each batch of processed
> records. Age is quantified by measuring the time elapsed between when
> Lambda received the batch in question and when the final record within
> that batch was written to the stream.

- https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

If You want to know as soon as possible when records are not processed as quickly as they need to. This metric will help you monitor this and prevent your applications from building a dangerous backlog of unprocessed records

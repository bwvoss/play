create-stack: ## Creates the stack
	aws cloudformation create-stack --stack-name myteststack --template-body file://./infrastructure.json --capabilities CAPABILITY_IAM > .aws_state

update-stack: ## Updates the stack
	aws cloudformation update-stack --stack-name myteststack --template-body file://./infrastructure.json --capabilities CAPABILITY_IAM

cleanup-stack: ## Cleansup the stack
	aws cloudformation delete-stack --stack-name myteststack > .aws_state
	echo "" > .kinesis_state
	echo "" > .logs

STACK_ID := $(shell cat .aws_state | jq .StackId)
describe-stack: ## Details the stack from current state
	aws cloudformation list-stack-resources --stack-name $(STACK_ID)

SHARD_ITERATOR := $(shell cat .kinesis_state | jq .ShardIterator)
get-logs: ## Pass in the Kinesis stream arn as stream=name
	aws kinesis get-shard-iterator --stream-name $(stream) --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON > .kinesis_state
	aws kinesis get-records --limit 10 --shard-iterator $(SHARD_ITERATOR) > .logs

decode-log: ## Placeholder for reminder
	echo -n "$(log)" | base64 --decode | zcat

THIS_FILE := $(lastword $(MAKEFILE_LIST))
.DEFAULT_GOAL := help
.PHONY: help

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

# var AWS = require('aws-sdk');

# exports.handler = function(event, context) {
    # var kinesis = new AWS.Kinesis();
    # var params = {
    # StreamName: 'myteststack-KinesisStream-FBHMM7K6OOF7', /* required */
    # ExclusiveStartShardId: 'shardId-000000000000',
     # Limit: 1
    # };
    # kinesis.describeStream(params, function(err, data) {
     # if (err) context.fail(err, err.stack); // an error occurred
     # else     console.log(data);           // successful response
    # });
# };

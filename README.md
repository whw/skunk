## Setup
* Install [Gordon](https://gordon.readthedocs.io/en/latest/index.html). I chose Gordon because it seemed to be the simplest and sharpest tool for working with AWS Lambda, and so far I've been pretty impressed.
* Install [Docker](https://www.docker.com/community-edition) (for running DynamoDB locally)
* I think you need to make sure that you've got AWS credentials setup in `$HOME/.aws` (instructions [here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files))

There's a good chance I'm forgetting something here, so if you have any problems please don't hesitate to let me know.

## Run unit tests
### Start local DynamoDB instance
`./bin/start-dynamodb`

### Run unit tests
`./bin/run-unit-tests`

### Stop local DynamoDB instance (after you're all done)
`./bin/stop-dynamodb`



## Run integration tests
### Create remote DynamoDB table
`T_STAGE=prod ./bin/create-table`

### Deploy latest code
`./bin/do-deploy`

### Modify API gateway endpoint
After you deploy, Gordon will spit back a set of outputs like:
```
Project Outputs:
  ApigatewayHelloworldapi
    https://rgx1pkb8se.execute-api.us-west-2.amazonaws.com/dev
```

### Run integration tests
`T_STAGE=prod ./bin/run-integration-tests`

### Delete table (after you're all done)
`T_STAGE=prod ./bin/delete-table`


## Simulate the Raspberry Pi client

So I've included a little dummy script that will just send a status (-1, 0, 1) to the backend once a second.

### To run against the local backend
`./bin/start-fake-pi`

### To run against the backend hosted on AWS
`T_STAGE=prod ./bin/start-fake-pi`

### See changes in the table (provide `T_STAGE=prod` for table hosted on AWS)
`./bin/scan-table`

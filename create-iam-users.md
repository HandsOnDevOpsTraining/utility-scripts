### Configure AWS Account in local machine

aws configure


### Create a json with the list of emails of the users to be created
#### chatgpt query
Create users.json that is needed for AWS ClI using the following users list and options
1. UserName as email
2. password as Training@123
3. PasswordResetRequired as false 

### Shell script
The following script iterates through the each json object and then does following
1. Create a IAM user
2. Create login profile
3. Add user to the group

```#!/bin/bash

JSON_FILE="/Users/apple/tech/trainings/devops/aws/julyusers.json"

for user in $(jq -c '.[]' $JSON_FILE); do
    email=$(echo "$user" | jq -r '.UserName')
    username=$(echo "$email" | awk -F'[@.]' '{ print $1 }')
    aws iam create-user --user-name "$username"
    aws iam create-login-profile --cli-input-json "$user"
    aws iam add-user-to-group --user-name "$username" --group-name "DevOpsBeginner04July2023"
done
```

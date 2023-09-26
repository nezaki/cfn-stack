```
AWS_ACCOUNT_ID=999999999999
AWS_SWITCH_ACCOUNT_ID=999999999999
AWS_ACCOUNT_NAME=
AWS_IAM_ROLE_NAME=
AWS_ROLE_SESSION_NAME=
AWS_USERNAME=

echo -n "mfa token: "
read MFA_TOKEN
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=

CREDENTIALS=$(aws sts assume-role \
  --serial-number arn:aws:iam::${AWS_ACCOUNT_ID}:mfa/${AWS_USERNAME} \
  --role-arn arn:aws:iam::${AWS_SWITCH_ACCOUNT_ID}:role/${AWS_IAM_ROLE_NAME} \
  --role-session-name ${AWS_ROLE_SESSION_NAME} \
  --duration-seconds 43200 \
  --token-code ${MFA_TOKEN})

echo ${CREDENTIALS}

export AWS_ACCESS_KEY_ID=$(echo ${CREDENTIALS} | jq -r .Credentials.AccessKeyId)
export AWS_SECRET_ACCESS_KEY=$(echo ${CREDENTIALS} | jq -r .Credentials.SecretAccessKey)
export AWS_SESSION_TOKEN=$(echo ${CREDENTIALS} | jq -r .Credentials.SessionToken)
export AWS_DEFAULT_REGION=ap-northeast-1

echo \\nExpiration $(echo ${CREDENTIALS} | jq -r .Credentials.Expiration)
echo export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
echo export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
echo export AWS_SESSION_TOKEN=${AWS_SESSION_TOKEN}
echo export AWS_DEFAULT_REGION=ap-northeast-1
```

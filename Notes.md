# Notes

```bash
aws configure
AWS Access Key ID [None]: test
AWS Secret Access Key [None]: test
Default region name [None]: us-east-1
Default output format [None]: 

aws --endpoint-url=http://localhost:4566 s3api create-bucket --bucket my-bucket --region us-east-1

aws s3api create-bucket \
    --bucket my-bucket \
    --region us-east-1
aws s3 cp ./ s3://my-bucket --recursive --include ".json"
aws s3 ls s3://my-bucket
aws s3 ls
aws secretsmanager create-secret \
    --name AdnanAppGitCreds \
    --description "app git clone credentials" \
    --secret-string "{\"username\":\"myuser\",\"password\":\"xxx\"}" \
    --tags Key=costCenter,Value=C8710ATG Key=dataClassification,Value=public Key=supportGroup,Value=PSS_CCoE Key=blockCode,Value=IIMCLCTCSP
aws secretsmanager list-secrets
```

```bash
cat << EOF | oc apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: localstack-addendum
rules:
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - get
  - list
  - watch
  - create
  - delete
  - deletecollection
  - patch
  - update
EOF

oc policy add-role-to-user localstack-addendum cgruver -n cgruver-che
```

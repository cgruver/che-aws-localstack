# Notes

```bash
alias aws="aws --endpoint-url=http://localhost:4566"
aws configure
AWS Access Key ID [None]: test
AWS Secret Access Key [None]: test
Default region name [None]: us-east-1
Default output format [None]: 

aws s3api create-bucket --bucket my-bucket --region us-east-1

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

## Non-working Devfile

```yaml
schemaVersion: 2.2.0
metadata:
  name: localstack
  projectType: LocalStack
  displayName: LocalStack
components:
- name: dev-tools
  container: 
    image: quay.io/cgruver0/che/che-dev-image:latest
    memoryRequest: 1Gi
    memoryLimit: 6Gi
    cpuRequest: 500m
    cpuLimit: 2000m
    mountSources: true
    sourceMapping: /projects
    env:
    - name: SHELL
      value: "/bin/zsh"
- name: localstack-deployment
  openshift:
    deployByDefault: true
    inlined: |
      kind: Deployment
      apiVersion: apps/v1
      metadata:
        name: localstack
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: localstack
        template:
          metadata:
            labels:
              app: localstack
          spec:
            containers:
              - name: localstack
                image: public.ecr.aws/localstack/localstack
                ports:
                  - name: http
                    containerPort: 4566
                    protocol: TCP
- name: localstack-service
  openshift:
    deployByDefault: true
    inlined: |
      kind: Service
      apiVersion: v1
      metadata:
        name: localstack
      spec:
        type: ClusterIP
        ports:
        - name: http-4566
          port: 4566
          protocol: TCP
          targetPort: 4566
      selector:
        app: localstack
- volume:
    size: 4Gi
  name: projects
```
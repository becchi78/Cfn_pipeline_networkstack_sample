# Cfn_pipeline_networkstack_sample

## Dummy デプロイ

```bash
aws cloudformation create-stack \
  --stack-name PipelineNetworkStack \
  --template-body file://dummy.yaml \
  --capabilities CAPABILITY_IAM
```

## 手動デプロイ

```bash
aws cloudformation create-stack \
  --stack-name PipelineNetworkStack \
  --template-body file://root-template.yaml \
  --parameters file://param/parameters-cli.json \
  --capabilities CAPABILITY_IAM
```

## 削除

```bash
aws cloudformation delete-stack --stack-name PipelineNetworkStack
```

```bash
aws s3 cp templates/subnet.yaml s3://cfn-networkstack-pipeline-sample/network/pipelinesubnet.yaml
aws s3 cp templates/vpc.yaml s3://cfn-networkstack-pipeline-sample/network/pipelinevpc.yaml
```

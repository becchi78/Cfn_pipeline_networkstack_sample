# Cfn_pipeline_networkstack_sample

Cfn_pipeline_sampleでデプロイするnetworktemplateです。

## 手動デプロイ

```bash
aws cloudformation deploy \
  --stack-name PipelineNetworkStack \
  --template-file root-template.yaml \
  --parameters file://param/parameters.json \
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

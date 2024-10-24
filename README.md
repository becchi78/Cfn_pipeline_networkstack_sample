# Cfn_pipeline_networkstack_sample

Cfn のネステッドスタックとクロススタック参照のサンプル（network）

CodePipeline による CI/CD を実施するためのファイルと GitHub Actions による Linting 用の workflow も含む。

- .github/workflow/cfn-static-analysis.yaml Github Actions の定義ファイル
- parameters/parameters.json 設定ファイル
- templates/pipelinesubnet.yaml Subnet 用 Cfn テンプレートファイル
- templates/pipelinevpc.yaml VPC 用 Cfn テンプレートファイル
- templates/root-template.yaml 親テンプレートファイル
- buildspec_driftdetection.yaml ドリフト検知を行う buildspec ファイル
- buildspec.yaml 子 template を S3 にアップロードする buildspec ファイル

## 手動デプロイの準備

templates/にある yaml はあらかじめ S3 の cfn-nested-sample/network に置いておく。

```bash
aws s3 cp ./templates/pipelinevpc.yaml s3://cfn-pipeline-nestedstack-sample/network/
aws s3 cp ./templates/pipelinesubnet.yaml s3://cfn-pipeline-nestedstack-sample/network/
```

## 手動デプロイ

```bash
aws cloudformation deploy \
  --stack-name Pipeline-NetworkStack \
  --template-file file://templates/root-template.yaml \
  --parameter-overrides file://parameters/parameters.json \
  --capabilities CAPABILITY_IAM
```

## 削除

```bash
aws cloudformation delete-stack --stack-name PipelineNetworkStack
```

## Output

| キー      | 説明                        | エクスポート名         |
| --------- | --------------------------- | ---------------------- |
| SubnetId1 | The ID of the first Subnet  | NetworkStack-SubnetId1 |
| SubnetId2 | The ID of the second Subnet | NetworkStack-SubnetId2 |
| SubnetId3 | The ID of the third Subnet  | NetworkStack-SubnetId3 |
| VpcId     | The ID of the VPC           | NetworkStack-VpcId     |

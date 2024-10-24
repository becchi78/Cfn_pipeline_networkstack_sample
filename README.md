# Cfn_pipeline_networkstack_sample

CI/CD Pipeline のデプロイ対象のスタックのサンプルです。（network）
CodePipeline による CI/CD を実施するためのファイルと GitHub Actions による Linting 用の workflow を含んでいます。

## 構成

```bash
Cfn_Pipeline_NetworkStack_Sample/
│
├── .github/
│   └── workflows
│       └── cfn-static-analysis.yaml ・・・GitHub Actionsでコードの静的解析を行うための定義ファイル
│
├── parameters/
│   └──  parameters/parameters.json ・・・ネットワークスタックのパラメータファイル
│
├── templates/
│   │── pipelinesubnet.yaml ・・・Subnet 用 Cfn テンプレートファイル
│   │── pipelinevpc.yaml ・・・VPC 用 Cfn テンプレートファイル
│   └── root-template.yaml ・・・親テンプレートファイル
│
├── buildspec_driftdetection.yaml ・・・ドリフト検知を行う buildspec ファイル
├── buildspec.yaml ・・・S3に子テンプレートをアップロードする buildspec ファイル
└── README.md ・・・このREADME
```

## 手動デプロイの準備

templates/にある yaml はあらかじめ S3 の所定のバケット（例として cfn-nested-sample/network） に置いておきます。

```bash
aws s3 cp ./templates/pipelinevpc.yaml s3://cfn-pipeline-nestedstack-sample/network/
aws s3 cp ./templates/pipelinesubnet.yaml s3://cfn-pipeline-nestedstack-sample/network/
```

## 手動デプロイ

```bash
aws cloudformation deploy \
  --stack-name PipelineNetworkStack \
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

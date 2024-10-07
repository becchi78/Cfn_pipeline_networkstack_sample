# Cfn_networkstack_sample

Cfn のネステッドスタックとクロススタック参照のサンプル（network）

CodePipeline による CI/CD を実施するためのファイルと GitHub Actions による Linting 用の workflow も含む。

- param/parameters-cli.json 手動デプロイ用の設定ファイル
- param/parameters-pipeline.json Pipeline 用の設定ファイル

## 手動デプロイの準備

templates/にある yaml はあらかじめ S3 の cfn-nested-sample/network に置いておく。

```bash
aws s3 cp ./templates/vpc.yaml s3://cfn-nested-sample/network/
aws s3 cp ./templates/subnet.yaml s3://cfn-nested-sample/network/
```

## 手動デプロイ

```bash
aws cloudformation create-stack \
  --stack-name NetworkStack \
  --template-body file://root-template.yaml \
  --parameters file://param/parameters-cli.json \
  --capabilities CAPABILITY_IAM
```

## 削除

```bash
aws cloudformation delete-stack --stack-name NetworkStack
```

## Output

| キー      | 説明                        | エクスポート名         |
| --------- | --------------------------- | ---------------------- |
| SubnetId1 | The ID of the first Subnet  | NetworkStack-SubnetId1 |
| SubnetId2 | The ID of the second Subnet | NetworkStack-SubnetId2 |
| SubnetId3 | The ID of the third Subnet  | NetworkStack-SubnetId3 |
| VpcId     | The ID of the VPC           | NetworkStack-VpcId     |

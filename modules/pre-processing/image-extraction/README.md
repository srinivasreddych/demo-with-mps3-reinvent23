# Pre Processing workflow

## Description

This module contains a Docker container for detecting lanes on images using 
LaneDet (https://github.com/Turoad/lanedet), with the resnet34_tusimple backbone (configs and weights).  It is designed to incorporate the weights, the model code, and the transformation/processing code into one image with the entry point being `tools/detect_lanes.py` when processing.  That entry point as one (1) positional required arguement to indicate the local path to the configuration (`configs/laneatt/resnet34_tusimple.py` in this case).  

NOTE: this image MUST use GPU compute resources that are NVIDIA driver enabled. Using ECS Optimized AMI with GPU enabled for this module

### Sample Sagemaker Processing Code

A sample piece of code that leverages this image as a Sagemaker Processing job is located at `/src/sample_sm_processor.py`.  This image CAN be reconfigured to run as a PyTorchProcessing job in SM, but that requires the following changes:
- a new image that does not have the supporting processing code or weights
- a `sourcedir.tar.gz` that has the weights and processing code
- a new python code (similar to `tools/detect_lanes.py`) that leverages the PytorchProcessor classes and the artifacts above.

## Inputs/Outputs

### Input Parameters

#### Required

- `vpc-id`: VPC Id to be used by the Pre Processing resources.
- `private-subnet-ids`: List of Private Subnets Ids where the Pre Processing resources should be deployed to.
- `artifact-bucket-name`: The Artifact bucket to which Scene Detection pipeline can upload any assets, other than module specific data.
- `ecr-repo-name`: ECR Repository name to be used/created if it doesnt exist.
- `on-demand-job-queue-arn`: Job Queue ARN from Batch Compute Core Module.

#### Optional

- `timeout-seconds`: AWS Batch job container execution timeout value.

### Sample declaration

```yaml
name: image-extraction
path: modules/pre-processing/image-extraction
parameters:
  - name: vpc-id
    valueFrom:
      moduleMetadata:
        group: optionals
        name: networking
        key: VpcId
  - name: private-subnet-ids
    valueFrom:
      moduleMetadata:
        group: optionals
        name: networking
        key: PrivateSubnetIds
  - name: artifacts-bucket-name
    valueFrom:
      moduleMetadata:
        group: optionals
        name: buckets
        key: ArtifactsBucketName
  - name: ecr-repo-name
    value: image-extraction-image-repo
  - name: on-demand-job-queue-arn
    valueFrom:
      moduleMetadata:
        group: core
        name: batch-compute
        key: OnDemandJobQueueArn
  - name: timeout-seconds
    value: 2000
```

### Module Metadata Outputs

- `ImageExtractionDkrImageUri`: AWS ECR URI of the container image
- `BatchExecutionRoleArn`: ARN of the IAM Role for running the AWS Batch container

#### Output Example

```json
{
  "BatchExecutionRoleArn": "arn:aws:iam::123456789012:role/addf-ros-image-demo-docke-addfrosimagedemodockerim-123456789012",
  "ImageExtractionDkrImageUri": "123456789012.dkr.ecr.us-west-2.amazonaws.com/image-extraction-image-repo:latest"
}
```

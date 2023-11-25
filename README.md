# Steps to deploy

## Clone the project

You will need to clone the repository using the below command:

```bash
git clone codecommit::us-west-2://demo-with-mps3-reinvent23
```

You should move into the repository:

```bash
cd demo-with-mps3-reinvent23
```

### Setup

#### Create and activate a Virtual environment

```bash
python3 -m venv .venv && source .venv/bin/activate
```

#### Install the requirements

```bash
pip install -r ./requirements.txt
```

#### Setting the AWS Region

ADDF submits build information to AWS CodeBuild via AWS CodeSeeder.  The initial submittal is done via AWS CLI, leveraging the configured active AWS profile.  If you would like to deploy ADDF to a region other than the region configured for the profile, you can explicitly set the region via:

```bash
export AWS_DEFAULT_REGION=us-west-2
```

> Note: You can set the above region to your region of interest

Please see [HERE for details](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

#### Bootstrap the CDKV2

We use AWS CDK V2 as the standard CDK version because CDK V1 is set to maintenance mode beginning June 1, 2022. You  would need to bootstrap the CDK environment (one time per region) with V2 - see [HERE](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html) in the account(s) you will be deploying.

```bash
cdk bootstrap aws://<account>/<region>
```

#### Bootstrap AWS Account(s)

Assuming that you will be using a single account, follow the guide [here](https://seed-farmer.readthedocs.io/en/latest/bootstrapping.html#) to bootstrap your account(s) to function as a toolchain and target account.

Following is the command to bootstrap your existing account to a toolchain and target account

```sh
seedfarmer bootstrap toolchain --project addf --trusted-principal [principal-arns] --as-target
```

> Note: seedfarmer bootstrap toolchain --project addf --trusted-principal arn:aws:iam::788323849607:role/Admin --as-target

#### Prepare the Manifests for Deployment

- Replace the value of `toolchainRegion` with your region of interest.
- Set the `accountId` under `targetAccountMappings` to the AWS Account you would deploy target modules. You could set the `accountId` as below:

```yaml
targetAccountMappings:
  - alias: primary
    accountId: 1234567890
```

### Deployment WalkThrough

For the below walkthrough, let us use the `manifests` directory for deployment, where the deploymemt name is set to `without-mps3`.

File `deployment.yaml` is the top level manifest, which should include the modules you wanted to deploy, grouped under logical containers called `group`.  Please see [manifests](https://seed-farmer.readthedocs.io/en/latest/manifests.html) for understanding more details about a deployment manifest, the keys it supports(if mandatory/optional).

#### Deploy

Below is the command to deploy the modules using the `SeedFarmer` CLI using the main manifest `deployment.yaml`:

```bash
seedfarmer apply manifests/deployment.yaml --debug
```

## Steps to Destroy ADDF

Below is the command to destroy all the modules related to a deployment:

```bash
seedfarmer destroy demo-with-mps3
```

> Note:
> Replace the `DEPLOYMENT_NAME` with the desired deployment name of your environment. For ex: `demo-with-mps3` is the current deployment name
> You can pass an optional `--debug` flag to the above command for getting debug level output

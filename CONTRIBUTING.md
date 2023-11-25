# Contributing Guidelines

Thanks for your interest in contributing to ADDF :)

Whether it's a bug report, new feature, correction, or additional
documentation, we greatly value feedback and contributions from our community.

Please read through this document before submitting any issues or pull requests to ensure we have all the necessary
information to effectively respond to your bug report or contribution.

## Reporting Bugs/Feature Requests

We welcome you to use the GitHub issue tracker to report bugs or suggest features.

When filing an issue, please check existing open, or recently closed, issues to make sure somebody else hasn't already
reported the issue. Please try to include as much information as you can. Details like these are incredibly useful:

* A reproducible test case or series of steps
* The version of our code being used
* Any modifications you've made relevant to the bug
* Anything unusual about your environment or deployment

## Contributing via Pull Requests

Contributions via pull requests are much appreciated. Before sending us a pull request, please ensure that:

1. You are working against the latest source on the *main* branch.
2. You check existing open, and recently merged, pull requests to make sure someone else hasn't addressed the problem already.
3. You open an issue to discuss any significant work - we would hate for your time to be wasted.

To send us a pull request, please:

Fork the repository and run the following commands to clone the repository locally.

```sh
$ git clone https://github.com/{your-account}/autonomous-driving-data-framework.git
```

Then, prepare your local environment before you move forward with the development.

```sh
$ cd autonomous-driving-data-framework
$ git checkout -b <<BRANCH-NAME>>
$ python3 -m venv .venv && source .venv/bin/activate
$ pip install -r requirements.txt -r requirements-dev.txt
```

You can refer to the SeedFarmer [guide](https://seed-farmer.readthedocs.io/en/latest/usage.html) to understand how SeedFarmer CLI can be used to bootstrap and deploy ADDF.

If you wish to create a new module, you can refer to the guide of [SeedFarmer](https://github.com/awslabs/seed-farmer/blob/bac754a1a66ca2a184fae691909f2e2bc6a115a6/docs/source/usage.md#create-a-new-module) else you can continue to make changes to the existing modules using standard Git process.

```sh
$ seedfarmer init module -g <<GROUP-NAME> -m <<MY-MODULE>>
$ cd modules/<<GROUP-NAME>/<<MY-MODULE>>
```

If you wish to generate the requirements.txt for your module(s) using `pip-compile` for CDK modules in python, then you can run the below command:

```sh
$ scripts/pip-compile.sh --path modules/optionals/networking/
```

>Note: To upgrade requirements, you can add `--upgrade` at the end of the above instruction

Modify the source; please focus on the specific change you are contributing. If you also reformat all the code, it will be hard for us to focus on your change.

Ensure you perform local checks on your change(s) to ADDF modules. From the root of the ADDF repo, you should run the below script to perform code formatting and sort imports:

```sh
scripts/fix.sh --language python --path modules/optionals/networking/

```

>Note: If the CDK language is typescript, you should replace it in the above command at `--language`

From the root of the ADDF repo, you should run the below script to perform validation checks:

```sh
scripts/validate.sh --language python --path modules/optionals/networking/
```

>Note: If the CDK language is typescript, you should replace it in the above command at `--language`.   If you wish to skip the static checks during the validation step, you can supply an argument as --skip-static-checks.

Commit to your fork using clear commit messages and send us a pull request, answering any default questions in the pull request interface.

Pay attention to any automated CI failures reported in the pull request, and stay involved in the conversation.

GitHub provides additional document on [forking a repository](https://help.github.com/articles/fork-a-repo/) and
[creating a pull request](https://help.github.com/articles/creating-a-pull-request/).

## Finding contributions to work on

Looking at the existing issues is a great way to find something to contribute on. As our projects, by default, use the default GitHub issue labels (enhancement/bug/duplicate/help wanted/invalid/question/wontfix), looking at any 'help wanted' issues is a great place to start.

## Code of Conduct

This project has adopted the [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct).
For more information see the [Code of Conduct FAQ](https://aws.github.io/code-of-conduct-faq) or contact
opensource-codeofconduct@amazon.com with any additional questions or comments.

## Security issue notifications

If you discover a potential security issue in this project we ask that you notify AWS/Amazon Security via our [vulnerability reporting page](http://aws.amazon.com/security/vulnerability-reporting/). Please do **not** create a public github issue.

## Licensing

See the [LICENSE](LICENSE) file for our project's licensing. We will ask you to confirm the licensing of your contribution.

We may ask you to sign a [Contributor License Agreement (CLA)](http://en.wikipedia.org/wiki/Contributor_License_Agreement) for larger changes.
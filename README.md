# NowSecure GitLab CI


This is the source repository to build the docker image available at https://hub.docker.com/repository/docker/nowsecure/gitlab-ci to be used within GitLab CI. 

This NowSecure image gives you the ability to perform automatic mobile app security testing for Android and iOS mobile apps through the NowSecure Platform test engine.

## Summary

Purpose-built for mobile app teams, NowSecure provides fully automated, mobile appsec testing coverage (static+dynamic+interactive+API  tests) optimized for the CI/CD dev pipeline. The NowSecure Extension for Azure DevOps integrates with Microsoft Azure DevOps directly for automated testing workflows and fast feedback loops.

NowSecure tests mobile app binaries developed in any language or framework (so no language limitations and no source code required) and provides complete results including newly developed code, 3rd party code, and compiler/operating system dependencies. Connected to GitLab CI/CD pipelines, NowSecure automatically tests on real devices for the deepest insights and test coverage. NowSecure delivers high accuracy with near zero false positives, pinpointing issues with full detail context for each issue and developer-level remediation instructions with code examples to speed resolution quickly, routing tickets automatically into GitLab Boards and other ticketing systems.
NowSecure is frequently used to perform security testing in parallel with functional testing in the dev cycle. Requires a license for and connection to the NowSecure Platform software. Learn more about NowSecure at https://www.nowsecure.com

## Getting Started

### Access token
Generate token as described in https://nowsecurehelp.zendesk.com/hc/en-us/articles/360034149691 (Note: customer sign in is required to access this resource). This token will be specified by environment variable `NOWSECURE_TOKEN`.

### Required Environment variables

- `NOWSECURE_GROUP=default_group` - Specifies group for your account
- `NOWSECURE_TOKEN=Access_Token` - Specifies token from your Platform account
- `NOWSECURE_BINARY_FILE=default_binary` - Path to Android apk or IOS ipa - this file must be mounted via volume for the access

**Note**: We recommend using secured environment variables in Gitlab to specify `NOWSECURE_GROUP` and `NOWSECURE_BINARY_FILE` values.

### Optional Environment variables

Following are optional parameters that can be set from environment variables:

- `NOWSECURE_MIN_WAIT=nn (default 30)` - Default max wait in minutes for the mobile analysis
- `NOWSECURE_MIN_SCORE=nn (default 50)` - Minimum score the app must have otherwise it would fail
- `NOWSECURE_ARTIFACTS_DIR=/home/gradle/artifacts` - Specifies artifacts directory where json files are stored


## Creating a Gitlab-CI Pipeline:
Here is a sample config that you can save under `.gitlab-ci.yml` in your mobile project. Please read https://docs.gitlab.com/ee/ci/pipelines/pipeline_architectures.html for more information on Gitlab Pipeline.
```yaml
nowsecure:
  stage: test
  image: nowsecure/gitlab-ci:latest
  variables:
    NOWSECURE_BINARY_FILE: test.apk
  script:
    - nowsecure.sh

stages:
  - build
  - test
  - deploy

image: alpine

build_a:
  stage: build
  script:
    - echo "Building...."

test_a:
  stage: test
  script:
    - echo "Testing..."

deploy_a:
  stage: deploy
  script:
    - echo "Deploying..."
```

## Adding Environment variables in Gitlab Pipeline
Select Settings option from your Gitlab project and then jump to `Variables` section to add environment variables for your pipeline, e.g.

![Gitlab Environment Add Variable](/images/gitlab_1a.png)

![Gitlab Environment Variables](/images/gitlab_2a.png)


## Submitting CI/CD Submitting Pipeline
The CI/CD will be run when you check-in new changes or you can select CI/CD option from your Gitlab project and then click on `Run Pipeline` to submit a pipeline, e.g. 

![Submit Pipeline](/images/gitlab_3.png)

![View Pipeline](/images/gitlab_4.png)

## Verifying the Build
Upon completion of CI/CD job, you will see a score of your mobile app. Note: you can configure your build to fail the CI/CD job when score is below a configurable miniumum value, e.g.

![View Score](/images/gitlab_5.png)

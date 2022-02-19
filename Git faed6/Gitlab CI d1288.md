# Gitlab CI

## Overview

There are some stages and some jobs. Each stage could have many jobs that are separate from each other and could be executed in parallel.

```yaml
tages:
  - build
  - test

build-code-job:
  stage: build
  script:
    - echo "Check the ruby version, then build some Ruby project files:"
    - ruby -v
    - rake

test-code-job1:
  stage: test
  script:
    - echo "If the files are built successfully, test some files with one command:"
    - rake test1

test-code-job2:
  stage: test
  script:
    - echo "If the files are built successfully, test other files with a different command:"
    - rake test2
```

## Defining variables

```yaml
variables:
  SA_PASSWORD: $SA_PASSWORD
```

Define for a single job

```yaml
job1:
  variables:
    TEST_VAR_JOB: "Only job1 can use this variable's value"
  script:
    - echo "$TEST_VAR" and "$TEST_VAR_JOB"
```

## Environments

environments describe where code is deployed.

There are two types of environments:

- Static environments have static names, like `staging` or `production`.

```yaml
job1:
  variables:
    TEST_VAR_JOB: "Only job1 can use this variable's value"
  script:
    - echo "$TEST_VAR" and "$TEST_VAR_JOB"
```

- Dynamic environments have dynamic names. Dynamic environments are a fundamental part of [Review apps](https://docs.gitlab.com/ee/ci/review_apps/index.html).

```yaml
deploy_review:
  stage: deploy
  script:
    - echo "Deploy a review app"
  environment:
    name: review/$CI_COMMIT_REF_SLUG
    url: https://$CI_ENVIRONMENT_SLUG.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
      when: never
    - if: $CI_COMMIT_BRANCH
```

## Define image

```yaml
default:
  image: ruby:2.6

  services:
    - postgres:11.7

  before_script:
    - bundle install

test:
  script:
    - bundle exec rake spec
```
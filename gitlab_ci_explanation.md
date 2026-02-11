# GitLab CI/CD

Quick reference for understanding GitLab CI/CD pipelines and runners.

## What is CI/CD?

**CI/CD** (Continuous Integration / Continuous Deployment) automates building, testing, and deploying your code:

- **CI** (Continuous Integration): Automatically build and test code on every commit
- **CD** (Continuous Deployment): Automatically deploy code to production or create releases

## GitLab Pipelines

### What is a Pipeline?

A **pipeline** is an automated workflow that runs when you push code to GitLab.

- Defined in a `.gitlab-ci.yml` file at the root of your repository
- Consists of **jobs** organized into **stages**
- Each job runs independently and can perform tasks like:
  - Building your application
  - Running tests
  - Creating releases
  - Deploying to servers

### Official documentation

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ee/ci/pipelines/)
- [.gitlab-ci.yml reference](https://docs.gitlab.com/ee/ci/yaml/)
- [Get started with GitLab CI/CD](https://docs.gitlab.com/ee/ci/quick_start/)

### Pipeline Stages

Pipelines are organized into **stages** that run sequentially:

1. **build** — Compile your application
2. **test** — Run automated tests
3. **deploy** — Deploy or create releases

Jobs in the same stage run in parallel. The next stage only starts if all jobs in the previous stage succeed.

## GitLab Runners

### What is a Runner?

A **runner** is an agent that executes the jobs in your pipeline.

- Runs on a machine (physical, virtual, or container)
- Can be shared (provided by GitLab.com) or self-hosted
- Can run on different operating systems: Linux, macOS, Windows

### Official documentation

- [GitLab Runner](https://docs.gitlab.com/runner/)
- [Install GitLab Runner](https://docs.gitlab.com/runner/install/)
- [Runner executors](https://docs.gitlab.com/runner/executors/)

### Types of Runners

| Type | Description |
|------|-------------|
| **Shared runners** | Provided by GitLab.com, available to all projects |
| **Group runners** | Available to all projects in a group |
| **Project runners** | Dedicated to a specific project |

### Runner Tags

Runners can have **tags** to target specific environments:

```yaml
build-macos:
  tags:
    - macos
  script:
    - make build
```

This job will only run on runners with the `macos` tag.

## Why Use CI/CD?

### Build on Every Commit

**Problem**: Manual builds are error-prone and inconsistent.

**Solution**: Automatically build your code on every commit:

- Catch build errors immediately
- Ensure code compiles in a clean environment
- Verify dependencies are properly defined
- Get feedback within minutes, not hours

### Make Releases on macOS Runner

**Use case**: Create macOS-specific releases automatically.

**Setup**:

1. **Install a macOS runner** on a Mac machine
2. **Tag the runner** with `macos`
3. **Create a release job** in `.gitlab-ci.yml`:

```yaml
release-macos:
  stage: deploy
  tags:
    - macos
  only:
    - tags
  script:
    - make build-release
    - ./upload-to-package-registry.sh
  artifacts:
    paths:
      - build/MyApp.dmg
```

**Benefits**:

- Releases are created automatically when you push a Git tag
- Build happens on a real macOS environment
- Binaries can be uploaded to the Generic Package Registry
- Consistent, reproducible builds
- No manual steps required

## Basic `.gitlab-ci.yml` Example

```yaml
stages:
  - build
  - test
  - deploy

build-job:
  stage: build
  script:
    - echo "Building the application..."
    - make build

test-job:
  stage: test
  script:
    - echo "Running tests..."
    - make test

release-job:
  stage: deploy
  tags:
    - macos
  only:
    - tags
  script:
    - echo "Creating release..."
    - make package
  artifacts:
    paths:
      - dist/
```

## Additional Resources

- [CI/CD examples](https://docs.gitlab.com/ee/ci/examples/)
- [Auto DevOps](https://docs.gitlab.com/ee/topics/autodevops/)
- [GitLab CI/CD variables](https://docs.gitlab.com/ee/ci/variables/)
- [Job artifacts](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)

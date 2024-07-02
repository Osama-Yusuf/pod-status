# pod-status

This repository contains a GitLab CI/CD pipeline configuration for managing Kubernetes deployments. The pipeline supports actions like retrieving pod status, restarting deployments, fetching logs, and getting environment variables of the pods.

### Required Environment Variables

When triggering the pipeline, the following environment variables must be provided:

1. **ACT** (required): Action to be performed. Supported values are `GET`, `LOG`, `RESTART`, and `GET_VARS`.
2. **ENV** (required): Environment in which the action is to be performed. Supported values are `sit`, `dev`, `ams`, and `uat`.
3. **POD** (required): Name of the microservice for which the action is to be performed.

## Pipeline Stages

The pipeline consists of three main stages:

1. **Setup**
2. **Triggers**
3. **Generic query on all pods**
4. **ACT-POD-ENV**

### Stages Description

1. **Setup**:
    
    * **generate-config**: This job dynamically generates a `.gitlab-ci.yml` file based on the provided environment variables. It uses a template file (`template-gitlab-ci.yml`) and replaces placeholders with the actual values.
2. **Triggers**:
    
    * **trigger-config**: This job triggers the dynamically generated configuration file to run the necessary actions based on the provided environment variables.

### Optional Environment Variables

* **MS_namespace**: This is set based on the provided `ENV` and is not required to be passed explicitly. The namespace is determined within the template file (`template-gitlab-ci.yml`).

## Pipeline Jobs

### 1. generate-config

Generates a dynamic GitLab CI configuration file based on the provided environment variables. The generated file will have stages and jobs tailored to the specified action and environment.

### 2. trigger-config

Triggers the dynamically generated configuration file to execute the specific job.

## Template File: `template-gitlab-ci.yml`

The template file includes definitions for the following jobs:

1. **get_PLACEHOLDER_JOB**:
    * **Action**: `GET`
    * **Description**: Retrieves the status of the specified pod in the given environment.
2. **restart_PLACEHOLDER_JOB**:
    * **Action**: `RESTART`
    * **Description**: Restarts the deployment associated with the specified pod in the given environment.
3. **log_PLACEHOLDER_JOB**:
    * **Action**: `LOG`
    * **Description**: Fetches the logs of the specified pod in the given environment.
4. **get_vars_PLACEHOLDER_JOB**:
    * **Action**: `GET_VARS`
    * **Description**: Retrieves the environment variables from the specified pod in the given environment.

## How to Trigger the Pipeline

To trigger the pipeline, the following environment variables need to be set in the GitLab CI/CD pipeline trigger or schedule:

```yaml
variables:
  ACT: <ACTION>
  ENV: <ENVIRONMENT>
  POD: <POD_NAME>
```

Example:

```yaml
variables:
  ACT: "GET"
  ENV: "dev"
  POD: "microservice-name"
```

## Scaling the Pipeline

To scale the pipeline, you can extend the `template-gitlab-ci.yml` with additional jobs as needed. Follow these steps:

1. Define a new job in the `template-gitlab-ci.yml` file, using placeholders similar to the existing jobs.
2. Ensure the new job is configured correctly to use the dynamic environment variables.
3. Modify the `generate-config` job in the main `.gitlab-ci.yml` file if additional placeholder replacements are required.
4. Trigger the pipeline with the appropriate environment variables

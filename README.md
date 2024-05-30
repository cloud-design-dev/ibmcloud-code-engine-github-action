# Code Engine Deploy GitHub Action

This GitHub Action allows you to Create or update Code Engine Apps, Jobs, and Functions in IBM Cloud. 

It offers flexibility for different deployment types and provides various configuration options. 

## Author

- Author: Ryan Tiffany

## Description

This action allows you to create or update IBM Cloud Code Engine workloads. It supports deploying applications, jobs, and functions and can be configured with the following inputs:

### Inputs

| Name            | Required | Default Value |Description |
|-----------------|----------|---------------|----------------------------------------------------------------|
| `ibmcloud_api_key` |  ✅  | -  | IBM Cloud API Key. Please store your IBM Cloud API key securely in your GitHub repository Secrets.|
| `resource_group` |  ✅  | -    | An IBM Cloud Resource Group, a logical container for organizing and managing related cloud resources.|
| `code_engine_region`  |  ✅  | - | The geographical area where your Code Engine project is located.|
| `code_engine_project` |  ✅   | - | A Code Engine Project, grouping your Apps, Functions, and Jobs.|
| `workload_type`        |  ✅   | - | The type of workload to create or update (App, Function, Job). |
| `workload_name`          |  ✅  | - | The name of the App, Function, or Job.|
| `function_runtime`       | ❌ | - | The runtime used for the Function. Currently supported `nodejs-18` and `python-3.11` see [IBM Code Engine Function Runtimes](https://cloud.ibm.com/docs/codeengine?topic=codeengine-fun-runtime) for more information.|
| `build_source`  | ❌ | . | Path to the directory containing the source code.|
| `workload_cpu`           | ❌ | - | CPU configuration for your workload. If not specified the Code Engine default is used. |
| `workload_memory`        | ❌ | - | Memory configuration for your workload. If not specified the Code Engine default is used. |
| `workload_port`          | ❌ | 8080 | Port configuration for your app workloads |


## Usage

To use this action add it to your GitHub Actions workflow YAML file. Here are examples of how to create or update an App, Job, and Function:



```yaml
name: Create or update workload in IBM Cloud Code Engine

on:
  push:
    branches:
      - main

env:
  CODE_ENGINE_REGION: "us-south"
  CODE_ENGINE_PROJECT: "Code Engine Project Name"
  WORKLOAD_NAME: "my-cool-app"

jobs:

  code-engine-app:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy Application to Code Engine
      uses: cloud-design-dev/ibmcloud-code-engine-github-action@v1
      with:
        ibmcloud_api_key: ${{ secrets.IBMCLOUD_API_KEY }}
        resource_group: 'Default'
        code_engine_region: ${{ env.CODE_ENGINE_REGION }}
        code_engine_project: ${{ env.CODE_ENGINE_PROJECT }}
        workload_type: 'app'
        workload_name: ${{ env.WORKLOAD_NAME }}
        workload_port: 8080
        build_source: './app-code'
        cpu: 1
        memory: 4G

  code-engine-job:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy Job to Code Engine
      uses: skywalkeretw/ibm-code-engine-github-action@v1
      with:
        ibmcloud_api_key: ${{ secrets.IBMCLOUD_API_KEY }}
        resource_group: 'Default'
        code_engine_region: ${{ env.CODE_ENGINE_REGION }}
        code_engine_project: ${{ env.CODE_ENGINE_PROJECT }}
        workload_type: 'job'
        workload_name: ${{ env.WORKLOAD_NAME }}
        build_source: './job-code'
        cpu: 1
        memory: 4G

  code-engine-fn-js:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy JavaScript Function to Code Engine
      uses: skywalkeretw/ibm-code-engine-github-action@v1
      with:
        ibmcloud_api_key: ${{ secrets.IBMCLOUD_API_KEY }}
        resource_group: 'Default'
        code_engine_region: ${{ env.CODE_ENGINE_REGION }}
        code_engine_project: ${{ env.CODE_ENGINE_PROJECT }}
        workload_type: 'fn'
        function_runtime: 'nodejs-18'
        workload_name: ${{ env.WORKLOAD_NAME }}
        build_source: './fn-js-code'
        cpu: 1
        memory: 4G

  code-engine-fn-py:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy JavaScript Function to Code Engine
      uses: skywalkeretw/ibm-code-engine-github-action@v1
      with:
        ibmcloud_api_key: ${{ secrets.IBMCLOUD_API_KEY }}
        resource_group: 'Default'
        code_engine_region: ${{ env.CODE_ENGINE_REGION }}
        code_engine_project: ${{ env.CODE_ENGINE_PROJECT }}
        workload_type: 'fn'
        function_runtime: 'python-3.11'
        workload_name: ${{ env.WORKLOAD_NAME }}
        build_source: './fn-python-code'
        cpu: 1
        memory: 4G

```

This action is not officially endorsed by IBM Cloud but can be used as a community-contributed GitHub Action.

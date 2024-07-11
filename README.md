# Azure_PublishPipelineArtifact.ado
Use this template to publish (upload) a file or directory as a named artifact for the current run in Azure Devops Pipeline.


## Parameters:

The pipeline requires the following parameters to be defined:


| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | ------------- | :-------------: | ------------- |
| targetPath | File or directory path | string | '$(Pipeline.Workspace)' | | Required | Specifies the path of the file or directory to publish. Can be absolute or relative to the default working directory. |
| artifact | Artifact name | string |  | | Optional | Specifies the name of the artifact to publish. It can be any name you choose, for example drop. If not set, the default is a unique ID scoped to the job. |
| publishLocation | Artifact publish location | string | 'pipeline' | pipeline (Azure Pipelines), filepath (A file share) | Required | Specifies whether to store the artifact in Azure Pipelines or to copy it to a file share that must be accessible from the pipeline agent. |
| fileSharePath | File share path | string |  | | Required when artifactType = filepath | Specifies the file share where the artifact files are copied. This can include variables |
| parallel | Parallel copy | boolean | false | | Optional. Use when artifactType = filepath | Specifies whether to copy files in parallel using multiple threads for greater potential throughput. If this setting is not enabled, one thread will be used. |
| parallelCount | Parallel count | string | '8' | | Optional. Use when artifactType = filepath && parallel = true | Specifies the degree of parallelism, or the number of threads used, to perform the copy. The value must be between 1 and 128. |
| properties | Custom properties | string | | | Optional | Specifies the custom properties to associate with the artifact. Use a valid JSON string with the prefix user- on all keys. |
--------------------------------------------------------------------------------------------------------------------------------------------------

These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

 ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_PublishPipelineArtifact.ado
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: Azure_PublishPipelineArtifact.yml
    parameters:
        targetPath: '${{parameters.targetPath}}'
        artifact: '${{parameters.artifact}}'
        publishLocation: '${{parameters.publishLocation}}'
        fileSharePath: '${{parameters.fileSharePath}}' 
        parallel: '${{parameters.parallel}}'
        parallelCount: '${{parameters.parallelCount}}'
        properties: '${{parameters.properties}}' 
  ```
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

## Q1 Scenario 1

----
A car rental company called FastCarz has a .net Web Application and Web API which are recently
migrated from on-premise system to Azure cloud using Azure Web App Service
and Web API Service.
The on-premises system had 3 environments Dev, QA and Prod.
The code repository was maintained in TFS and moved to Azure GIT now. The TFS has daily builds which
triggers every night which build the solution and copy the build package to drop folder.
deployments were done to the respective environment manually. The customer is planning to setup
Azure DevOps service for below requirements:

1. The build should trigger as soon as anyone in the dev team checks in code to the master branch.
Answer: The above process can be achieved using continuous Integration. 
Continuous Integration: Is a process of making small changes in our code repositories and every time we make these small change in repo it is going to trigger a process and that process going to execute a series of tasks in the defined order
    -	Build
    -	Test
    -	Analyse
    -	Security
    -  	Publish

For the above requirement, we can create/setup a Build pipeline in Azure DevOps for the respective branch. In the above scenario, the branch will be the Master branch.
    
    - Go to Respective Project
    - Click on Pipeline
    - Click on Create pipeline using YAML or Classic editor
    - Select repository in this it will be Azure repo
    - Select Master Branch
    - Then select the template (define, enable/disable respective jobs)
    - Go to Triggers tab/option. Enable the Continuous Integration option
        o	From Branch Filters select Master Branch
    - Click on Save and Que
    - Add comments
    - Click on Save and Run


2. There will be test projects which will create and maintained in the solution along the Web and API. The trigger should build all the 3 projects - Web, API and test.

*Answer*: Assuming all 3 project will have 3 different repositories and there will be Unit Tests for Application and API’s not Integration/Functional Tests. 
We can create a 3 pipelines for the 3 projects for each pipeline In build job (pipeline) we can define the Test step where it will test the application and API’s (Assuming unit tests). Usually, the build job contains below steps

    -	Restore
    -	Build
    -	Test
    -	Publish
    -	Publish Artifact
    
> `To trigger the pipeline one after another we can use Build Completion option from the Trigger tab/option. There we can define which pipeline to trigger/run.`

> `To mark the build as a failure if there are any unit tests failed/ technical debt, untick Continue on Error in control option in Test step.`

`We can have the similar option enabled for Test pipeline.`

3. The deployment of code and artifacts should be automated to the Dev environment.
Answer: With the help CI/CD we can achieve the deployment process to the respective environment by Release Pipeline

    -	Go to Pipelines
    -	Under pipelines select Releases
    -	Click on New release
    -	Just like build template, here we will get Release Templates. Select the respective template
    -	Define Stage name For eg: DEV
    -	Once the pipeline is created we have to add the generated artifacts from the build pipeline. 
    -	For automatic triggers/Continuous deployments:
    -	Go to release pipeline
    -	Edit 
    -	Click on the Bolt icon on top of artefacts.
    -	Enable the continuous deployment from the options


4. Upon successful deployment to the Dev environment, deployment should be easily promoted to QA and Prod through an automated process.
*Answer:*

    > We can have multiple Release pipeline name as QA/Production by cloning the existing one. 
    `QA: To reorganize the stages in the pipeline, choose the Pre-deployment conditions icon for the QA stage and set the trigger to After release.`
    
`Production: Choose the Pre-deployment conditions icon for the Production stage and set the trigger to After stage, then select QA in the Stages drop-down list.`



5. The deployments to QA and Prod should be enabled with Approvals from approvers only.
*Answer:* 
    > `We can enable the approval by selecting Pre-deployment conditions and enable Pre Deployment approval then enter approver name.`

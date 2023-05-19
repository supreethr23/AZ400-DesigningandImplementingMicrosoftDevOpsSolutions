# Lab 04: Enabling Continuous Integration with Azure Pipelines
# Student lab manual

## Lab overview

In this lab, you will learn how to configure continuous integration (CI) and continuous deployment (CD) for your applications using Build and Release in Azure Pipelines. This scriptable CI/CD system is both web-based and cross-platform, while also providing a modern interface for visualizing sophisticated workflows. Although we won’t demonstrate all of the cross-platform possibilities in this lab, it is important to point out that you can also build for iOS, Android, Java (using Ant, Maven, or Gradle) and Linux.

## Objectives

After you complete this lab, you will be able to:

-   Create a basic build pipeline from a template
-   Track and review a build
-   Invoke a continuous integration build

## Instructions

#### Set up an Azure DevOps organization.

1. On your lab VM open **Edge Browser** on desktop and navigate to https://go.microsoft.com/fwlink/?LinkId=307137. 

2. In the pop-up for *Help us protect your account*, select **Skip for now (14 days until this is required)**.

3. On the next page accept defaults and click on continue.
   
   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7ff3a8ba-6703-4624-87a7-e5f2817b19b3)
   
4. On the **Almost Done...** page fill the captcha and click on continue. 

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/17b7f53a-e09c-4fb1-8604-755e334303dc)


### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which consist of a new Azure DevOps project with a repository based on the **eShopOnWeb**.

#### Task 1: Create and configure the team project

In this task, you will create an **eShopOnWeb** Azure DevOps project to be used by several labs.

   1. On your lab computer, in a browser window open your Azure DevOps organization. Click on **New Project**. Give your project the name               **eShopOnWeb(1)** and leave the other fields with defaults. Click on **Create project(3)**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/9e2f68d4-65b1-40a9-9720-f0c6aaf54432)

**Task 2: (skip if done) Import eShopOnWeb Git Repository**

  In this task you will import the eShopOnWeb Git repository that will be used by several labs.
  
   1. On your lab computer, in a browser window open your Azure DevOps organization and the previously created eShopOnWeb project. Click on             **Repos(1)>Files(2) , Import a Repository**. Select **Import(3)**. On the **Import a Git Repository(4)** window, paste the following URL                     https://github.com/MicrosoftLearning/eShopOnWeb.git (5) and click **Import(6)**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f036be80-3530-4da7-99c9-ae11254d30de)
      
      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/8b265ea1-9526-4f10-b9b6-dd6917232725)

   2. The repository is organized the following way:

         o. **.ado** folder contains Azure DevOps YAML pipelines
         
         o **.devcontainer** folder container setup to develop using containers (either locally in VS Code or GitHub Codespaces)
         
         o **.azure** folder contains Bicep & ARM infrastructure as code templates used in some lab scenarios.
         
         o **.github** folder contains YAML GitHub workflow definitions.
         
         o. **src** folder contains the .NET 6 website used in the lab scenarios.
         
       ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/207304b4-e11e-4cb7-a6e3-ed73a0247a42)
         
 **Exercise 1: Include build validation as part of a Pull Request**
 
 In this exercise, you will include build validation to validate a Pull Request.
 
 **Task 1: Import the YAML build definition**
 
 In this task, you will import the YAML build definition that will be used as a Branch Policy to validate the pull requests.
 
 Let's start by importing the build pipeline named **eshoponweb-ci-pr.yml**.
 
   1. Go to **Pipelines(1)>Pipelines(2)**. Click on **Create Pipeline(3)** or **New Pipeline** button.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/5bd60911-3cb8-48f5-8d70-169d3191ee17)   

   2. Select **Azure Repos Git (YAML)**

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/da618915-df41-4523-91a1-24bbbf505eb5)

   3. Select the **eShopOnWeb** repository.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/22afdccf-4ea1-4f93-9a31-6b59aa607d97)

   4. Select **Existing Azure Pipelines YAML File**

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/61f93510-e853-4c27-a282-17ecca69ffa9)

   5. Select the **/.ado/eshoponweb-ci-pr.yml(1)** file then click on **Continue(2)**

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/09d727b5-d063-422e-9e91-ee03a3609d91)

       The build definition consists of the following tasks:
      
         o **DotNet Restore:** With NuGet Package Restore you can install all your project's dependency without having to store them in source                   control. 
        
         o **DotNet Build:** Builds a project and all of its dependencies.
        
         o **DotNet Test:** .Net test driver used to execute unit tests.
        
         o **DotNet Publish:** Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's                 **Build.ArtifactStagingDirectory**.
        
        ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/9ae5acd4-53b1-4164-9900-ff36d8141b8c)

   6. Click the **Save** button to save the pipeline definition

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7a2fd575-884e-4e57-9e51-5c0aa8dfc943)
     
   7. Your pipeline will take a name based on the project name. Let's **rename** it for identifying the pipeline better. Go to **Pipelines>Pipelines** and click on the recently created pipeline. Click on the **ellipsis(1)** and **Rename/Remove(2)** option.
   
      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/c06b4766-18a0-485f-8f3a-efc2cc3a728b)

   8. Name it **eshoponweb-ci-pr(1)** and click on **Save(2)**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a5fcc447-cf46-4aa4-b485-3a677ffc760b)    

**Task 2: Branch Policies**

In this task, you will add policies to the main branch and only allow changes using Pull Requests that comply with the defined policies. You want to ensure that changes in a branch are reviewed before they are merged.

   1. Go to **Repos(1)>Branches(2)** section. On the **Mine** tab of the **Branches** pane, hover the mouse pointer over the **main(3)** branch entry to reveal the **ellipsis symbol(4)** on the right side.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/2bd33ef6-01ca-4d9f-b943-d7f6189cca65)

   2. Click the **ellipsis(4)** and, in the pop-up menu, select **Branch Policies(5)**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0571e69e-7e32-44e7-b065-9749e04a6208)

   3. On the main tab of the repository settings, enable the option for **Require minimum number of reviewers(1)**. Add **1(2)** reviewer and check the box **Allow requestors to approve their own changes(3)**(as you are the only user in your project for the lab)

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7d05a76f-60d0-47a6-b5d6-b1751b4c7abc)

   4. On the **main(1)** tab of the repository settings, in the **Build Validation(2)** section, **click + (Add a new build policy)(3)** and in the Build pipeline list, select **eshoponweb-ci-pr(4)** then click **Save(5)**

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/40de106f-8b38-4f31-b711-ed68d5a2b5e1)

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/8a425e6c-4fc2-4659-92e6-7e7a3d5de51c)

 **Task 3: Working with Pull Requests**
 
 In this task, you will use the Azure DevOps portal to create a Pull Request, using a new branch to merge a change into the protected main branch.
 
 1. Navigate to the **Repos(1)->Branches(2)** section in the eShopOnWeb navigation and click **New Branch(3)**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ece4471b-3036-4a86-b215-634e6623ed5b)

 2. Create a new branch named **Feature01(1)** based on the **main** branch and click **Create(2)**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ad648c24-a981-4049-8fa9-12e15d6b9223)

3. Click **Feature01(1)** and navigate to the **/eShopOnWeb/src(2)/Web(3)/Program.cs(4)** file as part of the **Feature01** branch and click on **edit(5)** to make the following change on the first line:

   ```
   // Testing my PR
   ```

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/3363744c-c742-4d3e-807a-af4deee0f09f)

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/e732d4d4-f216-4c0f-addd-70c28eaf2a45)
   
 4. Click on **Commit > Commit** (leave default commit message).

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ab9929d4-9df7-47b5-8861-e64c35fdfe0a)
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/856c8ce9-61af-445a-9543-9179022536c2)

 5. A message will pop-up, proposing to create a Pull Request (as your **Feature01** branch is now ahead in changes, compared to **main**). Click on **Create a Pull Request(1)**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b2b2b35f-6086-4573-a254-58de8902c019)

6. In the **New pull request(1)** tab, leave defaults and click on **Create(2)**.
   
   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d30f60cc-c213-4916-a0f8-08c0fc47cdab)
   
7. The Pull Request will show some pending requirements, based on the policies applied to the target **main** branch.

     o **At least 1 user** (1) should review and approve the changes (**Add(2)** required approver and **select(3)** the approver to complete the PR).
     o Build validation, you will see that the build **eshoponweb-ci-pr** was triggered automatically
     
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/6ddfba1a-74c8-4250-ba16-7feca31278c2)
     
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/4bda9092-c8c4-428d-ab32-b1abbf66e143)    
      
8. After all validations are successful, on the top-right click on **Approve(1)**. Now from the **Set auto-complete dropdown(2)** you can click on **Complete(3)**.  

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/1f8be18a-d0fd-4861-aeb0-b60612eb370c)
  
9. On the **Complete Pull Request** tab, click on **Complete Merge**

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/6b5c35f3-0307-4caf-a89d-1e2007085a30)

**Exercise 2: Configure CI Pipeline as Code with YAML**

  In this exercise, you will configure CI Pipeline as code with YAML.

**Task 1: Import the YAML build definition**

  In this task, you will add the YAML build definition that will be used to implement the Continuous Integration.

  Let's start by importing the CI pipeline named **eshoponweb-ci.yml**.

  1. Go to **Pipelines>Pipelines(1)** and click on **New Pipeline(2)** button

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/268df693-0604-47e8-9b34-6e9cdfda4e17)

  2. Select **Azure Repos Git (YAML)**

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/da618915-df41-4523-91a1-24bbbf505eb5)

  3. Select the **eShopOnWeb** repository

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/22afdccf-4ea1-4f93-9a31-6b59aa607d97)

  4. Select **Existing Azure Pipelines YAML File**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/61f93510-e853-4c27-a282-17ecca69ffa9)

  5. Select the **/.ado/eshoponweb-ci.yml(1)** file then click on **Continue(2)**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/9b5c7e10-875f-477d-b8a9-7126b1aea7b0)

     The CI definition consists of the following tasks:
     
   o **DotNet Restore:** With NuGet Package Restore you can install all your project's dependency without having to store them in source control.
       
   o **DotNet Build:** Builds a project and all of its dependencies.
       
   o **DotNet Test:** .Net test driver used to execute unit tests.
       
   o **DotNet Publish:** Publishes the application and its dependencies to a folder for deployment to a hosting system. In this case, it's             **Build.ArtifactStagingDirectory**.
       
   o **Publish Artifact - Website:** Publish the app artifact (created in the previous step) and make it available as a pipeline artifact.
       
   o **Publish Artifact - Bicep:** Publish the infrastructure artifact (Bicep file) and make it available as a pipeline artifact.
       
              
   **Task 2: Enable Continuous Integration**
   
   The default build pipeline definition doesn't enable Continuous Integration
   
   1. Now, you need to replace the **trigger: none** code with the following code:
   
      ```
        trigger:
        branches:
          include:
          - main
        paths:
          include:
          - src/web/*
      ``` 
   
      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7a44b1d7-578d-4468-9331-d0857fabdf80)
   
      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/2b6ded54-6d9c-427b-b516-fefe3631ba8a)

      This will automatically trigger the build pipeline if any change is made to the main branch and the web application code (the src/web folder).Since you enabled Branch Policies, you need to pass by a Pull Request in order to update your code. 
    
  2. Click the **Dropdown** and **Save(2)** button (not **Save and run**) to save the pipeline definition.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0f563873-ac78-438b-865a-46bf8d880b95)
  
  3. Select **Create a new branch for this commit(1)** Keep the default branch name and **Start a pull request(2)** checked. and Click on **Save(3)**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d10c6eb2-38b6-4415-8ec8-82bc3f96e695)

  4. Your pipeline will take a name based on the project name. Let's **rename** it for identifying the pipeline better. Go to                          **Pipelines>Pipelines** and click on the recently created pipeline. Click on the **ellipsis(1)** and **Rename/Remove** option. Name it            **eshoponweb-ci(2)**   and click on **Save(3)**.

     <img width="826" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/3ba3897a-b43d-4d27-89d3-0e8e8cba9d1b">

  5. Go to **Repos(1)>Pullrequests(2)** and click on the existing pull request. After all validations are successful, on the top-right click on        **Approve(3)**. Now you can click on **Complete(4)**.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d8d00033-647d-4914-b92d-5fd1dcb67fca)

  6. On the **Complete Pull Request** tab, Click on **Complete Merge**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/999bbacb-aa0c-4d35-9b25-44ae05a6f414)

   **Task 3: Test the CI pipeline**
 
 In this task, you will create a Pull Request, using a new branch to merge a change into the protected main branch and automatically trigger the CI pipeline Navigate to the Repos section
 
 1. Navigate to the **Repos(1)->Branches(2)** section. Create a **new branch(3)** named **Feature02(4)** based on the **main** branch and Click on **Create(5)**

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/2b6bd299-9d55-4c83-820c-6598ecc32aae)
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0d2c67fa-41ac-4884-9220-367d396d6ce3)

 2. Click the new **Feature02(1)** branch and navigate to the **/eShopOnWeb/src(2)/Web(3)/Program.cs(4)** file and click on **Edit(5)** to remove the first line and click on commit.

    ```
     // Testing my PR (6)
    ```
   
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b82cb9de-06de-4cce-ae50-afaa5bbc0ae1)
   
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/90ac7a58-d510-4ec5-bb22-3bb92b3858fa)

 3. Click on **Commit > Commit** (leave default commit message).
   
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f73c1d6e-c295-4df8-bcf2-5e96c4c4432a)

 4. A message will pop-up, proposing to create a Pull Request (as your **Feature02** branch is now ahead in changes, compared to main).

 5. Click on **Create a Pull Request**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/bab7b162-256a-4cb3-8816-09c109904e35)

 6. In the **New pull request(1)** tab, leave defaults and click on **Create(3)** The Pull Request will show some pending requirements, based         on the policies applied to the target **main(2)** branch.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/644d5804-d059-4791-b773-c8d601ba2867)

 7. After all validations are successful, on the top-right click on **Approve(1)**. Now from the **Set auto-complete** dropdown you can click       on **Complete(2)**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/95e82965-e365-4544-b901-032680fa07c1)

 8. On the **Complete Pull Request** tab, Click on **Complete Merge**

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/fa6bf84b-9b4d-49fe-ba3a-566f2584bd00)

 9. Go back to **Pipelines>Pipelines,** you will notice that the build **eshoponweb-ci** was triggered automatically after the code was            merged.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/4ed89433-01fb-4877-9b07-ca0c59fa4386)
 
 10. Click on the **eshoponweb-ci** build then select the last run.

       ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/78d2d488-bf0c-492f-b70a-2e7b77584863)


 11. After its successful execution, click on **Related(1) > Published(2)** to check the published artifacts:
           
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/598b475b-6a0b-46a4-bf07-92d3a525b578)   
     
     o Bicep: the infrastructure artifact  
     o Website: the app artifact
     
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/e629c2b2-052d-448a-9551-c2cf5443ae90)
          
  **Review**
  
  In this lab, you enabled pull request validation using a build definition and configured CI pipeline as code with YAML in Azure DevOps. 

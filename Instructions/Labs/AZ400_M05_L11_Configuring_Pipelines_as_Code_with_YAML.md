# Lab 11: Configuring Pipelines as Code with YAML
# Student lab manual

## Lab overview

Azure DevOps supports two types of version control, Git and Team Foundation Version Control (TFVC). Here is a quick overview of the two version control systems:

- **Team Foundation Version Control (TFVC)**: TFVC is a centralized version control system. Typically, team members have only one version of each file on their dev machines. Historical data is maintained only on the server. Branches are path-based and created on the server.

- **Git**: Git is a distributed version control system. Git repositories can live locally (such as on a developer's machine). Each developer has a copy of the source repository on their dev machine. Developers can commit each set of changes on their dev machine and perform version control operations such as history and compare without a network connection.

Git is the default version control provider for new projects. You should use Git for version control in your projects unless you have a specific need for centralized version control features in TFVC.

In this lab, you will learn how to establish a local Git repository, which can easily be synchronized with a centralized Git repository in Azure DevOps. In addition, you will learn about Git branching and merging support. You will use Visual Studio Code, but the same processes apply for using any Git-compatible client.

## Objectives

After you complete this lab, you will be able to:

-   configure CI/CD pipelines as code with YAML in Azure DevOps

## Instructions

#### Set up an Azure DevOps organization

1. On your lab VM open **Edge Browser** on desktop and navigate to https://dev.azure.com. Then click on **Sign into Azure DevOps** and login with the credentials provided in environment details tab.

    ![Azure DevOps](images/devops.png)

2. On the next page accept defaults and click on continue.

    ![Azure DevOps](images/m1-1.png)

3. On the **Get started with Azure DevOps** page click on **Continue**.

4. On the **Almost Done...** page fill the captcha and click on continue. 

    ![Azure DevOps](images/m1-2.png)

5. On the Azure Devops page click on **Azure DevOps** located at top left corner and then click on **Organization Setting** at the left down corner

    ![Azure DevOps](images/agent1.png)

6. In the **Organization Setting** window on the left menu click on **Billing** and select **Setup Billing** then click on save.

    ![Azure DevOps](images/agent3.png)
    ![Azure DevOps](images/agent4.png)    

7. On the **MS Hosted CI/CD** section under **Paid parallel jobs** enter value **1** and at the end of the page click on **Save**.

    ![Azure DevOps](images/agent2.png)

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which consist of a new Azure DevOps project with a repository based on the [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb).

#### Task 1:  (skip if done) Create and configure the team project

In this task, you will create an **eShopOnWeb_ReleaseGates** Azure DevOps project to be used by several labs.

1.  On your lab computer, in a browser window open your Azure DevOps organization. Click on **New Project**. Give your project the name **eShopOnWeb_ReleaseGates** and leave the other fields with defaults. Click on **Create**.

    ![Create Project](images/create-project.png)

#### Task 2:  (skip if done) Import eShopOnWeb Git Repository

In this task you will import the eShopOnWeb Git repository that will be used by several labs.

1.  On your lab computer, in a browser window open your Azure DevOps organization and the previously created **eShopOnWeb_ReleaseGates** project. Click on **Repos>Files** , **Import a Repository**. Select **Import**. On the **Import a Git Repository** window, paste the following URL https://github.com/MicrosoftLearning/eShopOnWeb.git  and click **Import**:

    ![Import Repository](images/import-repo.png)

1.  The repository is organized the following way:
    - **.ado** folder contains Azure DevOps YAML pipelines
    - **.devcontainer** folder container setup to develop using containers (either locally in VS Code or GitHub Codespaces)
    - **.azure** folder contains Bicep&ARM infrastructure as code templates used in some lab scenarios.
    - **.github** folder container YAML GitHub workflow definitions.
    - **src** folder contains the .NET 6 website used on the lab scenarios.


#### Task 2: Create two Azure web apps

In this task, you will create two Azure web apps representing the **Canary** and **Production** environments, into which you'll deploy the application via Azure Pipelines.

1. From the lab computer, start a web browser, navigate to the [**Azure Portal**](https://portal.azure.com), and sign in with the user account that has the Owner role in the Azure subscription you will be using in this lab and has the role of the Global Administrator in the Azure AD tenant associated with this subscription.
1. In the Azure portal, click the **Cloud Shell** icon, located directly to the right of the search textbox at the top of the page.
1. If prompted to select either **Bash** or **PowerShell**, select **Bash**.

    >**Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and select **Create storage**.

1. From the **Bash** prompt, in the **Cloud Shell** pane, run the following command to create a resource group (replace the `<region>` variable placeholder with the name of the Azure region that will host the two Azure web apps, for example 'westeurope' or 'centralus' or any other available region of your choice):

    > **Note**: possible locations can be found by running the following command, use the **Name** on `<region>` : `az account list-locations -o table`

    ```bash
    REGION='centralus'
    RESOURCEGROUPNAME='az400m04l09-RG'
    az group create -n $RESOURCEGROUPNAME -l $REGION
    ```

1. To create an App service plan

    ```bash
    SERVICEPLANNAME='az400m04l09-sp1'
    az appservice plan create -g $RESOURCEGROUPNAME -n $SERVICEPLANNAME --sku S1
    ```

1.  Create two web apps with unique app names.
 
     ```bash
     SUFFIX=$RANDOM$RANDOM
     az webapp create -g $RESOURCEGROUPNAME -p $SERVICEPLANNAME -n RGATES$SUFFIX-Canary
     az webapp create -g $RESOURCEGROUPNAME -p $SERVICEPLANNAME -n RGATES$SUFFIX-Prod
     ```

    > **Note**: Record the name of the Canary web app. You will need it later in this lab.

1. Wait for the Web App Services Resources provisioning process to complete and close the **Cloud Shell** pane.

#### Task 3: Configure an Application Insights resource

1. In the Azure portal, use the **Search resources, services, and docs** text box at the top of the page to search for **Application Insights** and, in the list of results, select **Application Insights**.
1. On the **Application Insights** blade, select **+ Create**.
1. On the **Application Insights** blade, on the **Basics** tab, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Resource group | **az400m04l09-RG** |
    | Name | the name of the Canary web app you recorded in the previous task |
    | Region | the same Azure region to which you deployed the web apps earlier in the previous task |
    | Resource Mode | **Classic** |

    > **Note**: Disregard the deprecation message. This is required in order to prevent failures of the Enable Continuous Integration DevOps task you will be using later in this lab.

1. Click **Review + create** and then click **Create**.
1. Wait for the provisioning process to complete.
1. In the Azure portal, navigate to the resource group **az400m04l09-RG** you created in the previous task.
1. In the list of resources, click the **Canary** web app.
1. On the **Canary** web app page, in the vertical menu on the left, in the **Settings** section, click **Application Insights**.
1. On the **Application Insights** blade, click **Turn on Application Insights**.
1. In the **Change your resource** section, click the **Select existing resource** option, in the list of existing resources, select the newly created Application Insight resource, click **Apply** and, when prompted for confirmation, click **Yes**.
1. Wait until the change takes effect.

    > **Note**: You will create monitor alerts here, which you will use in later part of this lab.
1. From the same **Settings** / **Application Insights** menu option, select **View Application Insight Data**. This redirects you to the Application Insights blade in the Azure Portal.
1.  On the Application Insights resource blade, in the **Monitoring** section, click **Alerts** and then click **Create > Alert rule**.
1.  On the **Select a signal** blade, in the **Search by signal name** textbox, type **Failed Requests** and select it. 
1.  On the **Create an Alert Rule** blade, in the **Condition** section, leave the **Threshold** set to **Static**, validate the other default settings as follows:
- Aggregation Type: Count
- Operator: Greater Than
- Unit: Count
1. In the **Threshold value** textbox, type **0**, and click on **Next:Actions**. Don't make any changes in the **Actions** settings blade, and define the following parameters under the **Details** section:

    | Setting | Value |
    | --- | --- |
    | Severity | **2- Warning** |
    | Alert rule name | **RGATESCanary_FailedRequests** |
    | Advanced Options: Automatically resolve alerts | **cleared** |

    > **Note**: Metric alert rules might take up to 10 minutes to activate.

    > **Note**: You can create multiple alert rules on different metrics such as availability < 99 percent, server response time > 5 Seconds, or server exceptions > 0

1. Confirm the creation of the Alert rule by clicking **Review+Create**, and confirm once more by clicking **Create**. Wait for the alert rule to get created successfully.

### Exercise 1: Configure the build pipeline

In this exercise, you will prepare the release artifact (web deploy zip package) for the .NET 6 eShopOnWeb web application source code.

1. From the [Azure DevOps portal](https://dev.azure.com), navigate to the **eShopOnWeb_ReleaseGates** project you created earlier in this lab.
1. From within the Project, navigate to **Pipelines**.
1. Click **Create Pipelines**.
1. From the **Where is your Code** step, select **Azure Repos(Git)**.
1. In the **Select a repository**, select **eShopOnWeb_ReleaseGates**.
1. From the **Configure your pipeline** step, scroll down in the list and select **Existing Azure Pipelines YAML File**
1. Under the **Select an existing YAML file**, select **main** as Branch, and select **/.ado/eshoponweb-ci.yml** as Path.
1. Click **Continue**.
1. From the YAML pipeline screen, click **Run** to kick off the Build Pipeline.
1. After a few seconds, the status of the Job will switch to **Running**. Ignore any warnings regarding the source code.
1. Click on the **Build** Status line to navigate to the detailed **Running Job** window. 
1. Wait for all steps to be completed successfully (green checkmarks). This should take around 2-3min on average.
1. Once the Build Pipeline Job completed successfully, validate the Build Artifacts. From the Job Status page, navigate to the **Summary** tab. Under **Related**, it shows **2 published**. Select this link. 
1. From the **Artifacts** page appearing, see **>Bicep** and **>WebSite**.
1. Open **WebSite**. Notice **BlazorAdmin.zip** and **Web.zip**. We will use the **Web.zip** artifact in our upcoming Release Pipeline.

### Exercise 2: Configure the release pipeline

In this exercise, you will configure a release pipeline.

#### Task 1: Set Up Release Tasks

In this task, you will set up the release tasks as part of the Release Pipeline.

1. From the **eShopOnWeb_ReleaseGates** project in the Azure DevOps portal, in the vertical navigational pane, select **Pipelines** and then, within the **Pipelines** section, click **Releases**.
1. Click **New Pipeline**.
1. From the **Select a template** window, **choose** **Azure App Service Deployment** (Deploy your application to Azure App Service. Choose from Web App on Windows, Linux, containers, Function Apps, or WebJobs) under the **Featured** list of templates.
1. Click **Apply**.
1. From the **Stage** window appearing, update the default "Stage 1" Stage Name to **Canary**. Close the popup window by using the **X** button. You are now in the graphical editor of the Release Pipeline, showing the Canary Stage.
1. Hover the mouse over the Canary Stage, and click the **Clone** button, to copy the Canary Stage to an additional Stage. Name this Stage **Production**.

    > **Note**: The pipeline now contains two stages named **Canary** and **Production**.

1. On the **Pipeline** tab, select the **Add an Artifact** rectangle, and select the **eShopOnWeb_ReleaseGates** in the **Source (build pipeline)** field. Click **Add** to confirm the selection of the artifact.
1. From the **Artifact** rectangle, notice the **Continuous Integration Trigger** (lightning bolt) appearing. Click it to open the **Continuous deployment trigger** settings. Click the continuous deployment trigger to toggle the switch to enable it. Leave all other settings at default and close the **Continuous deployment trigger** pane, by clicking the **x** mark in its upper right corner.
1. Within the **Canary Environments** stage, click the **1 job, 2 tasks** label and review the tasks within this stage.

    > **Note**: The canary environment has 1 task which, respectively, publishes the artifact package to Azure Web App.

1. On the **All pipelines > New Release Pipeline** pane, ensure that the **Canary** stage is selected. In the **Azure subscription** dropdown list, select your Azure subscription and click **Authorize**. If prompted, authenticate by using the user account with the Owner role in the Azure subscription.
1. Confirm the App Type is set to "Web App on Windows". Next, in the **App Service name** dropdown list, select the name of the **Canary** web app.
1. Select the Task **Deploy Azure App Service**. In the **Package or Folder** field, update the default value of "$(System.DefaultWorkingDirectory)/**/*.zip" to "$(System.DefaultWorkingDirectory)/**/Web.zip"** 

    > notice an exclamation mark next to the Tasks tab. This is expected, as we need to configure the settings for the Production Stage.

1. On the **All pipelines > New Release Pipeline** pane, navigate to the **Pipeline** tab, and this time, within the **Production** Stage, click the **1 job, 2 tasks** label. Similar to the Canary stage earlier, complete the pipeline settings. Under the Tasks tab / Production Deployment process, in the **Azure subscription** dropdown list, select the Azure subscription you used for the **Canary Environment** stage, shown under **Available Azure Service connections**, as we already created the service connection before when authorizing the subscription use.
1. In the **App Service name** dropdown list, select the name of the **Prod** web app.
1. Select the Task **Deploy Azure App Service**. In the **Package or Folder** field, update the default value of "$(System.DefaultWorkingDirectory)/**/*.zip" to "$(System.DefaultWorkingDirectory)/**/Web.zip"** 
1. On the **All pipelines > New Release Pipeline** pane, click **Save** and, in the **Save** dialog box, click **OK**.

You have now successfully configured the Release Pipeline.

1. In the browser window displaying the **EshopOnWeb_ReleaseGates** project, in the vertical navigational pane, in the **Pipelines** section, click **Pipelines**.
1. On the **Pipelines** pane, click the entry representing **eShopOnWeb_ReleaseGates** build pipeline and then, on the **eShopOnWeb_ReleaseGates** pane, click on **Run Pipeline**.
1. On the **Run pipeline** pane, accept the default settings and click **Run** to trigger the pipeline. **Wait for the build pipeline to finish**.

    > **Note**: After the build succeeds, the release will be triggered automatically and the application will be deployed to both the environments. Validate the release actions, once the build pipeline completed successfully.

1. In the vertical navigational pane, in the **Pipelines** section, click **Releases** and, on the **eShopOnWeb_ReleaseGates** pane, click the entry representing the most recent release.
1. On the **eShopOnWeb_ReleaseGates > Release-1** blade, track the progress of the release and verify that the deployment to both web apps completed successfully.
1. Switch to the Azure portal interface, navigate to the resource group **az400m04l09-RG**, in the list of resources, click the **Canary** web app, on the web app blade, click **Browse**, and verify that the web page (E-commerce website) loads successfully in a new web browser tab.
1. Switch back to the Azure portal interface, this time navigating  to the resource group **az400m04l09-RG**, in the list of resources, click the **Production** web app, on the web app blade, click **Browse**, and verify that the web page loads successfully in a new web browser tab.
1. Close the web browser tab displaying the **EShopOnWeb** web site.

    > **Note**: Now you have the application with CI/CD configured. In the next exercise we will set up Quality Gates as part of a more advanced  Release pipeline.

### Exercise 3: Configure release gates

In this exercise, you will set up Quality Gates in the release pipeline.

#### Task 1: Configure pre-deployment gates for approvals

In this task, you will configure pre-deployment gates.

1. Switch to the web browser window displaying the Azure DevOps portal, and open the **EshopOnWeb_ReleaseGates** project. In the vertical navigational pane, in the **Pipelines** section, click **Releases** and, on the **New Release Pipeline** pane, click **Edit**.
1. On the **All pipelines > New Release Pipeline** pane, on the left edge of the rectangle representing the **Canary Environment** stage, click the oval shape representing the **Pre-deployment conditions**.
1. On **Pre-deployment conditions** pane, set the **Pre-deployment approvals** slider to **Enabled** and, in the **Approvers** text box, type and select your Azure DevOps account name.

    > **Note**: In a real-life scenario, this should be a DevOps Team name alias instead of your own name.

1. **Save** the pre-approval settings and close the popup window.
1. Click **Create Release** and confirm by pressing the **Create** button from the popup window.
1. Notice the green confirmation message, saying "Release-2" has been created. Click the link of "Release-2" to navigate to its details.
1. Notice the **Canary** Stage is in a **Pending Approval** state. Click the **Approve** button. This sets off the Canary Stage again.

#### Task 2: Configure post-deployment gates for Azure Monitor

In this task, you will enable the post-deployment gate for the Canary Environment.

1.  Back on the **All pipelines > New Release Pipeline** pane, on the right edge of the rectangle representing the **Canary Environment** stage, click the oval shape representing the **Post-deployment conditions**.
1.  On **Post-deployment conditions** pane, set the **Gates** slider to **Enabled**, click **+ Add**, and, in the pop-up menu, click **Query Azure Monitor Alerts**.
1.  On **Post-deployment conditions** pane, in the **Query Azure Monitor Alerts** section, in the **Azure subscription** dropdown list, select the **service connection** entry representing the connection to your Azure subscription, and, in the **Resource group** dropdown list, select the **az400m04l09-RG** entry.
1. On the **Post-deployment conditions** pane, expand the **Advanced** section and configure the following options:

- Filter type: **None**
- Severity: **Sev0, Sev1, Sev2, Sev3, Sev4**
- Time Range: **Past Hour**
- Alert State: **Acknowledged, New**
- Monitor Condition: **Fired**

1.  On **Post-deployment conditions** pane, expand the **Evaluation options** and configure the following options:

- Set the value of **Time between re-evaluation of gates** to **5 Minutes**.
- Set the value of **Timeout after which gates fail** to **8 Minutes**.
- Select the **On successful gates, ask for approvals** option.

    > **Note**: The sampling interval and timeout work together so that the gates will call their functions at suitable intervals and reject the deployment if they don't succeed during the same sampling interval within the timeout period.

1. Close the **Post-deployment conditions** pane, by clicking the **x** mark in its upper right corner.
1. Back on the **New Release Pipeline** pane, click **Save**, and in the **Save** dialog box, click **OK**.

### Exercise 4: Test release gates

In this exercise, you will test the release gates by updating the application, which will trigger a deployment.

#### Task 1: Update and deploy application after adding release gates

In this task, you will track the release process with the release gates enabled.

1. From the Azure Portal, in the "Search resources, services and docs" field, enter **Alerts** to open the Alerts Service of Azure Monitor.
1. Notice there should be at least **1 Alert** with **Severity 2 - Warning** showing up in the list. This got trigger when you validated the website in the previous exercise.

    > ** Note:** If no Alert shows up yet, wait another few minutes. Speeding up Alerts is possible by navigating to the Canary-URL again from a browser.

1. Return back to the Azure DevOps Portal, open the **EShopOnWeb-Release Gates** Project. Navigate to **Pipelines**, select **Releases** and select the **New Release Pipeline**.
1. Click the **Create Release** button.
1. Wait for the Release pipeline to kick off, and **approve** the Canary Stage release action.
1. Wait for the Canary release Stage to complete successfully. Notice how the **Post-deployment Gates** is switching to an **Evaluation Gates** status.  Click the **Evaluation Gates** icon.
1. For the **Query Azure Monitor Alerts**, notice an initial failed state.
1. Let the Release pipeline in a pending state for the next 5 minutes. After the 5 minutes did pass, notice the 2nd evaluation failing again.
1. This is expected behavior, since there is an Application Insights Alerts triggered for the Canary Web App.

    > **Note**: Since there is an alert triggered by the exception, **Query Azure Monitor** gate will fail. This, in turn, will prevent deployment to the **Production** environment.

1. Wait another 3 minutes and validate the status of the Release Gates again. As it is now +8 minutes after the initial Release Gates got checked, and it's been more than 8 minutes since the initial Application Insight Alert got triggered with action "Fired", it should result in a successful Release Gate, having allowed the deployment of the Production Release Stage as well.


### Exercise 2: Remove the Azure DevOps billing

In this exercise, you will remove the Azure DevOps billing enabled in this lab to eliminate unexpected charges. 

#### Task 1: Remove the Azure DevOps billing 
    
In this task, you will remove pipeline billing to eliminate unnecessary charges.
    
1.  On the lab computer, switch to the browser window displaying Azure DevOps organization homepage and select **Organization Settings** at bottom left corner.
    
1.  Under **Organization Settings** select **Billing** and click on **Change billing** button to open Change billing pane.
    
1.  In the **Change billing** pane, select **Remove billing** setting and click on Save.

## Review

In this lab, you configured CI/CD pipelines as code with YAML in Azure DevOps.

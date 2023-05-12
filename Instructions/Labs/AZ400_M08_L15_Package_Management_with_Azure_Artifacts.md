# Lab 15: Package Management with Azure Artifacts
# Student lab manual

## Lab overview

Azure Artifacts facilitate discovery, installation, and publishing NuGet, npm, and Maven packages in Azure DevOps. It's deeply integrated with other Azure DevOps features such as Build, making package management a seamless part of your existing workflows.

## Objectives

After you complete this lab, you will be able to:

-  Create and connect to a feed.
-  Create and publish a NuGet package.
-  Import a NuGet package.
-  Update a NuGet package.


## Instructions

#### Set up an Azure DevOps organization
1. On your lab VM open **Edge Browser** on desktop and navigate to https://go.microsoft.com/fwlink/?LinkId=307137, and if prompted sign with the credentials provided in environment details tab.

2. In the pop-up for *Help us protect your account*, select **Skip for now (14 days until this is required)**.

3. On the next page accept defaults and click on continue.

    ![Azure DevOps](images/400-3.png)

4. On the **Almost Done...** page fill the captcha and click on continue. 

    ![Azure DevOps](images/m1-2.png)

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which include the preconfigured Parts Unlimited team project based on an Azure DevOps Demo Generator template and a Visual Studio configuration.

#### Task 1: Configure the team project

In this task, you will use Azure DevOps Demo Generator to generate a new project based on the **EShopOnWeb** template.

1.  In a new tab of Edge browser navigate to https://azuredevopsdemogenerator.azurewebsites.net. This utility site will automate the process of creating a new Azure DevOps project within your account that is prepopulated with content (work items, repos, etc.) required for the lab. 

    > **Note**: For more information on the site, see https://docs.microsoft.com/en-us/azure/devops/demo-gen.

1.  Click **Sign in** and if prompted sign with the credentials provided in environment details tab.
1.  If required, on the **Azure DevOps Demo Generator** page, click **Accept** to accept the permission requests for accessing your Azure DevOps subscription.
1.  On the **Create New Project** page, in the **New Project Name** textbox, type **EShopOnWeb**, in the **Select organization** dropdown list, select your Azure DevOps organization, and then click **Choose template**.
1.  On the **Choose a template** page, in the list of templates, click the **EShopOnWeb** template, and then click **Select Template**.
1.  Back on the **Create New Project** page, click **Create Project**

    > **Note**: Wait for the process to complete. This should take about 2 minutes. In case the process fails, navigate to your DevOps organization, delete the project, and try again.

1.  On the **Create New Project** page, click **Navigate to project**.
![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/659c7acd-d80d-49a8-b40c-b1d7482492da)


#### Task 2: Configuring the EShopOnWeb solution in Visual Studio

In this task, you will configure Visual Studio to prepare for the lab.

1.  Ensure that you are viewing the **EShopOnWeb** team project on the Azure DevOps portal. 

    > **Note**: You can access the project page directly by navigating to the [https://dev.azure.com/`<your-Azure-DevOps-account-name>`/Package%20Management%20with%20Azure%20Artifacts](https://dev.azure.com/`<your-Azure-DevOps-account-name>`/Package%20Management%20with%20Azure%20Artifacts) URL, where the `<your-Azure-DevOps-account-name>` placeholder, represents your account name. 

2.  In the vertical menu on the left side of the **EShopOnWeb** pane, click **Repos**.
3.  On the **Files** pane, click **Clone**, select the drop-down arrow next to **Clone in VS Code**, and, in the dropdown menu, select **Visual Studio**.

![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b718c44f-ddbd-4bd6-bf8a-2148407fa423)

4.  If prompted whether to proceed, click **Open**. 
5.  If prompted, sign in with the user account you used to set up your Azure DevOps organization. Username and password can be obtained from the environmental details page.
6.  Within the Visual Studio interface, in the **Azure DevOps** pop-up window, accept the default local path and click **Clone**. This will automatically import the project into Visual Studio. Make a note of local path you will need it in further tasks.

![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/60510641-16db-4a03-8ff4-4b5f0c3bf8f7)

7.  After import is completed in lab VM go to start menu search for **Visual Studio Installer** and open it. Click on **Modify** within the **Visual Studio Community 2022** card and under workloads verify if **ASP<nolink>.NET and web development**, **Azure development**, and **.NET Multi-platform App UI development** already selected else select them and click on modify.
8.  Installation will take some time meanwhile minimise visualstudio and continue next task. After installation is completed it will restart Visual studio automatically.

    > **Note**: If you get before we get started window click on continue.

### Exercise 1: Working with Azure Artifacts

In this exercise, you will learn how to work with Azure Artifacts by using the following steps:

- create and connect to a feed.
- create and publish a NuGet package.
- import a NuGet package.
- update a NuGet package.

#### Task 3: Creating and connecting to a feed

In this task, you will create and connect to a feed.

1.  In the web browser window displaying your project settings in the Azure DevOps portal, in the vertical navigational pane, select **Artifacts**.
2.  With the **Artifacts** hub displayed, click **+ Create feed** at the top of the pane. 

    > **Note**: This feed will be a collection of NuGet packages available to users within the organization and will sit alongside the public NuGet feed as a peer. The scenario in this lab will focus on the workflow for using Azure Artifacts, so the actual architectural and development decisions are purely illustrative.  This feed will include common functionality that can be shared across projects in this organization. 

3.  On the **Create new feed** pane, in the **Name** textbox, type **EShopOnWebShared**, in the **Scope** section, select the **Organization** option, leave other settings with their default values, and click **Create**. 

    > **Note**: Any user who wants to connect to this NuGet feed must configure their environment. 

4.  Back on the **Artifacts** hub, click **Connect to feed**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/be592c08-abb6-41ed-a5f2-dbbc1ba55c8b)
    
5.  On the **Connect to feed** pane, in the **NuGet** section, select **Visual Studio** and, on the **Visual Studio** pane, copy the **Source** url.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/765d3650-f942-498f-bf03-ff758e83d1d2)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/995b6126-d007-4d80-9d8a-936856bcbfb9)

6.  Switch back to the **Visual Studio** window and wait for the installation to be get completed. 
1.  In the Visual Studio window, click **Tools** menu header, in the dropdown menu, select **NuGet Package Manager** and, in the cascading menu, select **Package Manager Settings**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/db7e98b5-cef7-429f-8fee-fc77c1632ec7)
    
7.  In the **Options** dialog box, click **Package Sources** and click the plus sign to add a new package source.
8.  At the bottom of the dialog box, in the **Name** textbox, replace **Package Source** with **EShopOnWebShared** and, in the **Source** textbox, paste the URL you copied in the Azure DevOps portal.    
9.  Click **Update** and then click **OK** to finalize the addition.
    
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d2d1b0d3-3ff2-4a7b-99cd-d58838c1b292)

    > **Note**: Visual Studio is now connected to the new feed.

10.  Close and reopen the other Visual Studio instance you used for cloning the PartsUnlimited repository, to account for the artifact source update and open the **EShopOnWebShared** solution. You will need it in the third task of this exercise.

#### Task 2: Creating and publishing a NuGet package

In this task, you will create and publish a NuGet package.

1.  In the Visual Studio window you used to configure the new package source, in the main menu, click **File**, in the dropdown menu, click **New** and then, in the cascading menu, click **Project**. 

    > **Note**: We will now create a shared assembly that will be published as a NuGet package so that other teams can integrate it and stay up to date without having to work directly with the project source.

2.  On the **Recent project templates** page of the **Create a new project** pane, use the search textbox to locate the **Class Library** template, and select the template for C#, and click **Next**. 
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/cb805d0b-1864-4b13-840f-6d8e161477eb)
    
3.  On the **Class Library** page of the **Create a new project** pane, specify the following settings and click **Create**:

    | Setting | Value |
    | --- | --- |
    | Project name | **EShopOnWeb.Shared** |
    | Location | accept the default value |
    | Solution | **Create new solution** |
    | Solution name | **EShopOnWeb.Shared** |
    
    > **Note**: Make sure not to select **.NET Standard**.
4.  Click Next. Accept **.NET 6.0 (Long Term Support)** as Framework option.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/84835659-bf28-4283-b948-362195ea8952)
    
5.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click **Class1.cs**, in the right-click menu, select **Delete**, and, when prompted for confirmation, click **OK**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/5d8b28f1-066a-42f6-a8b8-ac1cfbd1f22f)
    
6.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared** project node and select **Properties**.
7.  Press **Ctrl+Shift+B** or **Right-click** on the **EShopOnWeb.Shared Project** and select **Build** to build the project.. 

    > **Note**: In the next task we'll use **NuGet.exe** to generate a NuGet package directly from the built project, but it requires the project to be built first.

![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/8eaf23fb-a4af-432b-8527-82c7d17c7b75)
   
8.  Switch to the web browser displaying the Azure DevOps portal. 
9.  Navigate to the **Connect to feed** pane, in the **NuGet** section and select **NuGet.exe**. This will display the **NuGet.exe** pane.
10.  On the **NuGet.exe** pane, click **Get the tools**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/79f4f7a1-d61d-474a-9733-38afa86637f8)
    
11.  On the **Get the tools** pane, click the **Download the latest NuGet** link. This will automatically open another browser tab displaying the **Available NuGet Distribution Versions** page.
12.  On the **Available NuGet Distribution Versions** page, select nuget.exe version **v5.5.1** and download the executable to the local **Downloads** folder.
13.  Switch to the **Visual Studio** window. In the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared** project folder and, in the right-click menu, select **Open Folder in File Explorer**.
14.  Within the File Explorer window, move the downloaded **nuget.exe** file from the **Downloads** folder into the folder containing the **EShopOnWeb.Shared** file.
15.  In the same File Explorer window, select the **File** menu header, in the dropdown menu, select **Open Windows PowerShell**, and, in the cascading menu, click **Open Windows PowerShell as administrator**. 
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/185ab649-2e63-45b3-a9c0-f4d977eb8be0)
    
16.  In the **Administrator: Windows PowerShell** window, run the following to create a **.nupkg** file from the project. 

    > **Note**: This is a shortcut to package the NuGet bits for deployment. NuGet is highly customizable. To learn more, refer to the [NuGet package creation page](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflowhttps:/docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow).

    ```
    ./nuget.exe pack ./EShopOnWeb.Shared.csproj
    ```

    > **Note**: Disregard any warnings displayed in the **Administrator: Windows PowerShell** window.

    > **Note**: NuGet builds a minimal package based on the information it is able to identify from the project. For example, note that the name is **ESopOnWeb.Shared.1.0.0.nupkg**. That version number was retrieved from the assembly.
    
17. After the successful creation of the package, run the following to publish the package to the **EShopOnWebShared** feed:

    > **Note**: You need to provide an **API Key**, which can be any non-empty string. We're using **AzDO** here. When prompted, sign in to your Azure DevOps organization.

    ```
    ./nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO EShopOnWeb.Shared.1.0.0.nupkg
    ```
![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/73084231-08d5-4e2f-9141-31a7a138da18)
    
18.  Wait for the confirmation of the successful package push operation.
19.  Switch to the web browser window displaying the Azure DevOps portal and, in the vertical navigational pane, select **Artifacts**.
20.  On the **Artifacts** hub pane, click the dropdown list in the upper left corner and, in the list of feeds, select the **EShopOnWeb** entry.

    > **Note**: The **EShopOnWebShared** feed should include the newly published NuGet package. 

21.  Click the NuGet package to display its details.

#### Task 3: Importing an Open-Source NuGet package to the Azure DevOps Package Feed

Besides developing your own packages, why not using the Open Source Nuget (https://www.nuget.org) DotNet Package library? With a few million packages available, there will always be something useful for your application.
    
In this task, we will use a generic "Hello World" sample package, but you can use the same approach for other packages in the library.

1.  From the same PowerShell window, run the following nuget command to install the sample package:
    
    ```
    .\nuget install HelloWorld -ExcludeVersion
    ```
2. Check the output of the install process. At the first line, it shows the different Feeds it will try to download the package:
    
     ```
    Feeds used:  https://api.nuget.org/v3/index.json
    https://pkgs.dev.azure.com/<AZURE_DEVOPS_ORGANIZATION>/eShopOnWeb/_packaging/EShopOnWebPFeed/nuget/v3/index.json
    ```
3.  Next, it will show additional output regarding the actual installation process itself.

     ```
        Installing package 'Helloworld' to 'C:\eShopOnWeb\EShopOnWeb.Shared'.
        GET https://api.nuget.org/v3/registration5-gz-semver2/helloworld/index.json
        OK https://api.nuget.org/v3/registration5-gz-semver2/helloworld/index.json 114ms
        MSBuild auto-detection: using msbuild version '17.5.0.10706' from 'C:\Program Files\Microsoft Visual Studio\2022\Professional\MSBuild\Current\bin'.
        GET https://pkgs.dev.azure.com/pdtdemoworld/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v3/registrations2-            semver2/helloworld/index.json
        OK https://pkgs.dev.azure.com/pdtdemoworld/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v3/registrations2-             semver2/helloworld/index.json 698ms
        Attempting to gather dependency information for package 'Helloworld.1.3.0.17' with respect to project 'C:\eShopOnWeb\EShopOnWeb.Shared', targeting                  'Any,Version=v0.0'
        Gathering dependency information took 21 ms
        Attempting to resolve dependencies for package 'Helloworld.1.3.0.17' with DependencyBehavior 'Lowest'
        Resolving dependency information took 0 ms
        Resolving actions to install package 'Helloworld.1.3.0.17'
        Resolved actions to install package 'Helloworld.1.3.0.17'
        Retrieving package 'HelloWorld 1.3.0.17' from 'nuget.org'.
        GET https://api.nuget.org/v3-flatcontainer/helloworld/1.3.0.17/helloworld.1.3.0.17.nupkg
        OK https://api.nuget.org/v3-flatcontainer/helloworld/1.3.0.17/helloworld.1.3.0.17.nupkg 133ms
        Installed HelloWorld 1.3.0.17 from https://api.nuget.org/v3/index.json with content hash                                                                             1Pbk5sGihV5JCE5hPLC0DirUypeW8hwSzfhD0x0InqpLRSvTEas7sPCVSylJ/KBzoxbGt2Iapg72WPbEYxLX9g==.
        Adding package 'HelloWorld.1.3.0.17' to folder 'C:\eShopOnWeb\EShopOnWeb.Shared'
        Added package 'HelloWorld.1.3.0.17' to folder 'C:\eShopOnWeb\EShopOnWeb.Shared'
        Successfully installed 'HelloWorld 1.3.0.17' to C:\eShopOnWeb\EShopOnWeb.Shared
        Executing nuget actions took 686 ms
    
    ```
4.  The HelloWorld package got installed in a subfolder **HelloWorld**, under the EShopOnWeb.Shared folder. From the Visual Studio **Solution Explorer**, navigate to the **EShopOnWeb.Shared** Project, and notice the **HelloWorld** subfolder. Click on the little arrow to the left of the subfolder, to open the folder and file list.
    
![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/35e61833-6185-4f1d-81c7-e025b64f1a89)

#### Task 4: Upload the Open-Source NuGet package to Azure Artifacts

Let's consider this package an "approved" package for our DevOps team to reuse, by uploading it to the Azure Artifacts Package feed created earlier.

1.  From the PowerShell window, execute the following command:
     ```
    .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
    ```
     ```
    **Note**:  This results in an error message: Response status code does not indicate success: 409 (Conflict - 'HelloWorld 1.3.0.17' cannot be published to the   feed because it exists in at least one of the feed's upstream sources. Publishing this copy would prevent you from using 'HelloWorld 1.3.0.17' from 'NuGet Gallery'. For more information, see https://go.microsoft.com/fwlink/?linkid=864880 (DevOps Activity ID: AE08BE89-C2FA-4FF7-89B7-90805C88972C)).
    ```
When you created the Azure DevOps Artifacts Package Feed, by design, it allows for **upstream sources**, such as nuget.org in the dotnet example. However, nothing blocks your DevOps team to create an **"internal-only"** Package Feed.
    
1.  Navigate to the Azure DevOps Portal, browse to **Artifacts**, and select the **EShopOnWebShared** Feed.
1.  Click **Search Upstream Sources**
1.  In the **Go to an Upstream Package** window, select **Nuget** as Package Type, and enter **HelloWorld** in the search field.
1.  Confirm by pressing the **Search** button.
1.  This results in a list of all HelloWorld packages with the different versions available.
1.  Click the **left arrow key** to return to the **EShopOnWebShared** Feed.
1.  Click the cogwheel to open **Feed Settings**. Within the Feed Settings page, select **Upstream Sources**.
1.  Notice the different Upstream Package Managers for different development languages. Select **Nuget.org** from the list. Press the **Delete** button, Followed by pressing the **Save** button.    
 
 ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/8619c953-0c82-4d06-9770-44ce97a4963a)
    
1.  With these saved changed, it will be possible to upload the **HelloWorld** package using the Nuget.exe from the PowerShell Window, by relaunching the following command:
     
    ```
    .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
    ```
    **Note**:  This should now result in a successful upload
    
    ```
    Pushing HelloWorld.nupkg to 'https://pkgs.dev.azure.com/pdtdemoworld/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-               293fbf759f06/nuget/v2/'...
    PUT https://pkgs.dev.azure.com/<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/
    MSBuild auto-detection: using msbuild version '17.5.0.10706' from 'C:\Program Files\Microsoft Visual Studio\2022\Professional\MSBuild\Current\bin'.
    Accepted https://pkgs.dev.azure.com/pdtdemoworld<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/ 1645ms
    Your package was pushed. 
    PS C:\eShopOnWeb\EShopOnWeb.Shared> 
    ```
1.  From the Azure DevOps Portal, **refresh** the Artifacts Package Feed page. The list of packages shows both the **EShopOnWeb.Shared** custom-developed package,    as well as the **HelloWorld** public sourced package.
1.  From the Visual Studio **EShopOnWeb.Shared** Solution, right-click the **EShopOnWeb.Shared** Project, and select **Manage Nuget Packages** from the context menu.
1. From the Nuget Package Manager window, validate the **Package Source** is set to **EShopOnWebShared**.
1. Click **Browse**, and wait for the list of Nuget Packages to load.
1. This list will also show both the **EShopOnWeb.Shared** custom-developed package, as well as the **HelloWorld** public sourced package.

#### Review

In this lab, you learned how to work with Azure Artifacts by using the following steps:

- created and connect to a feed.
- created and publish a NuGet package.
- imported a NuGet package.
- updated a NuGet package.

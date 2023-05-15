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

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7ff3a8ba-6703-4624-87a7-e5f2817b19b3)

4. On the **Almost Done...** page fill the captcha and click on continue. 

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/c292f452-5b04-4e88-9c86-dec87f398141)

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which include the preconfigured EShopOnWeb team project based on an Azure DevOps Organization and a Visual Studio configuration.

#### Task 1: Configure the EShopOnWeb project

In this task, you will create a new project named **EShopOnWeb** in Azure DevOps Organization.

1.  Click **Sign in** and sign in with the credentials provided in environment details tab.
    
    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>

2.  On the **Create New Project** page, in the **New Project Name** textbox, type **EShopOnWeb(1)**, select visibilty as **Private(2)** and then click **Create Project(3)**

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ff673278-d825-45a2-bf15-96661f4fc36b)

#### Task 2: Configuring the EShopOnWeb solution in Visual Studio

In this task, you will configure Visual Studio to prepare for the lab.

1.  Ensure that you are viewing the **EShopOnWeb** team project on the Azure DevOps portal. 

    > **Note**: You can access the project page directly by navigating to the [https://dev.azure.com/<inject key="DeploymentID" enableCopy="false"/>/EShopOnWeb]URL.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/20b2de76-feaa-4f7a-9625-69a79fffe73e)

2.  In the vertical menu on the left side of the **EShopOnWeb** pane, click **Repos**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/93d76be3-eb78-402e-9823-6427b86609d5)

3.  When we click on the **Files(2)** pane, we can see that **EShopOnWeb is empty. Add some code!**. So click **Initialize(3)** to initialize the repository for the first time.

    <img width="615" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7472e4bb-247b-404d-ab3d-1282d7c0fc16">

4. In the **EShopOnWeb** repo Click on **Clone**.

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/c062b731-a76a-47ca-a2c9-a38b233a82f1)

5. Select the drop-down arrow next to **Clone in VS Code(1)**, and, in the dropdown menu, select **Visual Studio(2)**.

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b338d3e2-e172-4cbb-b950-7ded6017a68b)

6.  If prompted whether to proceed, click **Open Microsoft Visual Studio Web Protocol Handler Selector**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ced2d1ae-4c9d-46ee-9a9b-1270e8970d34)

7.  If prompted, sign in with the user account you used to set up your Azure DevOps organization. Username and password can be obtained from the environmental details page.

    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/bcb16d9c-00c0-4e4f-aa74-0681fdca736d)

8.  Within the Visual Studio interface, in the **Azure DevOps** pop-up window, accept the default local path and click **Clone**. This will automatically import the project into Visual Studio. Make a note of local path you will need it in further tasks.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/314e67a7-730f-4851-933c-4b31be1f2d08)

### Exercise 1: Working with Azure Artifacts

In this exercise, you will learn how to work with Azure Artifacts by using the following steps:

- create and connect to a feed.
- create and publish a NuGet package.
- import a NuGet package.
- update a NuGet package.

#### Task 3: Creating and connecting to a feed

In this task, you will create and connect to a feed.

1.  In the web browser window displaying your project settings in the Azure DevOps portal, in the vertical navigational pane, select **Artifacts**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b3494450-2aff-42d4-b6bb-b4a87fc14188)

2.  With the **Artifacts** hub displayed, click **+ Create feed** at the top of the pane. 

    > **Note**: This feed will be a collection of NuGet packages available to users within the organization and will sit alongside the public NuGet feed as a peer. The scenario in this lab will focus on the workflow for using Azure Artifacts, so the actual architectural and development decisions are purely illustrative.  This feed will include common functionality that can be shared across projects in this organization. 
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/fb0f3a21-3709-4570-b8bb-8bad139835d6)

3.  On the **Create new feed** pane, in the **Name** textbox, type **EShopOnWebShared(1)**, in the **Scope** section, select the **Organization(2)** option, leave other settings with their default values, and click **Create(3)**. 

    > **Note**: Any user who wants to connect to this NuGet feed must configure their environment. 
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/5deb9ab9-bcb9-49e4-a529-4b9257873bf6)
    
4.  Back on the **Artifacts** hub, click **Connect to feed**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/5f195095-85ee-41d5-9a55-36bbb1f29826)

5.  On the **Connect to feed** pane, in the **NuGet** section, select **Visual Studio(1)** and, on the **Visual Studio** pane, copy the **Source(2)** url.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/acd0b0b0-822b-452e-a031-ef1cc2265938)

6.  Switch back to the **Visual Studio** window and wait for the installation to be get completed. 
7.  In the Visual Studio window, click **Tools(1)** menu header, in the dropdown menu, select **NuGet Package Manager(2)** and, in the cascading menu, select **Package Manager Settings(3)**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0e4843f8-ce22-4058-9c5e-67e5c0138998)
    
8.  In the **Options** dialog box, click **Package Sources** and click the plus sign to add a new package source.
9.  At the bottom of the dialog box, in the **Name** textbox, replace **Package Sources(1)** with **EShopOnWebShared(2)** and, in the **Source** textbox, paste the **Source URL(3)** you copied in the Azure DevOps portal. 
    
    <img width="529" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/065aca07-0b41-4be4-935d-765c044f7fdc">
    
10.  Click **Update(4)** and then click **OK(5)** to finalize the addition.    
     > **Note**: Visual Studio is now connected to the new feed.

11.  Close and reopen the other Visual Studio instance you used for cloning the EShopOnWeb repository, to account for the artifact source update and open the **EShopOnWebShared** solution. You will need it in the third task of this exercise.

#### Task 2: Creating and publishing a NuGet package

In this task, you will create and publish a NuGet package.

1.  In the Visual Studio window you used to configure the new package source, in the main menu, click **File(1)**, in the dropdown menu, click **New(2)** and then, in the cascading menu, click **Project(3)**. 

    > **Note**: We will now create a shared assembly that will be published as a NuGet package so that other teams can integrate it and stay up to date without having to work directly with the project source.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/036ec295-cbcb-4c1b-8191-ba4fc7d92ef1)

2.  On the **Recent project templates** page of the **Create a new project** pane, use the search textbox to locate the **Class Library(1)** template, and select the template for **C#(2)**, and click **Next(3)**. 
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/46b7cb16-85dc-440f-a60d-8f9d440bc485)
    
3.  On the **Class Library** page of the **Create a new project** pane, specify the following settings and click **Next(5)**:

    | Setting | Value |
    | --- | --- |
    | Project name | **EShopOnWeb.Shared(1)** |
    | Location | accept the default value(2) |
    | Solution | **Create new solution(3)** |
    | Solution name | **EShopOnWeb.Shared(4)** |
    
    > **Note**: Make sure not to select **.NET Standard**.

    <img width="760" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/001443b5-e541-4601-800e-cfe4cba8e66a">
    
4.  Click Next. Accept **.NET 6.0 (Long Term Support)(1)** as Framework option and click **Create(2)**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/33215b51-4b9e-4eec-9e50-3c61d0827a34)

5.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click **Class1.cs(1)**, in the right-click menu, select **Delete(2)**, and, when prompted for confirmation, click **OK**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/41cfbb6a-3256-4c86-920b-eb590404083a)
   
6.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared** project node and select **Properties**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0f8df7e2-2a1e-4f69-a501-2815190d795f)

7.  Press **Ctrl+Shift+B** or **Right-click** on the **EShopOnWeb.Shared Project(1)** and select **Build(2)** to build the project.. 

    > **Note**: In the next task we'll use **NuGet.exe** to generate a NuGet package directly from the built project, but it requires the project to be built first.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/943dcffb-e6fc-4671-90e3-a4bcd898520a)
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/61315a4f-27d7-4416-87e1-ab24387dfcae)
   
8.  Switch to the web browser displaying the Azure DevOps portal. 
9.  Navigate to the **Connect to feed** pane, in the **NuGet** section and select **NuGet.exe**. This will display the **NuGet.exe** pane.
10.  On the **NuGet.exe(2)** pane, click **Get the tools(3)**.
    
     <img width="933" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a4724af0-2d15-4e3e-81d5-c2e5f6f8f2dd">
    
11.  On the **Get the tools** pane, click the **Download the latest NuGet** link. This will automatically open another browser tab displaying the **Available NuGet Distribution Versions** page.   

12.  On the **Available NuGet Distribution Versions** page, select nuget.exe version **v5.5.1** and download the executable to the local **Downloads** folder.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/c3ab865b-cae0-484a-92ff-0bd5064b0d0b)


13.  Switch to the **Visual Studio** window. In the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared(1)** project folder and, in the right-click menu, select **Open Folder in File Explorer(2)**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/6552d651-d479-488d-beee-f38f5f05d93f)

14.  Within the File Explorer window, move the downloaded **nuget.exe** file from the **Downloads** folder into the folder containing the **EShopOnWeb.Shared** file.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f68637a1-aa3d-4320-b37b-80210bbd5953)

15.  In the same File Explorer window, select the **File(1)** menu header, in the dropdown menu, select **Open Windows PowerShell(2)**, and, in the cascading menu, click **Open Windows PowerShell as administrator(3)**. 
    
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/74335420-e989-4524-91b0-ab24b464b112)
    
16.  In the **Administrator: Windows PowerShell** window, run the following to create a **.nupkg** file from the project. 

     > **Note**: This is a shortcut to package the NuGet bits for deployment. NuGet is highly customizable. To learn more, refer to the [NuGet package creation page](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflowhttps:/docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow).

    ```
    ./nuget.exe pack ./EShopOnWeb.Shared.csproj
    ```

   > **Note**: Disregard any warnings displayed in the **Administrator: Windows PowerShell** window.

   > **Note**: NuGet builds a minimal package based on the information it is able to identify from the project. For example, note that the name is **ESopOnWeb.Shared.1.0.0.nupkg**. That version number was retrieved from the assembly.
    
    <img width="786" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/37241967-c457-42cd-9a03-db5861d2c2af">

    
17. If you prompted with the **Error NU5133: NuGet.exe file on path C:\Users\PrathimaV\source\repos\EShopOnWeb.Shared\EShopOnWeb.Shared\nuget.exe needs to be unblocked after downloading** the we need to unblock the **nuget.exe(1)** file which we downloaded to the **EShareOnWeb.Shared** folder by selecting **Properties(2)**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/c25eb7c4-2812-45a8-a099-5f07bfff8906)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/90144ffc-dad9-4611-bf02-b6904ff0e5c5)
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/450c2906-e427-4ada-88dc-431ad61d1bba)

18. After the successful creation of the package, run the following to publish the package to the **EShopOnWebShared** feed. If it Prompted to signin window login with the credentials provided in the environment details tab.

    > **Note**: You need to provide an **API Key**, which can be any non-empty string. We're using **AzDO** here. When prompted, sign in to your Azure DevOps organization.

    ```
    ./nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO EShopOnWeb.Shared.1.0.0.nupkg
    ```
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0b64b742-087b-4e50-82b2-d86a47ec1e8c)
    
19.  Wait for the confirmation of the successful package push operation.      
20.  Switch to the web browser window displaying the Azure DevOps portal and, in the vertical navigational pane, select **Artifacts**.
21.  On the **Artifacts(1)** hub pane, click the dropdown list in the upper left corner and, in the list of feeds, select the **EShopOnWeb(2)** entry.

     > **Note**: The **EShopOnWebShared** feed should include the newly published NuGet package. 

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/b81fc038-925e-4894-9cb0-684c875f545b)
    
22.  Click the NuGet package to display its details.
    
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d51a0fb7-4c5f-44a3-b743-4d77b939291f)


#### Task 3: Importing an Open-Source NuGet package to the Azure DevOps Package Feed

Besides developing your own packages, why not using the Open Source Nuget (https://www.nuget.org) DotNet Package library? With a few million packages available, there will always be something useful for your application.
    
In this task, we will use a generic "Hello World" sample package, but you can use the same approach for other packages in the library.

1.  From the same PowerShell window, run the following nuget command to install the sample package:
    
    ```
    .\nuget install HelloWorld -ExcludeVersion
    ```
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/786352a3-4c4b-4845-9b1e-d1256fc006af)

2.  The HelloWorld package got installed in a subfolder **HelloWorld**, under the EShopOnWeb.Shared folder.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/38a9abc6-013b-4168-b6ad-4deb9aa9b7d6)

3.  From the Visual Studio **Solution Explorer**, navigate to the **EShopOnWeb.Shared** Project, and notice the **HelloWorld** subfolder. Click on the little arrow to the left of the subfolder, to open the folder and file list.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/2f715660-dd93-4f14-ae28-d784b2f50392)

#### Task 4: Upload the Open-Source NuGet package to Azure Artifacts

Let's consider this package an "approved" package for our DevOps team to reuse, by uploading it to the Azure Artifacts Package feed created earlier.

1.  From the PowerShell window, execute the following command:
     ```
    .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
    ```
     ```
   > **Note**:  This results in an error message: Response status code does not indicate success: 409 (Conflict - 'HelloWorld 1.3.0.17' cannot be published to the   feed because it exists in at least one of the feed's upstream sources. Publishing this copy would prevent you from using 'HelloWorld 1.3.0.17' from 'NuGet Gallery'. For more information, see https://go.microsoft.com/fwlink/?linkid=864880 (DevOps Activity ID: AE08BE89-C2FA-4FF7-89B7-90805C88972C)).
    ```
    <img width="946" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/5ccd5a9a-ebf4-4e2e-80bf-68a806e21282">

When you created the Azure DevOps Artifacts Package Feed, by design, it allows for **upstream sources**, such as nuget.org in the dotnet example. However, nothing blocks your DevOps team to create an **"internal-only"** Package Feed.
    
2.  Navigate to the Azure DevOps Portal, browse to **Artifacts**, and select the **EShopOnWebShared** Feed.
3.  Click **Search Upstream Sources(2)**

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a8f03dd9-8b59-41d6-b7d6-39eb1713b6ac)

4.  In the **Go to an Upstream Package** window, select **Nuget(1)** as Package Type, and enter **HelloWorld(2)** in the search field.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/9f45bd54-3d8a-49bc-a855-2dc4488cb3fd)
    
5.  Confirm by pressing the **Search(3)** button.
6.  This results in a list of all HelloWorld packages with the different versions available.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/359b2dc5-f689-4eb1-b83f-222c9b612fb1)

7.  Click the **left arrow key** to return to the **EShopOnWebShared** Feed.
8.  Click the cogwheel to open **Feed Settings(1)**. Within the Feed Settings page, select **Upstream Sources(2)**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/e8bf6f9c-2966-42c1-93ae-a297e953adc8)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ab6d6836-d109-4b96-8796-d02497a903be)


9.  Notice the different Upstream Package Managers for different development languages. Select **Nuget.org(3)** from the list. Press the **Delete(5)** button, Followed by pressing the **Save(6)** button.    
 
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/702e0286-b509-4891-b988-8b004d94278e)

    
10.  With these saved changed, it will be possible to upload the **HelloWorld** package using the Nuget.exe from the PowerShell Window, by relaunching the following command:
     
    ```
    .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
    ```
   > **Note**:  This should now result in a successful upload
    
    ```
    Pushing HelloWorld.nupkg to 'https://pkgs.dev.azure.com/pdtdemoworld/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-               293fbf759f06/nuget/v2/'...
    PUT https://pkgs.dev.azure.com/<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/
    MSBuild auto-detection: using msbuild version '17.5.0.10706' from 'C:\Program Files\Microsoft Visual Studio\2022\Professional\MSBuild\Current\bin'.
    Accepted https://pkgs.dev.azure.com/pdtdemoworld<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/ 1645ms
    Your package was pushed. 
    PS C:\eShopOnWeb\EShopOnWeb.Shared> 
    ```
11.  From the Azure DevOps Portal, **refresh** the Artifacts Package Feed page. The list of packages shows both the **EShopOnWeb.Shared** custom-developed package,    as well as the **HelloWorld** public sourced package.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f4ebf2b7-5b84-4899-bb05-156518542d0a)


12.  From the Visual Studio **EShopOnWeb.Shared** Solution, right-click the **EShopOnWeb.Shared(1)** Project, and select **Manage Nuget Packages(2)** from the context menu.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/75815c41-d64a-4cca-8c8b-25a207b83847)

13. From the Nuget Package Manager window, validate the **Package Source** is set to **EShopOnWebShared**.
14. Click **Browse(1)**, and wait for the list of Nuget Packages to load.
15. This list will also show both the **EShopOnWeb.Shared(2)** custom-developed package, as well as the **HelloWorld(2)** public sourced package.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a30d6701-4400-4579-953f-8542c744ec8c)


#### Review

In this lab, you learned how to work with Azure Artifacts by using the following steps:

- created and connect to a feed.
- created and publish a NuGet package.
- imported a NuGet package.
- updated a NuGet package.

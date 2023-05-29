# Lab 15: Package Management with Azure Artifacts


## Lab overview

Azure Artifacts facilitate discovery, installation, and publishing NuGet, npm, and Maven packages in Azure DevOps. It's deeply integrated with other Azure DevOps features such as Build, making package management a seamless part of your existing workflows.

## Objectives

After you complete this lab, you will be able to:

-  Create and connect to a feed.
-  Create and publish a NuGet package.
-  Import a NuGet package.
-  Update a NuGet package.


## Architecture Diagram

  ![Architecture Diagram](images/lab15-architecture.png)

## Set up an Azure DevOps organization
1. On your lab VM open **Edge Browser** on desktop and navigate to [Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137), and if prompted sign with the credentials.

    * Email/Username: <inject key="AzureAdUserEmail"></inject>

    * Password: <inject key="AzureAdUserPassword"></inject>

2. In the pop-up for *Help us protect your account*, select **Skip for now (14 days until this is required)**.

3. On the next page accept defaults and click on continue.

   ![Azure DevOps](images/lab1-image1.png)
    
4. On the **Almost Done...** page fill the captcha and click on continue. 

    ![Azure DevOps](images/lab1-image2.png)

# Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which include the preconfigured EShopOnWeb team project based on an Azure DevOps Organization and a Visual Studio configuration.

#### Task 1: Configure the EShopOnWeb project

In this task, you will create a new project named **EShopOnWeb** in Azure DevOps Organization.

1.  Click **Sign in** and sign in with the credentials provided in environment details tab.
    
    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>

2.  On the **Create New Project** page, in the **New Project Name** textbox, type **EShopOnWeb(1)**, select visibilty as **Private(2)** and then click **+ Create Project(3)**

    ![](images/AZ400_M08_L15_03.png)

#### Task 2: Configuring the EShopOnWeb solution in Visual Studio

In this task, you will configure Visual Studio to prepare for the lab.

1.  Ensure that you are viewing the **EShopOnWeb** team project on the Azure DevOps portal. 

    > **Note**: You can access the project page directly by navigating to the [https://dev.azure.com/<inject key="DeploymentID" enableCopy="false"/>/EShopOnWeb]URL.

    ![](images/AZ400_M08_L15_04.png)

2.  In the vertical menu on the left side of the **EShopOnWeb** pane, click **Repos**.

    ![](images/AZ400_M08_L15_05.png)

3.  When we click on the **Files(2)** pane, we can see that **EShopOnWeb is empty. Add some code!**. So click **Initialize(3)** to initialize the repository for the first time.

    <img width="615" alt="image" src="https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7472e4bb-247b-404d-ab3d-1282d7c0fc16">

4. In the **EShopOnWeb** repo Click on **Clone**.

    ![](images/AZ400_M08_L15_07.png)

5. Select the drop-down arrow next to **Clone in VS Code(1)**, and, in the dropdown menu, select **Visual Studio(2)**.

    ![](images/AZ400_M08_L15_08.png)

6.  If prompted whether to proceed, click on **Open**.

    ![](images/AZ400_M08_L15_(09).png)

7.  If prompted, sign in with the user account you used to set up your Azure DevOps organization. Enter following Username and password.

    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>
    
    ![](images/AZ400_M08_L15_10.png)
    
8.  On **Personalize your Visul Studio experience** page click **Start Visual Studio**.

     ![](images/az400-vs.png)
     
9.  Within the Visual Studio interface, in the **Azure DevOps** pop-up window, accept the default local path and click **Clone**. This will automatically import the project into Visual Studio. Make a note of local path you will need it in further tasks.

    ![](images/AZ400_M08_L15_11.png)

# Exercise 1: Working with Azure Artifacts

In this exercise, you will learn how to work with Azure Artifacts by using the following steps:

- create and connect to a feed.
- create and publish a NuGet package.
- import a NuGet package.
- update a NuGet package.

#### Task 1: Create and connect to a feed

In this task, you will create and connect to a feed.

1.  In the web browser window displaying your project settings in the Azure DevOps portal, in the vertical navigational pane, select **Artifacts**.
    
    ![](images/AZ400_M08_L15_12.png)

2.  With the **Artifacts** hub displayed, selct organization **odluser-<inject key="DeploymentID" enableCopy="false"/>** and click **+ Create feed** at the top of the pane. 

    > **Note**: This feed will be a collection of NuGet packages available to users within the organization and will sit alongside the public NuGet feed as a peer. The scenario in this lab will focus on the workflow for using Azure Artifacts, so the actual architectural and development decisions are purely illustrative.  This feed will include common functionality that can be shared across projects in this organization. 
    
    ![](images/AZ400_M08_L15_(13).png)

3.  On the **Create new feed** pane, in the **Name** textbox, type **EShopOnWebShared(1)**, in the **Scope** section, select the **Organization(2)** option, leave other settings with their default values, and click **Create(3)**. 

    > **Note**: Any user who wants to connect to this NuGet feed must configure their environment. 
    
    ![](images/AZ400_M08_L15_14.png)
    
4.  Back on the **Artifacts** hub, select **odluser-<inject key="DeploymentID" enableCopy="false"/>** Organization, click **Connect to feed**.
    
    !![](images/AZ400_M08_L15_(15).png)

5.  On the **Connect to feed** pane, in the **NuGet** section, select **Visual Studio(1)** and, on the **Visual Studio** pane, copy the **Source(2)** url.
    
    ![](images/AZ400_M08_L15_16.png)

6.  Switch back to the **Visual Studio** window and wait for the installation to be get completed. 
7.  In the Visual Studio window, click **Tools(1)** menu header, in the dropdown menu, select **NuGet Package Manager(2)** and, in the cascading menu, select **Package Manager Settings(3)**.
    
    ![](images/AZ400_M08_L15_17.png)
    
8.  In the **Options** dialog box, click **Package Sources** and click the plus sign to add a new package source.
9.  At the bottom of the dialog box, in the **Name** textbox, replace **Package Sources(1)** with **EShopOnWebShared(2)** and, in the **Source** textbox, paste the **Source URL(3)** you copied in the Azure DevOps portal. 
    
    ![](images/AZ400_M08_L15_18.png)
    
10.  Click **Update(4)** and then click **OK(5)** to finalize the addition.    
     > **Note**: Visual Studio is now connected to the new feed.

11.  Close and reopen the other Visual Studio instance you used for cloning the EShopOnWeb repository, to account for the artifact source update and open the **EShopOnWebShared** solution. You will need it in the third task of this exercise.


#### Task 2: Create and publish a NuGet package

In this task, you will create and publish a NuGet package.

1.  In the Visual Studio window you used to configure the new package source, in the main menu, click **File(1)**, in the dropdown menu, click **New(2)** and then, in the cascading menu, click **Project(3)**. 

    > **Note**: We will now create a shared assembly that will be published as a NuGet package so that other teams can integrate it and stay up to date without having to work directly with the project source.
    
    ![](images/AZ400_M08_L15_19.png)

2.  On the **Recent project templates** page of the **Create a new project** pane, use the search textbox to locate the **Class Library(1)** template, and select the template for **C#(2)**, and click **Next(3)**. 
    
    ![](images/AZ400_M08_L15_20.png)

3.  On the **Class Library** page of the **Create a new project** pane, specify the following settings and click **Next(5)**:

    | Setting | Value |
    | --- | --- |
    | Project name | **EShopOnWeb.Shared(1)** |
    | Location | accept the default value(2) |
    | Solution | **Create new solution(3)** |
    | Solution name | **EShopOnWeb.Shared(4)** |
    
    > **Note**: Make sure not to select **.NET Standard**.

    ![](images/AZ400_M08_L15_21.png)
    
4.  Click Next. Accept **.NET 6.0 (Long Term Support)(1)** as Framework option and click **Create(2)**.
    
    ![](images/AZ400_M08_L15_22.png)

5.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click **Class1.cs(1)**, in the right-click menu, select **Delete(2)**, and, when prompted for confirmation, click **OK**.
    
    ![](images/AZ400_M08_L15_23.png)
   
6.  Within the Visual Studio interface, in the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared(1)** project node and select **Properties(2)**.
    
    ![](images/AZ400_M08_L15_24.png)

7.  Press **Ctrl+Shift+B** or **Right-click** on the **EShopOnWeb.Shared Project(1)** and select **Build(2)** to build the project.. 

    > **Note**: In the next task we'll use **NuGet.exe** to generate a NuGet package directly from the built project, but it requires the project to be built first.

    ![](images/AZ400_M08_L15_25.png)
    
    ![](images/AZ400_M08_L15_26.png)
   
8.  Switch to the web browser displaying the Azure DevOps portal. 
9.  Navigate to the **Connect to feed** pane, in the **NuGet** section and select **NuGet.exe**. This will display the **NuGet.exe** pane.
10.  On the **NuGet.exe(2)** pane, click **Get the tools(3)**.
    ![](images/AZ400_M08_L15_27.png)
    
11.  On the **Get the tools** pane, click the **Download the latest NuGet** link. This will automatically open another browser tab displaying the **Available NuGet Distribution Versions** page.   

12.  On the **Available NuGet Distribution Versions** page, select nuget.exe version **v5.5.1** and download the executable to the local **Downloads** folder.
    ![](images/AZ400_M08_L15_28.png)


13.  Switch to the **Visual Studio** window. In the **Solution Explorer** pane, right-click the **EShopOnWeb.Shared(1)** project folder and, in the right-click menu, select **Open Folder in File Explorer(2)**.
    ![](images/AZ400_M08_L15_29.png)

14.  Within the File Explorer window, move the downloaded **nuget.exe** file from the **Downloads** folder into the folder containing the **EShopOnWeb.Shared** file.
    ![](images/AZ400_M08_L15_30.png)

15.  In the same File Explorer window, select the **File(1)** menu header, in the dropdown menu, select **Open Windows PowerShell(2)**, and, in the cascading menu, click **Open Windows PowerShell as administrator(3)**. 
    ![](images/AZ400_M08_L15_31.png)
    
16.  In the **Administrator: Windows PowerShell** window, run the following to create a **.nupkg** file from the project. 

     > **Note**: This is a shortcut to package the NuGet bits for deployment. NuGet is highly customizable. To learn more, refer to the [NuGet package creation page](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflowhttps:/docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow).

17.   Before running powershell command in step no-18, please perform below steps:
      - From the start menu search and select **Edit the system environment variable** and on Systems properties select **Environment variable**.
      - On **Environment variable** page under User variables for azureuser click **New** and on New User Variable, enter **NUGET_ENABLE_LEGACY_CSPROJ_PACK** in **Variable name** field and enter **true** in  **Variable value** field and click on **ok**.
      - Repeat same above step for **Systems variables** click **New** and on New User Variable, enter **NUGET_ENABLE_LEGACY_CSPROJ_PACK** in **Variable name** field and enter **true** in  **Variable value** field and click on **ok** for all wizards.
     
18.  Run following command:

     ```
      cd c:\Users\azureuser\source\repos\EShopOnWeb.Shared\EShopOnWeb.Shared
     ```

     ```
      ./nuget.exe pack ./EShopOnWeb.Shared.csproj
     ```

     > **Note**: Disregard any warnings displayed in the **Administrator: Windows PowerShell** window.

     > **Note**: NuGet builds a minimal package based on the information it is able to identify from the project. For example, note that the name is **ESopOnWeb.Shared.1.0.0.nupkg**. That version number was retrieved from the assembly.
        
        ![](images/AZ400_M08_L15_32.png)
    
17. If you prompted with the **Error NU5133: NuGet.exe file on path C:\Users\xxxxx\source\repos\EShopOnWeb.Shared\EShopOnWeb.Shared\nuget.exe needs to be unblocked after downloading** then we need to unblock the **nuget.exe(1)** file which we downloaded to the **EShareOnWeb.Shared** folder by selecting **Properties(2)**.

    ![](images/AZ400_M08_L15_33.png)

18. Check the **Unblock(1)** and click on **Apply(2)** to save the changes and click on **OK(3)**.

    ![](images/AZ400_M08_L15_34.png)
    
19. Now again run the **Poweshell command** from the **step 16** and it will create package successfully.
    
    ![](images/AZ400_M08_L15_35.png)

20. After the successful creation of the package, run the following to publish the package to the **EShopOnWebShared** feed. If it Prompted to signin window login with the following credentials.
    
    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>

    > **Note**: You need to provide an **API Key**, which can be any non-empty string. We're using **AzDO** here. When prompted, sign in to your Azure DevOps organization.

    ```
    ./nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO EShopOnWeb.Shared.1.0.0.nupkg
    ```
    
    ![](images/AZ400_M08_L15_36.png)
    
21.  Wait for the confirmation of the successful package push operation.      
22.  Switch to the web browser window displaying the Azure DevOps portal and, in the vertical navigational pane, select **Artifacts**.
23.  On the **Artifacts(1)** hub pane, click the dropdown list in the upper left corner and, in the list of feeds, select the **EShopOnWeb(2)** entry.

     > **Note**: The **EShopOnWebShared** feed should include the newly published NuGet package. 
    ![](images/AZ400_M08_L15_37.png)
    
24.  Click the NuGet package to display its details.
    ![](images/AZ400_M08_L15_38.png)

#### Task 3: Import an Open-Source NuGet package to the Azure DevOps Package Feed

Besides developing your own packages, why not using the Open Source Nuget (https://www.nuget.org) DotNet Package library? With a few million packages available, there will always be something useful for your application.
    
In this task, we will use a generic "Hello World" sample package, but you can use the same approach for other packages in the library.

1.  From the same PowerShell window, run the following nuget command to install the sample package:
    
    ```
    .\nuget install HelloWorld -ExcludeVersion
    ```
    ![](images/AZ400_M08_L15_39.png)

2.  The HelloWorld package got installed in a subfolder **HelloWorld**, under the EShopOnWeb.Shared folder.
    
    ![](images/AZ400_M08_L15_40.png)

3.  From the Visual Studio **Solution Explorer**, navigate to the **EShopOnWeb.Shared** Project, and notice the **HelloWorld** subfolder. Click on the little arrow to the left of the subfolder, to open the folder and file list.
    
    ![](images/AZ400_M08_L15_41.png)

#### Task 4: Upload the Open-Source NuGet package to Azure Artifacts

Let's consider this package an "approved" package for our DevOps team to reuse, by uploading it to the Azure Artifacts Package feed created earlier.

1.  From the PowerShell window, execute the following command:

     ```
    .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
    ```
    
    > **Note**:  This results in an error message: Response status code does not indicate success: 409 (Conflict - 'HelloWorld 1.3.0.17' cannot be published to the   feed because it exists in at least one of the feed's upstream sources. Publishing this copy would prevent you from using 'HelloWorld 1.3.0.17' from 'NuGet Gallery'. For more information, see https://go.microsoft.com/fwlink/?linkid=864880 (DevOps Activity ID: AE08BE89-C2FA-4FF7-89B7-90805C88972C)).
    
    ![](images/AZ400_M08_L15_42.png)

    When you created the Azure DevOps Artifacts Package Feed, by design, it allows for **upstream sources**, such as nuget.org in the dotnet example. However, nothing blocks your DevOps team to create an **"internal-only"** Package Feed.
    
2.  Navigate to the Azure DevOps Portal, browse to **Artifacts**, and select the **EShopOnWebShared** Feed.
3.  Click **Search Upstream Sources(2)**

    ![](images/AZ400_M08_L15_43.png)

4.  In the **Go to an Upstream Package** window, select **Nuget(1)** as Package Type, and enter **HelloWorld(2)** in the search field.
    
    ![](images/AZ400_M08_L15_44.png)
    
5.  Confirm by pressing the **Search(3)** button.
6.  This results in a list of all HelloWorld packages with the different versions available.

    ![](images/AZ400_M08_L15_45.png)

7.  Click the **left arrow key** to return to the **EShopOnWebShared** Feed.
8.  Click the cogwheel to open **Feed Settings(1)**. Within the Feed Settings page, select **Upstream Sources(2)**.
    
    ![](images/AZ400_M08_L15_46.png)

    ![](images/AZ400_M08_L15_47.png)


9.  Notice the different Upstream Package Managers for different development languages. Select **Nuget.org(3)** from the list. Press the **Delete(5)** button, Followed by pressing the **Save(6)** button.    
 
    ![](images/AZ400_M08_L15_48.png)

    
10.  With these saved changed, it will be possible to upload the **HelloWorld** package using the Nuget.exe from the PowerShell Window, by relaunching the following command:
     
     ```
     .\nuget.exe push -source "EShopOnWebShared" -ApiKey AzDO c:\Users\azureuser\source\repos\EShopOnWeb\EShopOnWeb.Shared\HelloWorld\HelloWorld.nupkg
     ```
     > **Note**:  This should now result in a successful upload
    
        ```
        Pushing HelloWorld.nupkg to 'https://pkgs.dev.azure.com/pdtdemoworld/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-                               293fbf759f06/nuget/v2/'...
        PUT https://pkgs.dev.azure.com/<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/
        MSBuild auto-detection: using msbuild version '17.5.0.10706' from 'C:\Program Files\Microsoft Visual Studio\2022\Professional\MSBuild\Current\bin'.
        Accepted https://pkgs.dev.azure.com/pdtdemoworld<AZUREDEVOPSORGANIZATION>/7dc3351f-bb0c-42ba-b3c9-43dab8e0dc49/_packaging/188ec0d5-ff93-4eb7-b9d3-293fbf759f06/nuget/v2/ 1645ms
        Your package was pushed. 
        PS C:\eShopOnWeb\EShopOnWeb.Shared> 
        ```
11.  From the Azure DevOps Portal, **refresh** the Artifacts Package Feed page. The list of packages shows both the **EShopOnWeb.Shared** custom-developed package, as well as the **HelloWorld** public sourced package.
    ![](images/AZ400_M08_L15_49.png)


12.  From the Visual Studio **EShopOnWeb.Shared** Solution, right-click the **EShopOnWeb.Shared(1)** Project, and select **Manage Nuget Packages(2)** from the context menu.
    ![](images/AZ400_M08_L15_50.png)

13. From the Nuget Package Manager window, validate the **Package Source** is set to **EShopOnWebShared**.
14. Click **Browse(1)**, and wait for the list of Nuget Packages to load.
15. This list will also show both the **EShopOnWeb.Shared(2)** custom-developed package, as well as the **HelloWorld(2)** public sourced package.
    
    ![](images/AZ400_M08_L15_51.png)


#### Review

In this lab, you learned how to work with Azure Artifacts by using the following steps:

- created and connect to a feed.
- created and publish a NuGet package.
- imported a NuGet package.
- updated a NuGet package.

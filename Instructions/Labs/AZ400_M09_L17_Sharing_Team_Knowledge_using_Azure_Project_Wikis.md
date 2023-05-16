# Lab 17: Sharing Team Knowledge using Azure Project Wikis

# Student lab manual

## Lab overview

In this lab, you will create and configure wiki in an Azure DevOps, including managing markdown content and creating a Mermaid diagram.

## Objectives

After you complete this lab, you will be able to:

- Create a wiki in an Azure Project
- Add and edit markdown
- Create a Mermaid diagram

## Lab duration

-   Estimated time: **45 minutes**

## Instructions

#### Set up an Azure DevOps organization. 

1. On your lab VM open **Edge Browser** on desktop and navigate to https://go.microsoft.com/fwlink/?LinkId=307137. 

2. In the pop-up for *Help us protect your account*, select **Skip for now (14 days until this is required)**.

3. On the next page accept defaults and click on continue.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7ff3a8ba-6703-4624-87a7-e5f2817b19b3)

4. On the **Almost Done...** page fill the captcha and click on continue. 

   ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/17b7f53a-e09c-4fb1-8604-755e334303dc)

### Exercise 0: Configure the lab prerequisites

In this exercise, you will set up the prerequisites for the lab, which consist of the preconfigured **EShopOnWeb** team project based on an Azure DevOps Demo Generator template and a team created in Microsoft Teams.

#### Task 1: Configure the EShopOnWeb project


In this task, you will create a new project named **EShopOnWeb** in Azure DevOps Organization.

1.  Click **Sign in** and sign in with the credentials provided in environment details tab.
    
    > **Email/Username**: <inject key="AzureAdUserEmail"></inject>
    
    > **Password**: <inject key="AzureAdUserPassword"></inject>

2.  On the **Create New Project** page, in the **New Project Name** textbox, type **EShopOnWeb(1)**, select visibilty as **Private(2)** and then click **Create Project(3)**

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/ff673278-d825-45a2-bf15-96661f4fc36b)
    
### Exercise 1: Publish code as wiki

In this exercise, you will step through publishing an Azure DevOps repository as wiki and managing the published wiki.

> **Note**: Content that you maintain in a Git repository can be published to an Azure DevOps wiki. For example, content written to support a software development kit, product documentation, or README files can be published directly to a wiki. You have the option of publishing multiple wikis within the same Azure DevOps team project.

#### Task 1: Publish a branch of an Azure DevOps repo as wiki

In this task, you will create Azure Repository and publish a branch of an Azure DevOps repo as wiki.

> **Note**: If your published wiki corresponds to a product version, you can publish new branches as you release new versions of your product. 

1.  Ensure that you are viewing the **EShopOnWeb** team project on the Azure DevOps portal. 

    > **Note**: You can access the project page directly by navigating to the [https://dev.azure.com/<inject key="DeploymentID" enableCopy="false"/>/EShopOnWeb]URL.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/15897f3d-e2ec-4fe5-b195-f7437bf32953)
    
2.  In the vertical menu on the left side of the **EShopOnWeb** pane, click **Repos**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/7431551f-bebf-4505-8533-4434ee85a9c9)

3.  Click on the **Files(1)** pane, we can see that **EShopOnWeb is empty. Add some code!**. So click **Initialize(2)** to initialize the repository for the first time.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/949b6133-75e2-42bf-a68d-b21d7d3b9511)
    
4.  Click the **Files(1)** pane. In the branch dropdown list , select **main(2)** branch, and review the content of the main branch.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/dcd483fa-3e95-443f-b917-138bb6d73fca)

5.  We will store the Wiki source files in a separate folder within the Repos current folder structure. From within Repos, select **Files**. Notice the **EShopOnWeb(1)** Repo title on top of the folder structure. Select the **elipsis (3 dots)(2)**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/485daed9-9839-4873-937c-38c59a817e86)

6.  Choose **New(1) / Folder(2)**, and provide **Documents(3)** as title for the New Folder name. As a repo doesn't allow you to create an empty folder, provide **READ.ME(4)** as New File name. Click to **Create(5)** folder.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/bff5c51f-564f-447b-9b35-97c64afff0b6)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/63a3eef8-e4d1-46b1-bb4d-bed20641a4ca)

7.  The **READ.ME(1)** file will open in the built-in view mode. Since this is stored **'as code'(2)**, you need to **Commit** the changes by clicking the **Commit(3)** button.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a160d567-e9c0-4678-8386-c2ea332c90a9)

8.  In the Azure DevOps vertical menu on the left side, click **Overview(1)**, in the Overview section, select **Wiki(2)**, select **Publish code as wiki(3)**.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f1c7908a-d510-4b5e-b792-b3fd13f94ed8)

9. On the **Publish code as wiki** pane, specify the following **settings** and click **Publish(5)**.
   | Setting | Value |
    | ------- | ----- |
    | Repository | **EShopOnWeb(1)** |
    | Branch | **main(2)** |
    | Folder | **/Documents(3)** |
    | Wiki name | **EShopOnWeb (Documents)(4)** |

    > **Note**: This will automatically open the Wiki section, and publish the editor, where you can provide a Wiki page title, as well as adding the actual content. Notice you are encouraged to use MarkDown format, but make use of the ribbon to help you with some of the MarkDown layout syntax.
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/00f548a7-fd25-40e3-949c-4dcb12c59fbc)

10.  In the Wiki Page **Title** field, enter **Welcome to our Online Retail Store!**.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/fb653a0f-c238-42c0-b15c-b81a8232951b)

11.  In the **body(1)** of the Wiki Page, paste in the following text and **Save(2)** it:
    
      ```
      ##Welcome to Our Online Retail Store!
      At our online retail store, we offer a **wide range of products** to meet the **needs of our customers**. Our selection includes everything       from *clothing and accessories to electronics, home decor, and more*.

      We pride ourselves on providing a seamless shopping experience for our customers. Our website offers the following benefits:
      1. user-friendly,
      2. and easy to navigate, 
      3. allowing you to find what you're looking for,
      4. quickly and easily. 

      We also offer a range of **_payment and shipping options_** to make your shopping experience as convenient as possible.

      ### about the team
      Our team is dedicated to providing exceptional customer service. If you have any questions or concerns, our knowledgeable and friendly           support team is always available to assist you. We also offer a hassle-free return policy, so if you're not completely satisfied with your       purchase, you can easily return it for a refund or exchange.

      ### Physical Stores
      |Location|Area|Hours|
      |--|--|--|
      | New Orleans | Home and DIY  |07.30am-09.30pm  |
      | Seattle | Gardening | 10.00am-08.30pm  |
      | New York | Furniture Specialists  | 10.00am-09.00pm |

      ## Our Store Qualities
      - We're committed to providing high-quality products
      - Our products are offered at affordable prices 
      - We work with reputable suppliers and manufacturers 
      - We ensure that our products meet our strict standards for quality and durability. 
      - Plus, we regularly offer sales and discounts to help you save even more.

      #Summary
      Thank you for choosing our online retail store for your shopping needs. We look forward to serving you!
      ```
      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/1e0e5a20-ffec-4abc-a38c-30b4c56d4f98)
      
12.  This sample text gives you an overview of several of the common MarkDown syntax features, ranging from Title and subtitles (##), bold            (**), italic (*), how to create tables, and more.
13.  **Refresh** your browser, or select any other DevOps portal option and return to the **Overview(1)** and **Wiki(2)** section. Notice you are now presented with the **EshopOnWeb (Documents)** Wiki, as well as having the **Welcome to our Online Retail Store(3)** as **HomePage** of the Wiki.
     
     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a8788d9b-3bd9-4fd0-90fe-63f8c221da8f)

### Exercise 2: Create and manage a project wiki

In this exercise, you will step through creating and managing a project wiki.

> **Note**: You can create and manage wiki independently of the existing repos. 

#### Task 1: Create a project wiki including a Mermaid diagram and an image

In this task, you will create a project wiki and add to it a Mermaid diagram and an image.

1.  On your lab VM, in the Azure DevOps portal displaying the **Overview(1)** and **Wiki(2)** pane of the **EShopOnweb** project, with the content of the **EShopOnWeb (Documents)(3)** wiki selected, at the top of the pane, click the **EShopOnWeb (Documents)** dropdown list header, and, in the drop down list, select **Create new project wiki(4)**. 

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/89f9619f-beed-4f6f-a046-486b7520f1ce)

2.  In the Page title text box, type **Project Design**.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/aac19ef9-9c47-4ab5-a17f-19b71a8f9dc1)

3.  Place the cursor in the body of the page, click the left-most icon in the toolbar representing the header setting and, in the dropdown list, click **Header 1**. This will automatically add the hash character (**#**) at the beginning of the line.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/63cc6507-b9e2-4bac-9dc2-7451e613af17)
    
    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f083d671-f86b-407a-871b-00db90970b2e)

4.  Directly after the newly added **#** character, type **Authentication and Authorization** and press the **Enter** key.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a717b53c-7c86-442c-8ffe-edcbf5df4ca6)

5.  Click the left-most icon in the toolbar representing the header setting and, in the dropdown list, click **Header 2**. This will automatically add the hash character (**##**) at the beginning of the line.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/90efc132-65ca-417a-bcd0-bd63d57b93ef)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/e1d6419d-3dd6-4aa8-94e8-8de96ffe81a2)

6.  Directly after the newly added **##** character, type **Azure DevOps OAuth 2.0 Authorization Flow** and press the **Enter** key.

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/f08324b5-bcd2-4072-9ac1-445f0e87e73f)

7.  **Copy and paste** the following code to insert a mermaid diagram on your wiki.

    ```
    ::: mermaid
     sequenceDiagram
     participant U as User
     participant A as Your app
     participant D as Azure DevOps
     U->>A: Use your app
     A->>D: Request authorization for user
     D-->>U: Request authorization
     U->>D: Grant authorization
     D-->>A: Send authorization code
     A->>D: Get access token
     D-->>A: Send access token
     A->>D: Call REST API with access token
     D-->>A: Respond to REST API
     A-->>U: Relay REST API response
    :::
    ```

    >**Note**: For details regarding the Mermaid syntax, refer to [About Mermaid](https://mermaid-js.github.io/mermaid/#/)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/a7d2219c-88ea-4a24-992e-9fd8f78a8ea2)


8.  To the right of the editor pane, in the preview pane, click **Load diagram** and review the outcome. 

    >**Note**: The output should resemble the flowchart that illustrates how to [Authorize access to REST APIs with OAuth 2.0](https://docs.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/oauth?view=azure-devops)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/d94a4c66-766f-432a-9000-16c2545f2bb9)

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/bf2d646f-0e33-499b-970d-51440016193f)

9.  In the upper right corner of the editor pane, click the down-facing caret next to the **Save** button and, in the dropdown menu, click **Save with revision message**. 

    ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/683085da-0d9d-4858-b0c0-1df8716789dd)

10.  In the **Save page** dialog box, type **Authentication and authorization section with the OAuth 2.0 Mermaid diagram** and click **Save**.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/2ecdc20b-cfa5-4aa9-b344-0848d335a127)

11.  On the **Project Design** editor pane, place the cursor at the end of the Mermaid element you added earlier in this task, press the **Enter** key to add an extra line, click the left-most icon in the **toolbar(1)** representing the header setting and, in the dropdown list, click **Header 2(2)**. This will automatically add the double hash character (**##(3)**) at the beginning of the line.

      ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/1038862f-9dc6-48af-8b5a-0f4d9b6bf3b8)

12.  Directly after the newly added **##** character, type **User Interface** and press the **Enter** key.

     ![image](https://github.com/prathimavalasapally/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/assets/127385764/0e3efc10-958b-4413-8a13-03fcad653fea)

15.  On the **Project Design** editor pane, in the toolbar, click the paper clip icon representing the **Insert a file** action, in the **Open** dialog box, navigate to the **Downloads** folder, select the **Website.png** file you downloaded in the previous exercise, and click **Open**.
16.  Back on the **Project Design** editor pane, review the preview pane and verify that the image is properly displayed.
17.  In the upper right corner of the editor pane, click the down-facing caret next to the **Save** button and, in the dropdown menu, click **Save with revision message**. 

    ![Azure DevOps](images/file-04.png)

1.  In the **Save page** dialog box, type **User Interface section with the Tailwind Traders image** and click **Save**.
1.  Back on the editor pane, in the upper right corner, click **Close**. 

#### Task 2: Manage a project wiki

In this task, you will manage the newly created project wiki.

>**Note**: You will start by reverting the most recent change to the wiki page.

1.  On you lab VM, in the Azure DevOps portal displaying the **Wiki pane** of the **Sharing Team Knowledge using Azure Project Wikis** project, with the content of the **Project Design** wiki selected, in the upper right corner, click the vertical ellipsis symbol and, in the dropdown menu, click **View revisions**.

    ![Azure DevOps](images/view-revisions-01.png)

    ![Azure DevOps](images/view-revisions-02.png)

1.  On the **Revisions** pane, click the entry representing the most recent change. 
1.  On the resulting pane, review the comparison between the previous and the current version of the document, click **Revert**, when prompted for the confirmation, click **Revert** again, and then click **Browse Page**. 
1.  Back on the **Project Design** pane, verify that the change was successfully reverted.

    >**Note**: Now you will add another page to the project wiki and set it as the wiki home page.

1.  On the **Project Design** pane, at the bottom left corner, click **+ New page**.
1.  On the page editor pane, in the **Page title** text box, type **Project Design Overview**, click **Save**, and then click **Close**.
1.  Back in the pane listing the pages within the **Project Design** project wiki, locate the **Project Design Overview** entry, select it with the mouse pointer, drag and drop it above the **Project Design** page entry. 
1.  Verify that the **Project Design Overview** entry is listed as the top level page with the home icon designating it as the wiki home page.

## Review

In this lab, you created and configured Wiki in an Azure DevOps, including managing markdown content and creating a Mermaid diagram.

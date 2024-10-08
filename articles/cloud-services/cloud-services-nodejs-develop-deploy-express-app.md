---
title: Build and deploy a Node.js Express app to Azure Cloud Services (classic)
description: Use this tutorial to create a new application using the Express module, which provides a Model-View-Control (MVC) framework for creating Node.js web applications.
ms.topic: article
ms.service: azure-cloud-services-classic
ms.date: 07/23/2024
author: hirenshah1
ms.author: hirshah
ms.reviewer: mimckitt
ms.custom: compute-evergreen, devx-track-js
---

# Build and deploy a Node.js web application using Express on an Azure Cloud Services (classic)

[!INCLUDE [Cloud Services (classic) deprecation announcement](includes/deprecation-announcement.md)]

Node.js includes a minimal set of functionality in the core runtime.
Developers often use non-Microsoft modules to provide more
functionality when developing a Node.js application. In this tutorial,
you create a new application using the [Express](https://github.com/expressjs/express) module, which provides a Model-View-Control framework for creating Node.js web applications.

The following screenshot shows the completed application:

![A web browser displaying Welcome to Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## Create a Cloud Service Project
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Perform the following steps to create a new cloud service project named `expressapp`:

1. From the **Start Menu** or **Start Screen**, search for **Windows PowerShell**. Finally, right-click **Windows PowerShell** and select **Run As Administrator**.

    ![Azure PowerShell icon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Change directories to the **c:\\node** directory and then enter the following commands to create a new solution named `expressapp` and a web role named **WebRole1**:

   ```powershell
   PS C:\node> New-AzureServiceProject expressapp
   PS C:\Node\expressapp> Add-AzureNodeWebRole
   PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   ```

   > [!NOTE]
   > By default, **Add-AzureNodeWebRole** uses an older version of Node.js. The preceding **Set-AzureServiceProjectRole** line instructs Azure to use v0.10.21 of Node. Note the parameters are case-sensitive. You can verify the correct version of Node.js has been selected by checking the **engines** property in **WebRole1\package.json**.

## Install Express
1. Install the Express generator by issuing the following command:

    ```powershell
    PS C:\node\expressapp> npm install express-generator -g
    ```

    The following screenshot shows the output of the npm command. Your output should look similar.

    ![Windows PowerShell displaying the output of the npm install express command.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Change directories to the **WebRole1** directory and use the express command to generate a new application:

    ```powershell
    PS C:\node\expressapp\WebRole1> express
    ```

    To continue, enter **y** or **yes** when prompted to overwrite your earlier application. Express generates the app.js file and a folder structure for building your application.

    ![The output of the express command](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)

3. To install the other dependencies defined in the package.json file,
   enter the following command:

    ```powershell
    PS C:\node\expressapp\WebRole1> npm install
    ```

   ![The output of the npm install command](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

4. Use the following command to copy the **bin/www** file to **server.js**. This step allows the cloud service to find the entry point for this application.

    ```powershell
    PS C:\node\expressapp\WebRole1> copy bin/www server.js
    ```

   After this command completes, you should have a **server.js** file in the WebRole1 directory.

5. Modify the **server.js** to remove one of the '.' characters from the following line.

    ```js
    var app = require('../app');
    ```

   Once you make this modification, the line should appear as follows:

    ```js
    var app = require('./app');
    ```

   This change is required since we moved the file (formerly `bin/www`) to the same directory as the app file being required. After making this change, save the **server.js** file.
6. Use the following command to run the application in the Azure emulator:

    ```powershell
    PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
    ```

    ![A web page containing welcome to express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## Modifying the View
Now modify the view to display the message "Welcome to Express in
Azure."

1. Enter the following command to open the index.jade file:

    ```powershell
    PS C:\node\expressapp\WebRole1> notepad views/index.jade
    ```

   ![The contents of the index.jade file.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

   Jade is the default view engine used by Express applications. 
   
2. Modify the last line of text by appending **in Azure**.

   ![The index.jade file, the last line reads: p Welcome to \#{title} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Save the file and exit Notepad.
4. To see your changes, refresh your browser.

   ![A browser window, the page contains Welcome to Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

After testing the application, use the **Stop-AzureEmulator** cmdlet to stop the emulator.

## Publishing the Application to Azure
In the Azure PowerShell window, use the **Publish-AzureServiceProject** cmdlet to deploy the application to a cloud service

```powershell
PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch
```

Once the deployment operation completes, your browser opens and displays the web page.

![A web browser displaying the Express page. The URL indicates it is now hosted on Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## Next steps
For more information, see the [Node.js Developer Center](/azure/developer/javascript/).

[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: https://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com

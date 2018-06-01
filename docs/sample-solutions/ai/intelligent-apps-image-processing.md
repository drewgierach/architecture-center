---
title: Intelligent apps - image processing on Azure
description: Proven solution for building image processing into your Azure applications.
author: david-stanford
ms.date: 06/01/2018
---
# Intelligent Applications - Image Processing on Azure

This example scenario is applicable for businesses that have an image processing need. Potential applications include classifying images for a fashion website, analyzing text and images for insurance claims or understanding telemetry data from game screenshots. Traditionally, companies would need to develop expertise in machine learning models, train the models, and then finally run the images through their custom process to get the data out of the images. By leveraging Azure services such as the Computer Vision API and Azure Functions, companies can remove the undifferentiated heavy lifting with an on-premises or IaaS deployment, while reducing costs and leveraging ….” “… This scenario will specifically solve X, however if you need Y or Z, you may want to consider….”

## Potential use cases

You should consider this solution for the following use cases:

* Classifying images on a fashion website.
* Classify images & text for insurance claims
* Capture and classify telemetry data from screenshots of games.

## Architecture diagram

The solution diagram below is an example of this solution:

![Intelligent apps architecture - computer vision][architecture-computer-vision]

## Architecture

In this example, image processing functions would invoke the Computer Vision Cognitive Service for automatically creating the caption and the tags from any supplied images. A mixture of pre-built AI in the form of Cognitive Services and custom AI in the form of Azure ML services would be used to process the text. Azure Functions would be used to coordinate the calls to the classifications and summary AI services which would run as containerized web services in Azure Container Service, while the Text Analytics API could be invoked directly to provide a sentiment score for each text.
Once all processing has completed, one final Azure Function could be used to insert the complete document into Azure Search for future searches. The document inserted would contain specific tabs and search metadata, so that it could always be tied back to the record store in Azure SQL Database.

These are the components found in this solution:

* [Resource Groups][resource-groups] is a logical container for Azure resources.
* [Computer Vision API][computer-vision-docs] is part of the Cognitive Services suite and is used to retrieve information about each image.
* [Azure Functions][functions-docs]: this provides the backend API for the web application, as well as the event processing for uploaded images.
* [Event Grid][eventgrid-docs]: triggers an event when a new image is uploaded to blob storage, that is then processed with Azure functions.
* [Blob Storage][storage-docs]: you use Azure Blob storage to host all of the image files that are uploaded into your web application, as well any static files that your web application consumes.
* [Cosmos DB][cosmos-docs]: Cosmos DB is used to hold metadata about each image that is uploaded, including the results of the processing from Computer Vision API.

## Architecture considerations

Here we will discuss the major architectural components of the solution, what some of the alternative options are, and why we selected the things we did.

### Data storage

There are a couple types of data present in this scenario. Raw data that relates to each individual customer submission, data derived via machine learning, and finally the meta-data to relate raw data to the customer.

We are storing the raw image data in Azure blob storage, the other storage options are detailed in our [storage documentation][storage-docs].

### AI Processing

In this solution we are primarily processing images, there are two main options in Azure to consider: Computer Vision API & the Custom Vision API. The main difference between the two is the computer vision API comes pre-trained and will give you a good amount of information by default. You need to provide training images and classifications to the custom vision API in order for it to give you back the information that you might be looking for.

## Deploy the solution

To deploy this sample solution into your account follow the directions below:

**Prerequisites.** You must have an existing Azure account.

To deploy the solution, perform the following steps.
1. Select the button below:<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Farchitecture-center%2Fmaster%2Fsample-solutions%2Fai%2Ftemplates%2Fintelligent-apps-image-processing.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>
2. Wait for the link to open in the Azure portal, then follow these steps: 
   * The **Resource group** name is already defined in the parameter file, so select **Create New** and enter `computer-vision-solution` in the text box.
   * Select the region from the **Location** drop down box.
   * Do not edit the **Template Root Uri** or the **Parameter Root Uri** text boxes.
   * Review the terms and conditions, then click the **I agree to the terms and conditions stated above** checkbox.
   * Click the **Purchase** button.
3. Wait for the deployment to complete.

## Pricing

Explore the [cost][pricing] of running this solution, all of the services are pre-configured in the cost calculator.  To see how the pricing would change for your particular use case change the appropriate variables to match your expected traffic.

## Related Resources

For a guided learning path of this solution please see [Build a serverless web app in Azure][serverless]

Best practice links  
Reference Architectures / Implementations

<!-- links -->
[pricing]: https://azure.com/e/d886c0e6c8b2414a9485d67c4691cf2c
[functions-docs]: https://docs.microsoft.com/en-us/azure/azure-functions/
[computer-vision-docs]: https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/home
[storage-docs]: https://docs.microsoft.com/en-us/azure/storage/
[architecture-computer-vision]: ./media/architecture-computer-vision.png
[serverless]: https://docs.microsoft.com/en-us/learn/build-serverless-app/index
[cosmos-docs]: https://docs.microsoft.com/en-us/azure/cosmos-db/
[eventgrid-docs]: https://docs.microsoft.com/en-us/azure/event-grid/
[resource-groups]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview
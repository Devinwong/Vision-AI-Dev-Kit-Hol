# Exercise 1 – Set up device and deploy the get-started vision module

In this exercise, you will set up your camera and deploy the get-started vision module with the guidance of the OOBE (Out-Of-Box Experience) webpages.

## Connecting the AI Vision Developer Kit to Internet and Azure IoT Hub/Edge

1. Connect a USB-type C cable from your workstation to the AI Vision Developer Kit. This is the only cable we need to provide power and transfer data.
2. Make sure the device LEDs are blinking RED. From your workstation, check the Wi-Fi scanning results and you will see an MSIOT\_xxxxxx (printed on the label at the bottom of the device) discovered where xxxxxx is the last 6 characters of your Wi-Fi mac address. For example, it looks like MSIOT\_BD097D. Please check this clear so that you won&#39;t connect to other attendees&#39; cameras. Connect to that Soft Access Point broadcasted by Peabody.

The default passphrase is printed on a label at the bottom of the camera, for example: password: _88888888_. If you didn&#39;t see this line on the label, please use _12345678_ as passphrase.

1. Once you connected to above Wi-Fi network, a default OOBE setup page will pop up automatically. If not, please launch a browser and visit http://setupaicamera.ms.
2. Follow the setup page to fill in the necessary information. You can use the default configuration. For the Wi-Fi SSID in the 3rd page, please select an AP that can connect to internet (not MSIOT\_xxxxxx). The Wi-Fi profile will be stored in Peabody so that it can automatically connect to the same Wi-Fi network after reboot.

**_Tips_** _: The Wi-Fi connection in the lab venue may be slow and congested because of the limited bandwidth for too many devices connecting at the same time. Please feel_ **_free to use your own cell phone hotspot if you encounter issues._**

1. Follow the steps in the webpages to create corresponding Azure resources and deploy the get-started vision module from Azure Marketplace.
2. You will be able to see the inferencing output on HDMI on completion.

In this exercise, you set up the Vision AI Dev Kit so that it connects to internet and IoT Hub. When you went through the OOBE webpages to create the IoT Edge device, it also associated to your camera in the background. Finally, it deployed the get-started vision module from Azure Marketplace dynamically and then you were able to see the inferencing output on HDMI.

# Exercise 2 – Build your own AI model

We&#39;ll build our own AI model (Image Classification) to detect when someone is wearing a hard hat. You will share a hard hat with other attendees to validate your model built with Custom Vision.

## Setup up a new Custom Vision project

1. Login to the Azure Custom Vision Service (Preview) at [https://www.customvision.ai/](https://www.customvision.ai/).
2. Create a new project, use these recommended settings:
  Give it a name like _Simulated HardHat Detector_
  Project Type - _[Classification]_
  Create a resource group
  Classification Type - _[Multiclass (Single tag per image)]_
  Domain - _[General(compact)]_
  Export Capabilities - _Vision AI Dev Kit_

## Upload and tag your training data

Some training images have already been collected for you for the hard hat use case. Download them and upload them to your Custom Vision project:

1. Downlaod the .zip file at this location: [https://1drv.ms/u/s!AkzLzaBpSgoMo9hXX4NPjd8QrfhQLA?e=M3ehCL](https://1drv.ms/u/s!AkzLzaBpSgoMo9hXX4NPjd8QrfhQLA?e=M3ehCL)
2. Decompress it
3. Upload images to custom vision per tag (HardHat/NoHardHat) and tag them appropriately them during upload. Upload all pictures names similarly (like HardHat) at the same time.

## Train and export your custom model

1. To train it with your new training data, go to your Custom Vision project and click on _Train_.
2. To export it, select tab _Performances_ and _Export_ button. Choose the _Vision AI Dev Kit_ option to download your new custom model in a zip file.

## Deploy your custom model to your device

To deploy your custom model, we will first store your model in a publicly accessible location and then update the configuration of the Get Started module to use this model instead of the default one. We will use a cloud blob store to store the model, but a public OneDrive link would just work as well.

## Uploading new custom model files

We&#39;ll start by creating a new storage account and then upload your model to it.

1. Login to the [http://portal.azure.com](http://portal.azure.com/)
2. Search for _Storage_ and select the _Storage accounts_ _service_
3. Use your existing subscription and resource group
4. Give it a unique name, upper case characters are not allowed
5. Select the _West-US 2_ region
6. Click on Review + Create (other default options should be correct)
7. Wait until provisioning is complete and navigate to your new storage account
8. From the _overview_ tab, click on _Blobs_ service
9. _Add a new container_, give it a name like _hardhatmodel_ and make sure to select _Container (anonymous read access for containers and blobs)_ for the _Public access level_
10. Click on the container just created, click on the _Upload_ button and select your model files in a zip file from the custom vision service and _Upload_
11. Copy the url of the zip file by clicking on it and copying its URL to the clipboard.

## Updating the configuration of the Get Started module to use your new custom model

1. Login to the [http://portal.azure.com](http://portal.azure.com/) and go to your IoT Hub resource
2. Click on IoT Edge tab and then on your camera device name.
3. Click on the _AIVisionDevKitGetStartedModule_ module name and click on _Module Identity Twin_
4. Update the three desired properties (model, label, vam config) to map to your new URLs and hit _Save_

Within a few minutes, your device should now be running your custom model! You can check the progress locally from the device. For details please see next exercise.

## Test your new model

1. Put your hard hat on and smile at the camera!
2. Verify that the camera picks it up and correctly classify you as wearing the hard hat.

In this exercise, you trained your own Image Classification model on Microsoft Custom Vision Service. Then you got the DLC files from the Custom Vision Service output. By using the _Module Identity Twin,_ you replaced your model directly from cloud storage to local device in a few minutes.

# Exercise 3 (Optional) – Access the device locally

The device provides a shell to directly access to local resources. Here are a few commands you may find them useful. Please use a command prompt to get started.

Please install the USB driver first. It's located at desktop > Lab Room > 0521 > Qud.win.
After installed it, please launch a command prompt and traverse to desktop > Lab Room >Platform tools and then start using the following commands.

| Commands | Descriptions |
| --- | --- |
| adb devices | Check if your workstation detects the camera correctly |
| adb shell | Enter the local shell |
| ifconfig | Check the network configurations |
| docker ps | List what containers are running on Docker |
| docker images | List what docker images have been downloaded to local device |
| docker logs -f edgeAgent | Check by following the logs output from edgeAgent container. Ctrl + c to exit |
| docker logs -f AIVisionDevKitGetStartedModule | Check by following the logs output from the vision module |

# Exercise 4 (Optional) – Build your own Object detection AI model

As a next step, you could reuse the same training images but build an object detection model instead of the image classification model in exercise 2. Again, please use Custom Vision Service [https://www.customvision.ai/](https://www.customvision.ai/) and its labeling tool.

To learn more about this Vision AI Dev Kit, visit [https://azure.github.io/Vision-AI-DevKit-Pages/docs/Get\_Started/](https://azure.github.io/Vision-AI-DevKit-Pages/docs/Get_Started/)

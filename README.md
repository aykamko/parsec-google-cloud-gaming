# Google Cloud + Parsec Gaming PC

### What is this?

This repo sets you up to play games like [Doom Eternal](https://store.steampowered.com/app/782330/DOOM_Eternal/) on your own laptop! You can play with all graphics set to max since you'll be streaming the screen from a beefy gaming PC in Google Cloud.

We just need to set up two things:

- A Google Cloud virtual machine with a [Telsa P100 GPU](https://www.microway.com/knowledge-center-articles/comparison-of-nvidia-geforce-gpus-and-nvidia-tesla-gpus/) running Windows Server 2019
  - If you haven't signed up for Google Cloud before, you get **$300 in credits for free!**
  - The virtual machine **costs about $1.30/hour** to run. Don't worry about getting overcharged — we'll set up your machine to shut down automatically when you're done playing
- [A Parsec account](https://parsecgaming.com/). This lets us stream the screen from the server at super low latency. It's sorta like [Google Stadia](https://store.google.com/product/stadia?gclid=CjwKCAjwguzzBRBiEiwAgU0FT7GirMrN5XiJOHrRMcFNXx6Y1a3BGxoZ2mX1wEKSO5e-3urfE4NeoxoCwd8QAvD_BwE).

---

### Guide

This should take about 30 minutes to set up. Could take longer depending on how fast Google responds to you via email after step 4.

1. [Sign up for Google Cloud.](https://cloud.google.com/gcp/) Choose the Google account you'll want to get your billed under.
1. Once you're at the main Google Cloud console page ([console.cloud.google.com](https://console.cloud.google.com/)), let's create a new Project
   1. Click `My First Project` at the top of the page
        ![a](https://dl.dropboxusercontent.com/s/vduxhpl4dgwnhw5/Screenshot%202020-03-25%2020.28.47.png?dl=0)
   1. Click `NEW PROJECT` at the top-right of the modal
   1. Call your new project `Parsec`
   1. Wait until your new project has finished creating
1. Using the search bar up top, search `vm instances` and click the result labeled "Instances"
       ![b](https://dl.dropboxusercontent.com/s/0uzkuqcssfim1oa/Screenshot%202020-03-25%2020.36.46.png?dl=0)
   1. You'll be brought to a screen saying
   
        > Compute Engine is getting ready.
        
      Wait for this message to go away.
   1. Click `Create` __but don't do anything else__. Move on to the next step.
1. Request an increase to your GPU quota. We need to ask Google Cloud to let us create a machine with a GPU attached.
   1. In the search bar up top, search `all quotas` and click the result
        ![c](https://dl.dropboxusercontent.com/s/zv6wwj4narkxr1l/Screenshot%202020-03-25%2020.34.11.png?dl=0)
   1. Under the dropdowns, you'll see a banner asking you to upgrade your account. Click "Upgrade Account" (twice), then refresh the page
        ![d](https://dl.dropboxusercontent.com/s/4sj1brjrw158i40/Screenshot%202020-03-25%2020.42.21.png?dl=0)
   1. Click the dropdown for `Metrics` and click `None` to deselect everything.
   1. Search `GPUs all` and select the only option
       ![e](https://dl.dropboxusercontent.com/s/75ffmx4b909dtw3/Screenshot%202020-03-25%2020.40.45.png?dl=0)
   1. Click the checkbox on the only result in the list, then click "Edit Quotas" at the top of the page
       ![f](https://dl.dropboxusercontent.com/s/l75j4wqeqcjy608/Screenshot%202020-03-25%2020.43.49.png?dl=0)
   1. In the right-side drawer:
      - Fill out your info and click "Next"
      - Request a new quota of `1` (up from zero) and provide a request description — be creative
      - Click "Done" on the small box and then click "Submit Request"
   1. Google now has to approve your quota increase. They'll send you an email when they confirm. This may take a while, but in practice it takes less than an hour. Move on to the next step while you wait.

1. [Download and install Parsec](https://parsecgaming.com/). Open Parsec and make an account. You'll eventually need to sign in on the cloud machine as well, so **remember your username+password**!
1. [Download and install VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/). You may need this later to connect and use the remote desktop on your cloud machine.
1. Download the default Service Account key
    1. [Go back to the Google Cloud console](https://console.cloud.google.com/)
    1. Search `credentials` at the top and click "APIs & Services"
        ![g](https://dl.dropboxusercontent.com/s/0ussa3kfm19aa42/Screenshot%202020-03-25%2020.48.26.png?dl=0)
    1. In the left-hand drawer, click "Credentials"
    1. Under the "Service Accounts" section, click the one named "Compute Engine default service account"
        ![h](https://dl.dropboxusercontent.com/s/uv33vzp4a6uz0h4/Screenshot%202020-03-25%2020.50.02.png?dl=0)
    1. Click "Create Key" at the bottom, choose JSON
    1. Save the key as `account.json` to root of this repo
1. Wait until Google Cloud approves your GPU quota increase from step 4.
1. Create the virtual machine. 
   - **WARNING!** Don't stop following the guide here! This step will start your cloud machine and you will start getting charged. **Make sure to stop the machine before leaving!!!**
   - Use [Terraform](https://www.terraform.io/) to automatically set up your machine. Open terminal and run the following
    
    ```bash
    brew install terraform
    cd <REPLACE ME WITH PATH TO REPO>/terraform
    terraform init
    terraform apply
    ```
    
1. Set a Windows password
    1. [Go back to the Google Cloud console](https://console.cloud.google.com/)
    1. Using the search bar up top, search `vm instances` and click the result labeled "Instances"
        ![i](https://dl.dropboxusercontent.com/s/0uzkuqcssfim1oa/Screenshot%202020-03-25%2020.36.46.png?dl=0)
    1. Click on your newly created instance called `parsec-1`
    1. Near the top of the page, click "Set Windows password"
        ![j](https://dl.dropboxusercontent.com/s/aopu2eouf3notxh/Screenshot%202020-03-25%2021.00.15.png?dl=0)
    1. Follow the instructions and **save the generated password**
1. Use RDP to connect to Windows VM
    1. Click "RDP" near the top of the instance info page
    1. Follow the instructions until you get to the remote desktop
1. Once your on the remote desktop, let's set up Parsec. **This step takes a while.**
    1. Follow steps 3 and 4 at https://github.com/jamesstringerparsec/Parsec-Cloud-Preparation-Tool
    1. Open "Auto Login" on the desktop and run "Setup Auto Login"
    1. Run "Setup Auto Shutdown" on the desktop. Choose a reasonable time (I use 30 minutes)
    1. Open Google Chrome and download the latest Google Cloud NVIDIA driver from here: https://storage.googleapis.com/nvidia-drivers-us-public/GRID/GRID10.1/442.06_grid_win10_server2016_server2019_64bit_international.exe
    1. Run the downloaded .exe to install the driver
1. Set up VNC
    1. From the start menu, search "tightvnc" and run "TightVNC Service - Control Interface"
         ![k](https://dl.dropboxusercontent.com/s/f61rxic04000e5d/Screenshot%202020-03-25%2021.07.27.png?dl=0)
    1. In the bottom-right icon tray, click the TightVNC icon
         ![l](https://dl.dropboxusercontent.com/s/qp5ce5w8y37aydz/Screenshot%202020-03-25%2021.07.44.png?dl=0)
    1. Use your Windows password to unlock the settings
    1. Change the VNC password to be your Windows password
1. Restart the host and connect with VNC.
    1. Restart Windows using the start menu
    1. Wait a 2 minutes for your host to reboot
    1. On your Mac, open VNC Viewer, installed in Step 6
    1. Copy the External IP address of your cloud machine from the info page on the Google Cloud console
        ![m](https://dl.dropboxusercontent.com/s/1b8gg1c6k2o8l4a/Screenshot%202020-03-25%2021.12.50.png?dl=0)
    1. Paste the IP into VNC Viewer to connect to your machine. The password will be your Windows password which you just set
    1. You should be connected to the host via VNC now!
1. Last bit of Parsec setup
    1. From VNC, open Parsec and sign in
    1. Go to Settings. Scroll to the bottom and click the small text saying `edit the configuration file directly`. Paste these settings and save the file (adjust resolution as necessary, these values are tuned for a Macbook Pro):
    
       ```
       app_run_level = 3
       network_server_start_port = 8000
       client_decoder_h265 = true
       server_resolution_x = 2880
       server_resolution_y = 1800
       encoder_h265 = 1
       ```
     
    1. From the icon tray in the bottom-right of the desktop, right click on Parsec and restart it.
    1. Right click the desktop and open "Display Settings"
    1. Click on Display 2 and check the box that says "Make this my main display"
    1. Set the scaling factor to 150%.
1. On your Mac, open Parsec and connect to your cloud machine. **Done!!!**

At this point, you can install Steam and whatever games you want. Voila!

**Don't forget to turn off your machine when you're not using it, or you'll be charged!** Google Cloud charges by the second.

----

### Troubleshooting
  - Maybe set GPU into WDDM mode: https://docs.nvidia.com/gameworks/content/developertools/desktop/nsight/tesla_compute_cluster.htm

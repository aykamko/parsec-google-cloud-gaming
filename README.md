1. Sign up for Google Cloud
1. TODO: Make a project, etc
1. Download the defualt Service Account key
    1. Go to APIs & Services > Credentials
    1. Under "Service Accounts", click the one named "Compute Engine default service account"
    1. Click "Create Key" at the bottom, choose JSON
    1. Save the key as `account.json` to root of this repo
1. Create VM
    ```
    cd terraform
    terraform init
    terraform apply
    ```
1. Set a Windows password
    1. In GCP, go to Compute Engine > VM instances
    1. Click on your newly created instance called `parsec-1`
    1. Near the top of the page, click "Set Windows password"
    1. Follow the instructions and **save the generated password**
1. Use RDP to connect to Windows VM
    1. Click "RDP" near the top of the instance info page
    1. Follow the instructions until you get to the remote desktop
1. Follow instructions at https://github.com/jamesstringerparsec/Parsec-Cloud-Preparation-Tool
    - do this: https://www.dropbox.com/s/aazhorq9caiixvv/Screenshot%202020-03-21%2012.41.03.png?dl=0
    - also:
        - setup auto shutdown (30 min)
1. Make the second display the Primary display
1. Set Parsec host port to 8000

TODO (unfinished instructions)
====
1. Install latest NVIDIA GRID driver from Google Cloud bucket
  - https://storage.googleapis.com/nvidia-drivers-us-public/GRID/GRID10.1/442.06_grid_win10_server2016_server2019_64bit_international.exe
  - Maybe set GPU into WDDM mode: https://docs.nvidia.com/gameworks/content/developertools/desktop/nsight/tesla_compute_cluster.htm
1. For MBP, set custom resolution

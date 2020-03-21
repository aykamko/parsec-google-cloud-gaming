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

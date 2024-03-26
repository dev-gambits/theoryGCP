# App Dev - Setting up a Development Environment: Node.js 

* [LAB-1](https://www.cloudskillsboost.google/course_templates/22/labs/446802)

### App Dev - Setting up a Development Environment: Node.js
schedule
2 hours
universal_currency_alt
5 Credits
Overview
In this lab, you provision a Google Compute Engine virtual machine and install software libraries for Node.js software development on Google Cloud Platform (GCP).

Objectives
In this lab, you learn how to perform the following tasks:

Provision a Google Compute Engine instance.
Connect to the instance using SSH.
Install software on the instance.
Verify the software installation.
Setup and requirements
For each lab, you get a new Google Cloud project and set of resources for a fixed time at no cost.

Sign in to Qwiklabs using an incognito window.

Note the lab's access time (for example, 1:15:00), and make sure you can finish within that time.
There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click Start lab.

Note your lab credentials (Username and Password). You will use them to sign in to the Google Cloud Console.

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.
If you use other credentials, you'll receive errors or incur charges.

Accept the terms and skip the recovery resource page.

Note: Do not click End Lab unless you have finished the lab or want to restart it. This clears your work and removes the project.

Task 1. Create a Compute Engine Virtual Machine instance
In this section, you use the Google Cloud Platform Console to provision a new Google Compute Engine virtual machine (VM) instance.

In the Cloud Platform Console, on the Navigation menu, select Compute Engine > VM instances.

On the VM Instances dialog, click Create Instance.

On the Create an instance dialog:

Name the instance dev-instance.
Set the Region to us-central1.
Set the Zone to us-central1-a.
Note: Regions and zones
Google Cloud offers products and services in multiple distinct geographic locations, called regions. Each region has multiple distinct zones. Each zone is isolated from other zones in terms of power and internet connectivity.
In the Identity and API access > Access Scopes section, select Allow full access to all Cloud APIs.
In the Firewall section, enable Allow HTTP traffic.
Leave the remaining settings as their defaults, and click Create.
Note: It takes about 20 seconds for the virtual machine to be provisioned and started.
On the VM instances dialog, in the row for the dev-instance, click SSH (in the Connect column).
Note: This launches a browser-hosted SSH session. If you have a popup blocker, you may need to click twice. There's no need to configure or manage SSH keys.
Click Check my progress below to verify the objective.
Create a Compute Engine Virtual Machine Instance

Task 2. Install software on the VM instance
In this section, you use the SSH session to install software on your VM instance.

In the SSH session, to update the Debian package list, execute the following command:

sudo apt-get update
Copied!
To install Git, execute the following command:

sudo apt-get install git
Copied!
If prompted, press Y then Enter to continue.

To install Node.js and Node Package Manager (npm), execute the following commands:

sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
Copied!
Click Check my progress to verify the objective.
Install software on the VM instance

Task 3. Configure the VM to run application software
In this section, you verify the software installation and run some sample codes.

To check the version of Node.js, execute the following command:

node -v
Copied!
Note: You should see the Node.js version 20.x.x.
To clone the class repository, execute the following command:

git clone --depth=1 https://github.com/GoogleCloudPlatform/training-data-analyst
Copied!
Create a soft link as a shortcut to the working directory:

ln -s ~/training-data-analyst/courses/developingapps/v1.3/nodejs/devenv ~/devenv
Copied!
Navigate to the directory that contains the sample files for this lab, execute the following command:

cd ~/devenv
Copied!
To run a simple web server, execute the following command:

sudo node server/app.js
Copied!
Return to the Cloud Console VM instances list, and click on the External IP address for the dev-instance.

Note: A browser tab opens to display a Hello GCP dev! message from Node.js.
Return to the SSH window and stop the application by pressing Ctrl+C.

To install the Node.js library for Compute Engine, execute the following command:

npm install
Copied!
To run a simple Node.js application that lists Compute Engine instances, execute the following command:

node list-gce-instances.js
Copied!
Note: Many details about your machine should appear in the terminal window.
Note: If you try to do this on your own machine, it will not work if credentials have not been set up to access Google Cloud on your machine.
Click Check my progress to verify the objective.
Clone the repository

Review
Congratulations! You learned how to provision a Google Compute Engine virtual machine and install software libraries for Node.js software development on Google Cloud Platform.

End your lab
When you have completed your lab, click End Lab. Google Cloud Skills Boost removes the resources you’ve used and cleans the account for you.

You will be given an opportunity to rate the lab experience. Select the applicable number of stars, type a comment, and then click Submit.

The number of stars indicates the following:

1 star = Very dissatisfied
2 stars = Dissatisfied
3 stars = Neutral
4 stars = Satisfied
5 stars = Very satisfied
You can close the dialog box if you don't want to provide feedback.

For feedback, suggestions, or corrections, please use the Support tab.

Copyright 2022 Google LLC All rights reserved. Google and the Google logo are trademarks of Google LLC. All other company and product names may be trademarks of the respective companies with which they are associated.
# App Dev - Setting up a Development Environment: Python

* [LAB-3](https://www.cloudskillsboost.google/course_templates/22/labs/446804)

### App Dev - Setting up a Development Environment: Python
schedule
2 hours
universal_currency_alt
5 Credits
Objectives
In this lab, you set up a Python development environment on Google Cloud. You then use Compute Engine to create a virtual machine (VM) and install software libraries for software development.

You perform the following tasks:

Provision a Compute Engine instance
Connect to the instance using SSH
Install a Python library on the instance
Verify the software installation
Overview
Compute Engine is just one resource provided on Google Cloud.

Google Cloud
Google Cloud consists of a set of physical assets, such as computers and hard disk drives, and virtual resources, such as virtual machines (VMs), that are contained in Google's data centers around the globe. Each data center location is in a global region. Regions include Central US, Western Europe, and East Asia. Each region is a collection of zones, which are isolated from each other within the region. Each zone is identified by a name that combines a letter identifier with the name of the region. For example, zone a in the East Asia region is named asia-east1-a.

This distribution of resources provides several benefits, including redundancy in case of failure and reduced latency by locating resources closer to clients. This distribution also introduces some rules about how resources can be used together.

Projects
Any Google Cloud resources that you allocate and use must belong to a project. You can think of a project as the organizing entity for what you're building. A project is made up of the settings, permissions, and other metadata that describe your applications.

Resources within a single project can work together easily, for example by communicating through an internal network, subject to the regions-and-zones rules. The resources that each project contains remain separate across project boundaries; you can only interconnect them through an external network connection.

Each Google Cloud project has a:

Project name, which you provide. In this lab, the project name is the same as the project ID.
Project ID, which you can provide or Google Cloud can provide for you. For this lab, the project ID is the Google Cloud Project ID provided when you start the lab.
Project number, which Google Cloud provides.
As you work with Google Cloud, you'll use these identifiers in certain command lines and API calls. When you open and log into the Google Cloud Console, the DASHBOARD provides the Project info:

Dashboard displaying the project name, project ID and project number.

In this example:

Field	Value
Project name	qwiklabs-gcp-gcpd-30d966efdb51
Project ID	qwiklabs-gcp-gcpd-30d966efdb51
Project number	734845473929
Each project ID is unique across Google Cloud. Once you have created a project, you can delete the project but its project ID can never be used again.

When billing is enabled, each project is associated with one billing account. Multiple projects can have their resource usage billed to the same account.

A project serves as a namespace. This means every resource within each project must have a unique name, but you can usually reuse resource names if they are in separate projects. Some resource names must be globally unique. Refer to the resource documentation for details.

In this lab, you provision a Compute Engine virtual machine (VM) and install software libraries for Python software development on Google Cloud.

Ways to interact with the services
Google Cloud provides three ways to interact with the services and resources.

Cloud Console: a web-based, graphical user interface that you can use to manage your Google Cloud projects and resources.

Command-line interface:

Google Cloud SDK: provides the gcloud command-line tool, which gives you access to the commands you need.
Cloud Shell: a browser-based, interactive shell environment for Google Cloud. You can access Cloud Shell from the Cloud console. If you prefer to work in a terminal window, the Google Cloud SDK provides the gcloud command-line tool, which gives you access to the commands you need. The gcloud tool can be used to manage both your development workflow and your Google Cloud resources. See the gcloud reference for the complete list of available commands.
Client libraries: The Cloud SDK includes client libraries that enable you to easily create and manage resources. Google Cloud client libraries expose APIs to provide access to services and resource management functions. You also can use the Google API client libraries to access APIs for products such as Google Maps, Google Drive, and YouTube.

Setup and requirements
Lab setup
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

Task 1. Create a Compute Engine virtual machine instance
In this section, you use the Cloud Console to provision a new Compute Engine (VM) instance.

Create and connect to a virtual machine
In the Console, click Navigation menu > Compute Engine > VM instances.

Expanded Navigation menu highlighting the Compute Engine submenu and VM instances option

On the VM instances dialog, click Create instance.

On the Create an instance dialog, set the following fields and leave all other fields at their default:

Name: type dev-instance
Region: us-central1 (Iowa)
Zone: us-central1-a.
Note: Google Cloud offers products and services in multiple distinct geographic locations, called regions. Each region has multiple distinct zones. Each zone is isolated from other zones in terms of power and internet connectivity.
In the Boot disk section, click Change, then select Debian GNU/Linux 11 (bullseye) x86/64, and then click Select.
In the Identity and API access section, under Access scopes, select Allow full access to all Cloud APIs.
In the Firewall section, enable Allow HTTP traffic.
Click Create.
It takes a approximately a minute to provision and start the VM.

Test completed task
Click Check my progress to verify your performed task. If you have completed the task successfully, the assessment score increases.

Create a Compute Engine Virtual Machine Instance (zone: us-central1-a)
On the VM instances dialog, in the dev-instance row, click SSH to launch a browser-hosted SSH session. If you have a popup blocker, you may need to click twice.
Note: There's no need to configure or manage SSH keys.
Install software on the VM instance
In the SSH session, update the Debian package list:
sudo apt-get update
Copied!
Install Git:
sudo apt-get install git
Copied!
When prompted, enter Y to continue, accepting the use of additional disk space.

Install Python:
sudo apt-get install python3-setuptools python3-dev build-essential
Copied!
Again, when prompted, enter Y to continue, accepting the use of additional disk space.

Install pip:
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 get-pip.py
Copied!
Test completed task
Click Check my progress to verify your performed task. If you have completed the task successfully you will granted with an assessment score.

Install software and configure the VM instance
Task 2. Configure the VM to run application software
In this section, you verify the software installation on your VM and run some sample code.

Verify Python installation
Still in the SSH window, verify the installation by checking the Python and pip version:
python3 --version
Copied!
pip3 --version
Copied!
The output provides the version of Python and pip that you installed.

Clone the class repository:
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
Copied!
Create a soft link as a shortcut to the working directory:
ln -s ~/training-data-analyst/courses/developingapps/v1.3/python/devenv ~/devenv
Copied!
Change the directory that contains the sample files for this lab:
cd ~/devenv/
Copied!
Run a simple web server:
sudo python3 server.py
Copied!
Return to the Cloud Console VM instances list (Navigation menu > Compute Engine > Virtual Instances), and click on the External IP address for the dev-instance.
External IP 35.225.28.129 highlighted

A browser opens and displays a Hello GCP dev! message from Python.

Test completed task
Click Check my progress to verify your performed task. If the check fails, wait a minute and try again. When task completes successfully, the assessment score increases.

Run application software to get a success response
Return to the SSH window, and stop the application by pressing Ctrl+C.
Install the Python packages needed to enumerate Compute Engine VM instances:
sudo pip3 install -r requirements.txt
Copied!
Now list your instance in Cloud Shell. Enter the following command to run a simple Python application that lists Compute Engine instances. Replace <PROJECT_ID> with your Google Cloud Project ID and <YOUR_VM_ZONE> is the region you specified when you created your VM. Find these values on the VM instances dialog of the console:
python3 list-gce-instances.py <PROJECT_ID> --zone=<YOUR_VM_ZONE>
Copied!
Your instance name shows in the SSH terminal window.

Output example:

qwiklabs-gcp-00-034bd0a24a62 and zone us-centrall-a: 
- dev-instance
Test your understanding
Below are multiple choice-questions to reinforce your understanding of this lab's concepts. Answer them to the best of your abilities.


pip is a package management system used to install and manage software packages written in Python.

True

False
End your lab
When you have completed your lab, click End Lab. Google Cloud Skills Boost removes the resources you’ve used and cleans the account for you.

You will be given an opportunity to rate the lab experience. Select the applicable number of stars, type a comment, and then click Submit.
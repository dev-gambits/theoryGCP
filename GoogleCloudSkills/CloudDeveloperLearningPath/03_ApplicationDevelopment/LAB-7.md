# App Dev - Storing Image and Video Files in Cloud Storage: Node.js

* [LAB-7](https://www.cloudskillsboost.google/course_templates/22/labs/446829)

### App Dev - Storing Image and Video Files in Cloud Storage: Node.js
schedule
2 hours
universal_currency_alt
5 Credits
Overview
In this lab, you enhance the online Quiz application to work with images and videos. You store files as objects in a Cloud Storage bucket.

The Quiz application user interface now includes a file input button in the add question form, and the handler can receive image data in the server-side application.

You add the code that stores the uploaded file into Cloud Storage, make the object publicly available, and then store the object URL and other application data in Google Cloud Datastore.

Objectives
In this lab, you will learn how to perform the following tasks:

Create a Cloud Storage bucket.
Review file upload UI and code changes.
Write code to store uploaded file data into Cloud Storage.
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

Activate Google Cloud Shell
Google Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud.

Google Cloud Shell provides command-line access to your Google Cloud resources.

In Cloud console, on the top right toolbar, click the Open Cloud Shell button.

Highlighted Cloud Shell icon

Click Continue.

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your PROJECT_ID. For example:

Project ID highlighted in the Cloud Shell Terminal

gcloud is the command-line tool for Google Cloud. It comes pre-installed on Cloud Shell and supports tab-completion.

You can list the active account name with this command:
gcloud auth list
Copied!
Output:

Credentialed accounts:
 - @.com (active)
Example output:

Credentialed accounts:
 - google1623327_student@qwiklabs.net
You can list the project ID with this command:
gcloud config list project
Copied!
Output:

[core]
project = 
Example output:

[core]
project = qwiklabs-gcp-44776a13dea667a6
Note: Full documentation of gcloud is available in the gcloud CLI overview guide .
Task 1. Reviewing the case study application
In this section, clone the git repository containing the Quiz application, and run the application.

Clone source code in Cloud Shell
In Cloud Shell, clone the repository for the class:
git clone --depth=1 https://github.com/GoogleCloudPlatform/training-data-analyst
Copied!
Create a soft link as a shortcut to the working directory:
ln -s ~/training-data-analyst/courses/developingapps/v1.3/nodejs/cloudstorage ~/cloudstorage
Copied!
Configure and run the case study application
Navigate to the directory that contains the sample files for this lab:
cd ~/cloudstorage/start
Copied!
To configure the application, execute the following command:
. prepare_environment.sh
Copied!
Note: This script file:
Creates an App Engine application.
Exports an environment variable, GCLOUD_PROJECT.
Runs npm install.
Creates entities in Cloud Datastore.
Prints out the Google Cloud Project ID.
To run the application, execute the following command:
npm start
Copied!
Click Check my progress to verify the objective.
Configure the case study application

Review the case study application
To view the application, click Web preview > Preview on port 8080.
Expanded web preview menu option with the Preview on port 8080 option highlighted

Click Create Question in the toolbar.
Note: You should see a simple form that contains textboxes for the question and answers and radio buttons to select the correct answer. The form has a new Image field to upload either image or video files.
Task 2. Examining the case study application Code
In this section, you use the Cloud Shell text editor to review the case study application code.

Launch the Cloud Shell Editor
In Cloud Shell, select Open Editor > Open in a new window.

Navigate to the /cloudstorage/start folder using the file browser panel on the left side of the editor.

Review the Express Web application
Select the add.pug file in the .../server/web-app/views/questions folder.
Note: This file contains the pug template for the Create Question form.
Notice how the form has been modified to use multipart/form-data as the enc-type, and there are two new form controls:
A file upload control called image
A hidden field called imageUrl
Select the questions.js file in the .../server/web-app folder.
Note: This file has been modified so that the POST handler chains together three distinct middleware calls:
Configures Multer to accept a single image file from a form field called image. Multer is a popular Express middleware package for handling file uploads. The Multer handler consumes the file multipart upload from the browser and holds the file data in memory.
Consumes the file processed by Multer. This handler is where you will write the Cloud Storage code to upload a file, make it publicly available, and store the public URL for the object.
Consumes the public URL to store the data into Cloud Datastore. You already wrote the code to store an entity in Cloud Datastore in the previous lab!
Select the .../server/gcp/cloudstorage.js file.
Note: This is the file where you will write code to save image file data into Cloud Storage.
Task 3. Creating a Cloud Storage bucket
In this section, you will create a Cloud Storage bucket and export an environment variable that references it.

Create a Cloud Storage bucket
Return to Cloud Shell, click Open Terminal and press Ctrl+C to stop the application.
To create a Cloud Storage bucket named <Project ID>-media, execute the following command:
gcloud storage buckets create gs://$DEVSHELL_PROJECT_ID-media
Copied!
Note: You can create a bucket using the gcloud storage buckets create command, passing through the name of the bucket as gs://BUCKET_NAME.
You can use $DEVSHELL_PROJECT_ID as the bucket name prefix followed by -media.
To export the Cloud Storage bucket name as an environment variable named GCLOUD_BUCKET, execute the following command:
export GCLOUD_BUCKET=$DEVSHELL_PROJECT_ID-media
Copied!
Note: Recall that the application makes use of environment variables for configuration.
This allows the development team to deploy the application into development, test, staging, and production just by changing these variables.
Click Check my progress to verify the objective.
Create a Cloud Storage bucket

Task 4. Adding objects to Cloud Storage
In this section, you will write code to save uploaded files into Cloud Storage.

Note:
Update code within the sections marked as follows:
// TODO
// END TODO
To maximize your learning, review the code, inline comments, and related API documentation.
Note: Learn more about adding objects to Cloud Storage from the Google Cloud Storage: Node.js Client documentation.
Import and use the NodeJS Cloud Storage module
In the editor, move to the top of the ...server/gcp/cloudstorage.js file.
Load the '@google-cloud/storage module, and assign it to a constant named Storage:
// TODO: Load the module for Cloud Storage
const {Storage} = require('@google-cloud/storage');
// END TODO
Copied!
Use the Storage(...) factory to construct a Cloud Storage client named storage:
// TODO: Create the storage client
// The Storage(...) factory function accepts an options
// object which is used to specify which project's Cloud
// Storage buckets should be used via the projectId
// property.
// The projectId is retrieved from the config module.
// This module retrieves the project ID from the
// GCLOUD_PROJECT environment variable.
const storage = new Storage({
    projectId: config.get('GCLOUD_PROJECT')
});
// END TODO
Copied!
Declare a string constant named GCLOUD_BUCKET, and initialize it with the name of the bucket you previously exported as an environment variable:
// TODO: Get the GCLOUD_BUCKET environment variable
// Recall that earlier you exported the bucket name into an
// environment variable.
// The config module provides access to this environment
// variable so you can use it in code
const GCLOUD_BUCKET = config.get('GCLOUD_BUCKET');
// END TODO
Copied!
Declare a constant named bucket to reference the Cloud Storage bucket:
// TODO: Get a reference to the Cloud Storage bucket
const bucket = storage.bucket(GCLOUD_BUCKET);
// END TODO
Copied!
Write code to send a file to Cloud Storage
In the sendUploadToGCS(req, res, next) handler, use the Cloud Storage client to upload a file to your Cloud Storage bucket and make it publicly available.

Get a reference to a Cloud Storage file object in the bucket:
  // TODO: Get a reference to the new object
  const file = bucket.file(oname);
  // END TODO
Copied!
Create a writeable stream to send data to the Cloud Storage object. Pass through the MIME type for the object:
  // TODO: Create a stream to write the file into
  // Cloud Storage
  // The uploaded file's MIME type can be retrieved using
  // req.file.mimetype.
  // Cloud Storage metadata can be used for many purposes,
  // including establishing the type of an object.
  const stream = file.createWriteStream({
    metadata: {
      contentType: req.file.mimetype
    }
  });
  // END TODO
Copied!
Attach an event handler to the stream to handle errors. In the error event handler, invoke the next Express middleware:
  // TODO: Attach two event handlers (1) error
  // Event handler if there's an error when uploading
  stream.on('error', err => {
    // TODO: If there's an error move to the next handler
    next(err);
    // END TODO
  });
  // END TODO
Copied!
Attach a second event handler to the stream. This handler will be invoked when the data has finished uploading. In the finish event handler, make the file public, and set a new property on the Datastore entity that references the new public URL, and invoke the next middleware handler:
  // TODO: Attach two event handlers (2) finish
  // The upload completed successfully
  stream.on('finish', () => {
    // TODO: Make the object publicly accessible
    file.makePublic().then(() => {
      // TODO: Set a new property on the file for the
      // public URL for the object
  // Cloud Storage public URLs are in the form:
  // https://storage.googleapis.com/[BUCKET]/[OBJECT]
  // Use an ECMAScript template literal (`https://...`)to
  // populate the URL with appropriate values for the bucket
  // ${GCLOUD_BUCKET} and object name ${oname}
      req.file.cloudStoragePublicUrl = `https://storage.googleapis.com/${GCLOUD_BUCKET}/${oname}`;
      // END TODO
      // TODO: Invoke the next middleware handler
      next();
      // END TODO
    });
    // END TODO
  });
  // END TODO
Copied!
At the end of the sendUploadToGCS(req, res, next) handler, to upload the file's data into Cloud Storage, call the end() method to write the file to the stream:
  // TODO: End the stream to upload the file's data
  stream.end(req.file.buffer);
  // END TODO
Copied!
cloudstorage.js
After making the above changes, your file should look like this:

'use strict';
const config = require('../config');
// TODO: Load the module for Cloud Storage
const {Storage} = require('@google-cloud/storage');
// END TODO
// TODO: Create the storage client
// The Storage(...) factory function accepts an options
// object which is used to specify which project's Cloud
// Storage buckets should be used via the projectId
// property.
// The projectId is retrieved from the config module.
// This module retrieves the project ID from the
// GCLOUD_PROJECT environment variable.
const storage = new Storage({
  projectId: config.get('GCLOUD_PROJECT')
});
// END TODO
// TODO: Get the GCLOUD_BUCKET environment variable
// Recall that earlier you exported the bucket name into an
// environment variable.
// The config module provides access to this environment
// variable so you can use it in code
const GCLOUD_BUCKET = config.get('GCLOUD_BUCKET');
// END TODO
// TODO: Get a reference to the Cloud Storage bucket
const bucket = storage.bucket(GCLOUD_BUCKET);
// END TODO
// Express middleware that will automatically pass uploads to Cloud Storage.
// req.file is processed and will have a new property:
// * ``cloudStoragePublicUrl`` the public url to the object.
// [START sendUploadToGCS]
function sendUploadToGCS(req, res, next) {
  // The existing code in the handler checks to see if there
  // is a file property on the HTTP request - if a file has
  // been uploaded, then Multer will have created this
  // property in the preceding middleware call.
  if (!req.file) {
    return next();
  }
  // In addition, a unique object name, oname,  has been
  // created based on the file's original name. It has a
  // prefix generated using the current date and time.
  // This should ensure that a new file upload won't
  // overwrite an existing object in the bucket
  const oname = Date.now() + req.file.originalname;
  // TODO: Get a reference to the new object
  const file = bucket.file(oname);
  // END TODO
  // TODO: Create a stream to write the file into
  // Cloud Storage
  // The uploaded file's MIME type can be retrieved using
  // req.file.mimetype.
  // Cloud Storage metadata can be used for many purposes,
  // including establishing the type of an object.
  const stream = file.createWriteStream({
    metadata: {
      contentType: req.file.mimetype
    }
  });
  // END TODO
  // TODO: Attach two event handlers (1) error
  // Event handler if there's an error when uploading
  stream.on('error', err => {
    // TODO: If there's an error move to the next handler
    next(err);
    // END TODO
  });
  // END TODO
  // TODO: Attach two event handlers (2) finish
  // The upload completed successfully
  stream.on('finish', () => {
    // TODO: Make the object publicly accessible
    file.makePublic().then(() => {
      // TODO: Set a new property on the file for the
      // public URL for the object
  // Cloud Storage public URLs are in the form:
  // https://storage.googleapis.com/[BUCKET]/[OBJECT]
  // Use an ECMAScript template literal (`https://...`)to
  // populate the URL with appropriate values for the bucket
  // ${GCLOUD_BUCKET} and object name ${oname}
      req.file.cloudStoragePublicUrl = `https://storage.googleapis.com/${GCLOUD_BUCKET}/${oname}`;
      // END TODO
      // TODO: Invoke the next middleware handler
      next();
      // END TODO
    });
    // END TODO
  });
  // END TODO
  // TODO: End the stream to upload the file's data
  stream.end(req.file.buffer);
  // END TODO
}
// [END sendUploadToGCS]
// Multer handles parsing multipart/form-data requests.
// This instance is configured to store images in memory.
// This makes it straightforward to upload to Cloud Storage.
// [START multer]
const Multer = require('multer');
const multer = Multer({
  storage: Multer.MemoryStorage,
  limits: {
    fileSize: 40 * 1024 * 1024 // no larger than 40mb
  }
});
// [END multer]
module.exports = {
  sendUploadToGCS,
  multer
};
Run the application and create a Cloud Storage object
Save the ...server/gcp/cloudstorage.js file, and then return to the Cloud Shell command.
Start the application by typing npm start.
Download this image file to your local machine.
In Cloud Shell, click Web preview > Preview on port 8080 to preview the Quiz application.
Click Create Question.
Complete the form with the following values, and then click Save.
Form Field

Value

Author

Your Name

Quiz

Google Cloud Platform

Title

Which product does this logo relate to?

Image

Upload the Google-Cloud-Storage-Logo.svg file you previously downloaded

Answer 1

App Engine

Answer 2

Cloud Storage (select the Answer 2 radio button!)

Answer 3

Compute Engine

Answer 4

Container Engine

Return to the Cloud Console and click on Navigation menu > Cloud Storage.
On the Cloud Storage > Buckets page, click the correct bucket (named <Project ID>-media).
Note: You should see your new object named #UniqueNumber#Google-Cloud-Storage-Logo.svg.
Click Check my progress to verify the objective.
Run the application and create a Cloud Storage object

Run the client application and test the Cloud Storage public URL
Add /api/quizzes/gcp to the end of the application's URL.
Note: You should see that JSON data has been returned to the client corresponding to the Question you added in the web application.
The imageUrl property should have a value corresponding to the object in Cloud Storage.
Return to the application home page and click Take Test.
Click GCP, and answer each question.
Note: When you get to the question you just added, you should see the image has been formatted inside the client-side web application!
Note: Review
Choose the correct statements about Cloud Storage.
Cloud Storage bucket names must be globally unique.
Cloud Storage object URIs must be globally unique.
Cloud Storage bucket names must be unique for each billing account.
Cloud Storage object paths within a bucket must be unique.
Which object and method are causing data to be sent to Cloud Storage?
Storage and bucket(...)
bucket and file(...)
file and end(...)
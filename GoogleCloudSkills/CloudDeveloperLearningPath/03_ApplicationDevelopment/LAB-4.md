# App Dev - Storing Application Data in Cloud Datastore: Node.js 

* [LAB-4](https://www.cloudskillsboost.google/course_templates/22/labs/446818)

### App Dev - Storing Application Data in Cloud Datastore: Node.js
schedule
2 hours
universal_currency_alt
5 Credits
Overview
In this lab, you review the case study application, an online Quiz. You store application data for the Quiz application in Cloud Datastore.

The Quiz application skeleton has already been written for you. You clone a repository that contains the skeleton using Google Cloud Shell, review the code using the Cloud Shell code editor, and view it using the Cloud Shell web preview feature.

Then you modify the code that stores data to use Cloud Datastore.

Objectives
In this lab, you learn how to perform the following tasks:

Harness Cloud Shell as your development environment.

Integrate Cloud Datastore into a NodeJS application.

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
Task 1. Previewing the case study application
In this section, you access Cloud Shell, clone the git repository that contains the Quiz application, and then run the application.

Clone source code in Cloud Shell
To clone the repository for the class, execute the following command in Cloud Shell:

git clone --depth=1 https://github.com/GoogleCloudPlatform/training-data-analyst
Copied!
Configure and run the case study application
Create a soft link as a shortcut to the working directory:

ln -s ~/training-data-analyst/courses/developingapps/v1.3/nodejs/datastore ~/datastore
Copied!
Navigate to the directory that contains your working files:

cd ~/datastore/start
Copied!
Export an environment variable, GCLOUD_PROJECT that references the GCP Project ID:

export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID
Copied!
Note: Project ID in Cloud Shell
While working in Cloud Shell, you have access to the Project ID in the $DEVSHELL_PROJECT_ID environment variable.
Install the application dependencies:

npm install
Copied!
Note: The installation may take a couple of minutes.
To run the application, execute the following command:

npm start
Copied!
Review the case study application
In Cloud Shell, click Web preview > Preview on port 8080 to preview the quiz application.
Note: You should see the user interface for the web application. The three main parts to the application are:
Create Question
Take Test
Leaderboard
In the navigation bar, click Create Question.
Note: You should see a simple form that contains textboxes for the question and answers with radio buttons to select the correct answer.
Quiz authors can add questions in this part of the application. This part of the application is written as a server-side web application using the popular Node.js web application framework Express.
In the navigation bar, click Take Test.
Note: The client application opens.
In the navigation bar, click GCP.
Note: You should see a sample (Dummy) question. Quiz takers can answer questions in this part of the application. This part of the application is written as a client-side web application using the popular JavaScript framework AngularJS. It receives JSON data from the server and uses JavaScript in the browser to display questions and collect answers.
To return to the server-side application, click Quite Interesting Quiz in the navigation bar.

Task 2. Examining the case study application code
In this section, you use the Cloud Shell text editor to review the case study application code.

Employ the Cloud Shell code editor
From Cloud Shell, click Open Editor.
Note: You can also launch the Cloud Shell Editor in a separate tab of your browser by clicking on open in new window icon.
Navigate to the datastore/start/server folder using the file browser panel on the left side of the editor.
Note: The application is a standard Node.js application written using the popular Express application framework.
Review the Express web application
Select the datastore/start/server/app.js file.
Note: This file contains the entrypoint for the Express application and registers the main application routes for creating questions and delivering quizzes to users.
Select the datastore/start/server/web-app folder.
Note: This folder contains the Node.js modules for the server-side web application.
Select the datastore/start/server/web-app/questions.js file.
Note: This file contains the handlers that display the form and collect form data posted by quiz authors in the web application.
In the questions.js file, find the handler that responds to HTTP POST requests for the /questions/add route.
Note: Review the source code comments for a detailed description of this handler's functionality.
Select the datastore/start/server/web-app/views folder.
Note: This folder contains templates for the web application user interface using the pug (formerly jade) layout engine.
View the datastore/start/server/web-app/views/questions/add.pug file.
Note: This file contains the pug template for the Create Question form. Notice how there is a select list to pick a quiz, textboxes where an author can enter the question and answers, and radio buttons to select the correct answer.
Select the datastore/start/server/api/index.js file.
Note: This file contains the handler that sends JSON data to students taking a test. Notice that the handlers also make use of the model object.
Select the datastore/start/server/gcp/datastore.js file.
Note: This is the file where you write Datastore code to save and load quiz questions to and from Cloud Datastore. This module will be imported into the web application and API as the model object for the web application and API
Task 3. Adding entities to Cloud Datastore
In this section, you write code to save form data in Cloud Datastore.

Note: Update code within the sections marked as follows:
# TODO
# END TODO
To maximize your learning, try to write the code without reference to the completed code block at the end of the section. In addition, review the code, inline comments, and related API documentation.
Note: Fore more information, refer to the Google Cloud Datastore: Node.js Client reference.
Create an App Engine application to provision Cloud Datastore
Return to Cloud Shell and press Ctrl+C to stop the application.

To create an App Engine application in your project, execute the following command:

gcloud app create --region "us-central"
Copied!
Note: Although you aren't using App Engine for your web application yet, Cloud Datastore requires you to create an App Engine application in your project.
Import and use the NodeJS Datastore module
Open the ...gcp/datastore.js file in the Cloud Shell code editor.

Load the config module from the parent folder.

Load the @google-cloud/datastore module.

Declare a Datastore client object named ds.

datastore.js
'use strict';
// TODO: Load the ../config module
const config = require('../config');
// END TODO
// TODO: Load the @google-cloud/datastore module
const {Datastore} = require('@google-cloud/datastore');
// END TODO
// TODO: Create a Datastore client object, ds
// The Datastore(...) factory function accepts an options
// object which is used to specify which project's  
// Datastore should be used via the projectId property.
// The projectId is retrieved from the config module. This
// module retrieves the project ID from the GCLOUD_PROJECT
// environment variable.
const ds = new Datastore({
 projectId: config.get('GCLOUD_PROJECT')
});
// END TODO
Copied!
Write code to create a Cloud Datastore entity
Declare a constant named kind, initialized with the value 'Question'.

datastore.js
// TODO: Declare a constant named kind
// The Datastore key is the equivalent of a primary key in a
// relational database.
// There are two main ways of writing a key:
// 1. Specify the kind, and let Datastore generate a unique
//    numeric id
// 2. Specify the kind and a unique string id
const kind = 'Question';
// END TODO
Copied!
In the create(...) function, remove the existing Promise.resolve({}) placeholder statement from the create(...) function.

Declare a constant called key to store the key for this entity.

Declare a constant named entity and initialize it with the key and the quiz question properties extracted from the form data.

Use the Datastore client object (ds) to save the entity by calling the save(entity) method.

datastore.js
// The create({quiz, author, title, answer1, answer2,
// answer3, answer4, correctAnswer}) function uses an
// ECMAScript 2015 destructuring assignment to extract
// properties from the form data passed to the function
function create({ quiz, author, title, answer1, answer2, answer3, answer4, correctAnswer }) {
  // TODO: Remove Placeholder statement
  // return Promise.resolve({});
  // END TODO
  // TODO: Declare the entity key,
  // with a Datastore generated id
  const key = ds.key(kind);
  // END TODO
  // TODO: Declare the entity object, with the key and data
  const entity = {
    key,
    // The entity's members are represented in a data property.
    // This is an array where each element represents one
    // member in the entity. Each element is an object with a
    // name and a value
    data: [
      { name: 'quiz', value: quiz },
      { name: 'author', value: author },
      { name: 'title', value: title },
      { name: 'answer1', value: answer1 },
      { name: 'answer2', value: answer2 },
      { name: 'answer3', value: answer3 },
      { name: 'answer4', value: answer4 },
      { name: 'correctAnswer', value: correctAnswer },
    ]
  };
  // END TODO
  // TODO: Save the entity, return a promise
  // The ds.save(...) method returns a Promise to the  
  // caller, as it runs asynchronously.
  return ds.save(entity);
  // END TODO
}
Copied!
Run the application and create a Cloud Datastore entity
Save the ...gcp/datastore.js file, and then return to the Cloud Shell command prompt.

To start the application, execute the following command:

npm start
Copied!
In Cloud Shell, click Web preview > Preview on port 8080 to preview the quiz application.
Click Create Question.
Complete the form with the following values, and then click Save.
Form Field

Value

Author

Your Name

Quiz

Google Cloud Platform

Title

Which company owns GCP?

Answer 1

Amazon

Answer 2

Google (select the Answer 2 radio button!)

Answer 3

IBM

Answer 4

Microsoft

Note: You should be returned to the home page for the application.
Go to the Cloud Console and, on the Navigation menu, click Datastore.
Note: Your new question is in the Entities list!
Note: Review
Choose the two correct options to create a key in Cloud Datastore.
You can specify the kind and let Cloud Datastore generate a unique numeric ID.
You can specify the kind and a unique string ID.
You can ask Cloud Datastore to leave the key as a null value.
You can create an entity without a key.
Which method enables you to store entities in Cloud Datastore?
Persist
Save
Click Check my progress to verify the objective.
Create an App Engine application and add entities to Cloud Datastore

Task 4. Bonus: querying Cloud Datastore
In this section, you write code to retrieve entity data from Cloud Datastore.

Write code to retrieve Cloud Datastore entities
Return to the code editor.

In the ...gcp/datastore.js file, remove the code from the list(quiz)method.

In the list(...) method, a query that retrieves Question entities for a specific quiz from Cloud Datastore.

Use the Datastore client to run the query, and assign the returned promise object to a constant.

Write a statement to return the promise.

Chain a then(...) method to the promise.

Write an arrow function in the then(...) method to retrieve the response from Cloud Datastore.

In the arrow function extract the results from the response.

Reshape the data by adding each entity ID and removing the correct answer from the data returned from Cloud Datastore.

Complete the code in the arrow function body to return a page of entities or an object that indicates that this is the last page of results.

Run the application and test the Cloud Datastore query
Save the ...gcp/datastore.js file, and then return to the Cloud Shell command.
Stop the application by pressing Ctrl+C.
Start the application.
In Cloud Shell, click Web preview > Preview on port 8080 to preview the quiz application.
Replace the querystring at the end of the application's URL with /api/quizzes/gcp.
Note: The URL will be in the form: https://8080-####-####.ql-####-####.cloudshell.dev/api/quizzes/gcp
You should see that JSON data has been returned to the client corresponding to the question you added in the web application!
Return to the application home page, and click Take Test.
Click GCP.
Note: You should see that the quiz question has been formatted inside the client-side web application!
You can find the solution to the bonus in the lab's bonus folder.
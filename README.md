# xAPI Package & LRS Communication
This repository purpose is to show how to create a [xAPI Package](https://xapi.com/download-prototypes/) and how to communicate with a [LRS](https://xapi.com/learning-record-store/) with an iframe. You should be able to do the same without an iframe.
You can use [SCORM Cloud](https://cloud.scorm.com/) to test your xAPI Package

# xAPI Package

You will have to zip 3 files ( *index.html*, *tincan.xml*, *tincan-min.js* ) to create your xAPI Package with an iframe. If you want to run your code inside the LMS you have to zip all your files ( including *tincan.xml* and *tincan-min.js* ).

This repo is based on TinCanJS by Rustici Software ( [More info](http://rusticisoftware.github.io/TinCanJS/) )

## tincan.xml

Describe your xAPI Package in this file. Specify **Name**, **Description** and **Launch** path. You can define multiple language using "lang" with ISO code ( example "fr-FR" ). Specify URL to your server with *xapi_course_file*,

## tincan-min.js

This is the library used to communicate with your LRS ( [source here](https://github.com/RusticiSoftware/TinCanJS/tree/master/build) )

## index.html ( and more )

This is your course content. In this example we decided to use an iframe to display our content. You can also include all your code inside the xAPI Package.

## xapi_course_file

Describe your course in this file and upload it to your server. Add file URL to *tincan.xml*

# LRS Communication

Use TinCanJS to communicate with your LRS. You can send multiple "information" to your LRS using Verbs, in this example we will only send "completed" without any details. [Read more here](https://xapi.com/statements-101/) 

## Get data from URL

Get parameters from URL

    var urlParams =  new  URLSearchParams(window.location.search);
Extract data

    var actor = JSON.parse(urlParams.get('actor'));
    var activity_id = urlParams.get('activity_id');
    var registration = urlParams.get('registration');
Setup communication

    var lrs =  new TinCan.LRS({endpoint: endpoint, auth: auth});
Setup Statement

    var completionStatement = new TinCan.Statement(
    {
	    actor: actor, 
	    object: {id: activity_id, objectType: "Activity"},
	    context: {registration: registration},
	    verb: {
		    id: "http://adlnet.gov/expapi/verbs/completed",
		    display: {"en-US": "completed"}
		},
		result: {
			completion: true
		}
	});
Send statement

    lrs.saveStatement(completionStatement,
    {
	    callback:  function  (error, xhr)
	    {
		    // error will be null if this succeeded
	    }
	})


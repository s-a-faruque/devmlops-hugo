+++
title = "Calling an API Using Microsoft Forms and Power Automate"
description = "Learn how to use Microsoft Forms and Power Automate to collect responses and trigger an API call."
author = "Safique"
draft = true
date = "2025-03-13"
tags = ["Microsoft Forms", "Power Automate", "API Integration", "Automation"]
categories = ["Automation", "Microsoft Power Platform"]
+++

Microsoft Forms is a simple way to collect user input, and Power Automate can process that data by triggering API calls. This guide will walk you through the process of capturing responses from a Microsoft Form and sending them to an external API.

Prerequisites

Before starting, ensure you have:
	•	A Microsoft 365 account
	•	Access to Microsoft Forms
	•	A Power Automate (Flow) license
	•	An API endpoint to call

Step 1: Create a Microsoft Form
	1.	Go to Microsoft Forms.
	2.	Click New Form and give it a meaningful name.
	3.	Add questions based on the data you need.
	4.	Click Share to get the Form link for submissions.

Step 2: Create a Power Automate Flow
	1.	Go to Power Automate.
	2.	Click Create → Automated Cloud Flow.
	3.	Select When a new response is submitted (Microsoft Forms trigger).
	4.	Click Create and select your form from the dropdown.

Step 3: Retrieve Form Responses
	1.	Add a new action Get response details (Microsoft Forms).
	2.	Select the same form and use Response ID from the previous step.

Step 4: Call an API Using HTTP Action
	1.	Click New step → HTTP (Premium connector).
	2.	Configure the HTTP request:
	•	Method: POST (or GET, PUT, etc., as required)
	•	URL: Your API endpoint
	•	Headers: Include Content-Type: application/json
	•	Body: Construct a JSON object using the form responses

{
  "name": "@{body('Get_response_details')?['name']}",
  "email": "@{body('Get_response_details')?['email']}",
  "feedback": "@{body('Get_response_details')?['feedback']}"
}

Step 5: Test and Deploy
	1.	Click Save and then Test the flow.
	2.	Submit a response in Microsoft Forms and check if the API receives the data.
	3.	Monitor the flow execution under Flow Runs.

By integrating Microsoft Forms with Power Automate, you can easily automate API calls, enabling seamless data collection and processing. This approach is useful for lead collection, feedback management, and various other business workflows.
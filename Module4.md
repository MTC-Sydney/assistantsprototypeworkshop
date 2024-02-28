![MTC Header](./media/header.jpeg)

# Creating an Azure Open AI Assistant for Data Analysis

![Hands On Logo](./media/workshop.png)

## Module 4 - Publish to Teams

### Connect Smart Assistant to Azure OpenAI Assistants via an Azure Function

#### Edit copilot topic

1.	Go to the Microsoft Copilot Studio home page.
   
   ![Copilot Topic](./media/copilot-connect.png)
  	
3.	Click the copilot you wish to edit.
   
   ![Copilot Topic](./media/copilot-connect-1.png)
  	
5.	Under the “Topics & Plugin” section click the “System” tab.
 
6.	Select the “Fallback” topic name to edit.
7.	Under the Trigger box, hit the “+” button and select “Call an action”  “Create a flow”.
 
#### Create connection to Azure OpenAI Assistants

6.	This will open a new tab for you to create a flow with a start and an end step to connect with your copilot.
 
7.	Click on the trigger step “When Power Virtual Agent calls a flow” and add two input parameters
-	UserInput
-	Context
 
8.	Click on the “+” button in the between the two steps and search for “HTTP” action
 
9.	Select the “HTTP” action and complete the following details
-	Rename step to “Azure Function”
-	URI - https://smart-assistants-chat.azure-api.net/ai/ask-question
-	Method – POST
-	Headers
o	Key - subscription-key
o	Value - <<YOUR AZURE FUNCTION KEY>>
-	Body
{
"assistants": [
"asst_18nASIotfRPVMZSQ41f5uynD"
],
"context": "",
"prompt": ""
} 

10.	Click between the “” for “context” variable and click the lightning icon.
11.	Then select the dynamic variable “Context” under “When Power Virtual Agents calls a flow” 
 
12.	Repeat step 11 for “prompt” and this time use “UserInput”.
13.	Add a new action “Parse JSON”
14.	Under the content parameter, select the lightning icon and select “Body” from the “Azure Function” section.
 
15.	Paste the following sample schema in the schema box
{
    "type": "object",
    "properties": {
        "context": {
            "type": "string"
        },
        "data": {
            "type": "object",
            "properties": {
                "responses": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "role": {
                                "type": "string"
                            },
                            "content": {
                                "type": "array",
                                "items": {
                                    "type": "string"
                                }
                            },
                            "assistant": {
                                "type": "string"
                            }
                        },
                        "required": [
                            "role",
                            "content",
                            "assistant"
                        ]
                    }
                },
                "message_id": {
                    "type": "string"
                }
            }
        }
    }
}
16.	Add another action; “Initialize variable” to capture the Assistant response.
17.	Name the variable “chat-response” and type as “String”.
 
18.	After initializing the variable, click the “+” button and add and action called “Apply to each”.
 
19.	Under the “Parameters” tab, click the lightning icon for “Select An Output From Previous Steps” and add “Body responses” from the Parse JSON section.
 
20.	Inside the 1st “Apply to each” loop hit the “+” button and add another “Apply to each” loop.
 
21.	Under the “Parameters” tab, click the lightning icon for “Select An Output From Previous Steps” and add “Body content” from the Parse JSON section.
 
22.	In the 2nd “Apply to each” loop, hit the “+” button and search for “Append to string variable” action.
  
23.	Under the “Parameters” tab Select “chat-response” and under value, click the lightning icon.
24.	Then select “Current item” under the “Apply to each” section. IMPORTANT: make sure the current item value is from the nested “Apply to each” section and not the outer “Apply to each” section.
 
25.	Click on the last step “Return value(s) to Power Virtual Agents” and add two parameters
-	AIOutput – Click on the lightning icon and select “chat response” under the “Variables” section.
 
-	AIContext – Click on the lightning icon and select “Body context” under the “Parse JSON” section.
 
26.	Click save 
 

#### Complete topic authoring



### Publish the copilot to Teams.



![Footer](./media/footer.png)

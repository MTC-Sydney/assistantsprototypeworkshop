![MTC Header](./media/header.jpeg)

# Creating an Azure Open AI Assistant for Data Analysis

![Hands On Logo](./media/workshop.png)

## Module 4 - Publish to Teams

### Connect Smart Assistant to Azure OpenAI Assistants via an Azure Function

#### Edit copilot topic

1.	Go to the Microsoft Copilot Studio home page.
   
   ![Copilot Topic](./media/copilot-connect.png)
  	
2.	Click the copilot you wish to edit.
  	
3.	Under the “Topics & Plugin” section click the “System” tab.
   
   ![Copilot Topic](./media/copilot-connect-1.png)
  	 
4.	Select the “Fallback” topic name to edit.
5.	Under the Trigger box, hit the “+” button and select “Call an action” then “Create a flow”.
    
   ![Copilot Topic](./media/copilot-connect-2.png)
  	
#### Create connection to Azure OpenAI Assistants

6.	This will open a new tab for you to create a flow with a start and an end step to connect with your copilot.
    
   ![Copilot Topic](./media/copilot-connect-3.png)
  	
7.	Click on the trigger step “When Power Virtual Agent calls a flow” and add two input parameters
-	UserInput
-	Context
    
   ![Copilot Topic](./media/copilot-connect-4.png)
  	
8.	Click on the “+” button in the between the two steps and search for “HTTP” action
    
   ![Copilot Topic](./media/copilot-connect-5.png)
  	
9.	Select the “HTTP” action and complete the following details
-	Rename step to “Azure Function”
-	URI - https://\<your-api-url-from-module3>/ai/ask-question
-	Method – POST
-	Headers:
    -	`subscription-key`: \<subscription-key-to-your-api-from-module3>
-	Body: 

```JSON
{
   "assistants": [
     "<name-of-your-assistant>"
   ],
   "context": "",
   "prompt": 
}
```
  	
10.	Click between the quotation marks (\"\") for the `context` value variable and then click the lightning icon.

11.	Then select the dynamic variable “Context” under “When Power Virtual Agents calls a flow” 
    
   ![Copilot Topic](./media/copilot-connect-7.png)
  
  	
12.	Now, click where the value goes for the `prompt` field, and then click on the **fx** icon. In the large "function" input, set the function to: 

```
string(triggerBody()?['text'])
```

![Prompt Input Function](./media/prompt-input-function.png)


13. The Azure Function HTTP Action config should look something like this: 

![Copilot Topic](./media/copilot-connect-6.png)

  > NB: This image shows how you can work with multiple assistants with an interpreter - this is an advanced integration and not required for this module. 

14.	Add a new action “Parse JSON”
15.	Under the content parameter, select the lightning icon and select “Body” from the “Azure Function” section.
    
   ![Copilot Topic](./media/copilot-connect-8.png)
  	
16.	Paste the following sample schema in the schema box
```JSON
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
```
17.	Add another action; “Initialize variable” to capture the Assistant response.
18.	Name the variable “chat-response” and type as “String”.
    
   ![Copilot Topic](./media/copilot-connect-9.png)
  	
19.	After initializing the variable, click the “+” button and add and action called “Apply to each”.
    
   ![Copilot Topic](./media/copilot-connect-10.png)
  	
20.	Under the “Parameters” tab, click the lightning icon for “Select An Output From Previous Steps” and add “Body responses” from the Parse JSON section.
    
   ![Copilot Topic](./media/copilot-connect-11.png)
  	
21.	Inside the 1st “Apply to each” loop hit the “+” button and add another “Apply to each” loop.
    
   ![Copilot Topic](./media/copilot-connect-12.png)
  	
22.	Under the “Parameters” tab, click the lightning icon for “Select An Output From Previous Steps” and add “Body content” from the Parse JSON section.
    
   ![Copilot Topic](./media/copilot-connect-13.png)
  	
23.	In the 2nd “Apply to each” loop, hit the “+” button and search for “Append to string variable” action.
     
   ![Copilot Topic](./media/copilot-connect-14.png)
  	
24.	Under the “Parameters” tab Select “chat-response” and under value, click the lightning icon.
25.	Then select “Current item” under the “Apply to each” section. IMPORTANT: make sure the current item value is from the nested “Apply to each” section and not the outer “Apply to each” section.
    
   ![Copilot Topic](./media/copilot-connect-15.png)
  	
26.	Click on the last step “Return value(s) to Power Virtual Agents” and add two parameters
-	AIOutput – Click on the lightning icon and select “chat response” under the “Variables” section.
    
   ![Copilot Topic](./media/copilot-connect-16.png)
  	
-	AIContext – Click on the lightning icon and select “Body context” under the “Parse JSON” section.
    
   ![Copilot Topic](./media/copilot-connect-17.png)
  	
27.	Click save 
    
   ![Copilot Topic](./media/copilot-connect-18.png)
  	

#### Complete topic authoring
28.	Add the following parameters to the Action in topic.
     
   ![Copilot Topic](./media/copilot-connect-19.png)
  	

29.	Complete the topic branching as follows.
    
   ![Copilot Topic](./media/copilot-connect-20.png)

   
    
   ![Copilot Topic](./media/copilot-connect-21.png)
  	



### Publish the copilot to Teams.

1.	In the left navigation pane of copilot studio, click publish.
2.	Then click the “Publish” button.
     
   ![Copilot Topic](./media/copilot-connect-22.png)
  	

3.	Expand the “Settings” section in the navigation pane and then click “Channels”.
     
   ![Copilot Topic](./media/copilot-connect-23.png)
  	

4.	Click on the Teams tile and then click “Open Bot”
    
   ![Copilot Topic](./media/copilot-connect-24.png)
  	



![Footer](./media/footer.png)

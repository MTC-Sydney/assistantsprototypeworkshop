![MTC Header](./media/header.jpeg)

# Creating an Azure Open AI Assistant for Data Analysis

![Hands On Logo](./media/workshop.png)

## Module 2 - Please complete the following steps

### Prequisites
1. Get ready to create your first Assistant
[Complete these Steps](https://learn.microsoft.com/en-us/azure/ai-services/openai/assistants-quickstart?tabs=command-line&pivots=programming-language-studio#prerequisites)

### Creating Your Assistant

1. Set the Prompt

> You help people find the best product for their needs. Ask questions to clarify in needed. Format replies as a table where there are multiple rows to consider. Get all the information for the rows and summarize the answer.

2. Load your files - one by one
3. check your assistant works by asking it to calculate something

#### Dealing with Multi-Header files

If your dataset is complex you may need to do additional steps as the underlying engine is dataframes based. 

Use the following example code to flatten your multi-header file

```
# Read the Excel file  
df = pd.read_excel('NBN Internet Pricing.xlsx', header=[0, 1, 2])  

# Flatten the column index  
df.columns = [' '.join(filter(None, col)) for col in df.columns.values]  
  
# Reset the index  
df.reset_index(drop=True, inplace=True)  
  
  
# Reset the index  
df.reset_index(drop=True, inplace=True)  
  
print(df)

# Save a DataFrame to a CSV file  
df.to_csv('NBNInternetPricingFlattened.csv', index=False)  

```

Then rename the columns, but get the assistant to write the code to rename the columns for you.

Your message to the agent could be something like 
> Rename the columns to something sensible, remove the \n and give me the python code to do the renaming

 The result could look something like

```

# Dictionary of old column names to new column names
col_name_mapping = {
    '9. NBN INTERNET PRICING Unnamed: 0_level_1 RU REFERENCE': 'RU_REFERENCE',
    '9. NBN INTERNET PRICING Unnamed: 1_level_1 SERVICE TYPE': 'SERVICE_TYPE',
    '9. NBN INTERNET PRICING Unnamed: 2_level_1 ZONE': 'ZONE',
    '9. NBN INTERNET PRICING Unnamed: 3_level_1 BANDWIDTH': 'BANDWIDTH',
    '9. NBN INTERNET PRICING Unnamed: 4_level_1 BANDWIDTH DETAILS (Symmetrical / Asymmetrical)': 'BANDWIDTH_DETAILS',
    '9. NBN INTERNET PRICING Unnamed: 5_level_1 SERVICE\nPROVIDER': 'SERVICE_PROVIDER',
    'SLA SERVICE TYPE': 'SLA_SERVICE_TYPE',
    'STANDARD SERVICE\nMONTHLY SERVICE CHARGES Zero Term\nContract Pricing Monthly $\n(Ex-GST)': 'STANDARD_ZERO_TERM_PRICE',
    'STANDARD SERVICE\nMONTHLY SERVICE CHARGES 12 Month\nContract Pricing Monthly $\n(Ex-GST)': 'STANDARD_12M_PRICE',
    'STANDARD SERVICE\nMONTHLY SERVICE CHARGES 36 Month\nContract Pricing Monthly $\n(Ex-GST)': 'STANDARD_36M_PRICE',
    'STANDARD SERVICE\nMONTHLY SERVICE CHARGES 60 Month\nContract Pricing Monthly $\n(Ex-GST)': 'STANDARD_60M_PRICE',
    'ENHANCED 12 SERVICE\nMONTHLY SERVICE CHARGES Zero Term\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_12_ZERO_TERM_PRICE',
    'ENHANCED 12 SERVICE\nMONTHLY SERVICE CHARGES 12 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_12_12M_PRICE',
    'ENHANCED 12 SERVICE\nMONTHLY SERVICE CHARGES 36 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_12_36M_PRICE',
    'ENHANCED 12 SERVICE\nMONTHLY SERVICE CHARGES 60 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_12_60M_PRICE',
    'ENHANCED 8 SERVICE\nMONTHLY SERVICE CHARGES Zero Term\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_8_ZERO_TERM_PRICE',
    'ENHANCED 8 SERVICE\nMONTHLY SERVICE CHARGES 12 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_8_12M_PRICE',
    'ENHANCED 8 SERVICE\nMONTHLY SERVICE CHARGES 36 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_8_36M_PRICE',
    'ENHANCED 8 SERVICE\nMONTHLY SERVICE CHARGES 60 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_8_60M_PRICE',
    'ENHANCED 4 SERVICE\nMONTHLY SERVICE CHARGES Zero Term\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_4_ZERO_TERM_PRICE',
    'ENHANCED 4 SERVICE\nMONTHLY SERVICE CHARGES 12 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_4_12M_PRICE',
    'ENHANCED 4 SERVICE\nMONTHLY SERVICE CHARGES 36 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_4_36M_PRICE',
    'ENHANCED 4 SERVICE\nMONTHLY SERVICE CHARGES 60 Month\nContract Pricing Monthly $\n(Ex-GST)': 'ENHANCED_4_60M_PRICE',
    'COMMENTS Additional Notes E.g. Service not provided, extra service inclusion. etc.': 'COMMENTS'
}

# Rename columns
data_renamed = df.rename(columns=col_name_mapping)

# Remove '\n' from column names
data_renamed.columns = data_renamed.columns.str.replace('\n', ' ')

# Print first few rows of renamed data
data_renamed.head()

# Save a DataFrame to a CSV file
data_renamed.to_csv('NBNInternetPricingFlattenedRenamed.csv', index=False)

```

### Testing your Assistant


1. Remove the old file. Save. 
2. Upload the new file. Save.
3. Ask the question 
> i live in the CBD and want a 100 MBit line. what are my options for 36 months?


### Now Create an Application to call the Assistant API

[Go to Module 3](./Module3.md)


add additional information to the system prompt that describes the dataset. An example is 

>The table you're describing presents a comprehensive view of NBN Internet service pricing, organized with hierarchical column headers to facilitate a detailed comparison of available plans. The primary layer of headers indicates the main categories: "NBN Internet Pricing" which is further divided into four service types - "Standard Service", "Enhanced 12 Service", "Enhanced 8 Service", "Enhanced 4 Service", along with a "Comments" section. The second layer of headers specifies four contract durations under each service type: "Zero Term Contract Pricing", "12 Month Contract Pricing", "36 Month Contract Pricing", and "60 Month Contract Pricing". The third layer details individual aspects of each plan such as "RU Reference", "Service Type", "Zone", "Bandwidth", "Bandwidth Details (Symmetrical/Asymmetrical)", "Service Provider", "SLA Service Type", and "Monthly $ (Ex-GST)" for each contract duration. An additional column for "Additional Notes" provides space for noting information like service availability or extra inclusions. This table structure allows for a systematic and organized display of data, simplifying the process of selecting the most suitable internet service plan.




![Footer](./media/footer.png)
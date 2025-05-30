# AI Augmented Predictive Modelling
I used GenAI to augment a predictive modelling workflow. Here are the steps I undertook: (sample context and prompts contained under Files)

**1. Decomposed a typical predictive modelling workflow into main tasks (or steps) and sub-tasks based on modelling best practices**
**2. Key inputs:**
  - context.txt file (sample provide) - Contains `Modelling_context`, `dataset_description`, `independent_variables` and `target_variable`.
  - Input dataset - static .csv file

**3. Created detailed prompts for each main task which included:**
  - Role assignment
  - Guidelines to adhere to the `Modelling_context`, use the `dataset_description` for additional context and identify `independent_variables` and `target_variable`
  - Sub-tasks and steps to follow under each
  - Output guidelines

For example, here is a sample prompt frame for 'Inspection and Cleaning':

        "You are a highly experienced data analyst who is inspecting and cleaning the data for further processing
        
        .......
        
        You should generate a python file for each `Task` (or step) in the process. Ensure that each generated file
        is clearly named (e.g., ic_step1.py, ic_step2.py, etc.) and output files from each step are located on the
        folder named ‘ic_step()_output’. Please include descriptive comments above each code block explaining
        what it does. After generating the code, confirm that the file (e.g., ic_step1.py) has been created.
        
        Perform these tasks on the dataset 'retail_customer_data.csv'
        
        `Task` 1: Verify Dataset Size, Structure, and Formats
        1. Perform an initial assessment of the dataset:
        - Count and report the total number of rows and columns
        - List all column names and their respective data types
        - Check for consistency in the schema (column names match expected format)
        
        2. Validate data formats and consistency:
        - Ensure datetime fields follow consistent formats (YYYY-MM-DD or as specified)
        - Verify numeric columns have appropriate data types and units
        - Normalize text fields (consistent capitalization, trimming whitespace)
        - Detect null values, empty strings, or unexpected encodings

        `Task` 2: Remove Duplicates:
        ...........
        
        For each step:
        1. Start with descriptive statistics and visualizations
        2. Document all transformations applied to the data
        3. Provide clear reasoning for subjective decisions
        4. Create a data quality report summarizing all issues found and actions taken
        5. Output both the cleaned dataset clearly named (e.g.‘dataset_cleaned.csv') and modifications made"

Thereafter our project split into two main approaches
**4. Approach 1 - Fed prompt frames into Cursor to conduct predictive analysis**
 - This approach ran very smoothly and we soon had a predictive model which gave us an accuracy score of ~78% on a sample problem, which was impressive considering the limited number of features

**5. Approach 2 - Using FastAPI, LangChain and OpenAI API, created an AI agent to automate the entire workflow:**
 - Front-end on FastAPI takes in context.txt, input .csv file and outputs .py files
 - OpenAI API - LLM backend
 - LangChain - orchestration layer, uses pre-developed prompt frames to generate code using LLM

This ran into several roadblocks as the OpenAI API would interrupt generation abruptly due to output length limits. In the future we would revise the orchestration by:
 - Breaking down components into separate prompts for each sub-task
 - Generating output .csv file after each sub-task and feeding forward into the next

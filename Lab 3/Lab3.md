# Lab 3: Build a Health Insurance Plans Analyser AI Agent

**Overview**

In this lab, you will build a Health Insurance Plans Analyser AI Agent
designed specifically to process and analyze health insurance plan data.
This intelligent agent automates the comparison of different health
benefit plans by interpreting plan details, analyzing coverage options,
and generating visual representations to support decision-making. Using
Microsoft Foundry and Azure AI services, the agent will create
comparative bar charts that clearly display differences between
insurance plans, making it easier for users to understand their options
and choose the most suitable health insurance coverage.

**Lab Objectives**

You'll perform the following task in this lab.

- Task 1: Create a Simple AI Agent

## Task 1: Create a Simple AI Agent

In this task, you will build a simple AI Agent that processes data and
generates a bar chart comparing different health benefit plans using
Azure AI services for analysis and visualization.

1.  Open the **Lab 2 - Create A Simple AI Agent.ipynb** file. This **Lab
    2 - Create A Simple AI Agent.ipynb** notebook guides you through how
    to build a simple AI Agent that processes data and generates a bar
    chart comparing different health benefit plans.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Select the **Select kernel** setting available in the top right
    corner. Select **venv (Python 3.x.x)** from the list.

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  Run the below cell to import necessary libraries and load
    environment variables for working with Azure AI Projects. This setup
    enables secure authentication and interaction with Azure AI
    services.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  Run the below cell to connect to your Microsoft Foundry project and
    access the deployed **gpt-4o** model. This establishes a secure
    connection using the project connection string and Azure
    credentials.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  Run this cell to create a **simple AI Agent** that processes data
    and generates a bar chart comparing different health benefit plans
    using Microsoft Foundry.This script initializes the AI agent, sends
    a prompt containing health plan data, and requests a bar chart. The
    agent processes the request, generates the chart, saves the image
    file, and then cleans up by deleting the agent.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  Finally observe the output.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**Summary**

In this lab, you successfully built a Health Insurance Plans Analyser AI
Agent using Microsoft Foundry and Azure AI services to automate the
analysis and comparison of health benefit plans. You learned how to
connect to your Microsoft Foundry project, access the deployed GPT-4o
model, and create an intelligent agent that processes complex health
insurance plan data. The agent interpreted plan details, analyzed
coverage options, and automatically generated comparative bar charts to
visualize differences between insurance plans. This hands-on experience
demonstrated how AI agents can streamline data analysis, support
decision-making processes, and help users easily understand and compare
their health insurance options.

You have successfully completed this lab. Kindly click Next \>\> to
proceed further.

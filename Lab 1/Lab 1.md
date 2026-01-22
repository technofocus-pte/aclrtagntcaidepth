
# Lab 1 Build and Extend Intelligent Agents

**Overview**

This hands-on lab introduces building intelligent AI agents using Azure
AI services and Microsoft 365 Copilot. Participants will learn to
leverage Copilot for HR workflows, set up Microsoft Foundry projects,
build simple AI agents, create RAG (Retrieval-Augmented Generation)
agents, and develop multi-agent systems with orchestration capabilities.

**Objectives**

By the end of this lab, you will be able to:

- **Build HR Assistant Agents with Copilot Studio** - Automate employee
  recruitment, screening, training material development, feedback
  collection, and performance reviews using Microsoft 365 Copilot.

- **Set Up AI Project and Perform Chat Completion** - Configure an AI
  Project in Microsoft Foundry, deploy Large Language Models (LLMs) and
  embedding models, and establish VS Code connectivity for chat
  completions.

- **Build a Health Insurance Plans Analyser AI Agent** - Create AI
  agents that process data and generate visualizations (e.g., bar charts
  comparing health benefit plans) using Azure AI services.

- **Develop a Health plan report generation multi-agent system** -
  Design and implement coordinated multi-agent systems where specialized
  agents (Search, Report, Validation, and Orchestrator agents) work
  together to accomplish complex tasks.

**Prerequisites**

Participants should have:

- **Visual Studio Code (VS Code)**: Proficiency in using VS Code for
  coding, debugging, and managing extensions for various programming
  languages and frameworks.

- **Development Skills**: Basic programming knowledge in Python or
  JavaScript, experience with APIs, SDKs, and working in Visual Studio
  Code.

- **Command Line/Terminal**: Familiarity with running PowerShell
  commands and managing virtual environments.

**Explanation of Components**

- **Azure AI Search**: Vector-based search service enabling RAG by
  indexing and retrieving relevant documents.

- **Azure OpenAI Service**: Provides access to GPT-4o and embedding
  models through Azure's enterprise infrastructure.

- **Large Language Models (LLMs)**: Advanced AI models like GPT-4o for
  text understanding and generation.

- **Embedding Models**: Convert text into vector representations for
  semantic search and retrieval (e.g., text-embedding-3-large).

- **Microsoft 365 Copilot**: AI-powered productivity tool for document
  analysis and workflow automation.

- **Semantic Kernel**: SDK for integrating LLMs with programming
  languages and building orchestration capabilities.

# Lab 1: Build HR Assistant Agent with Copilot Studio

Estimated Duration: 30 Minutes

Overview

In this lab, you will focus on streamlining and improving the employee
transition and onboarding process within an organization by using
Microsoft 365 Copilot and Copilot Studio. You will learn how to identify
suitable candidates, create tailored transition and onboarding plans,
generate effective communication and training materials, automate HR
workflows, collect feedback, and set up performance monitoring and
review mechanisms. By leveraging these AI-powered tools, this lab
demonstrates how organizations can ensure a smooth and efficient
transition process, enhance internal mobility, and support employees in
successfully adapting to their new roles.

Lab Objectives

You'll perform the following tasks in this lab.

- Task 1: Quickly screen candidates

- Task 2: Develop training materials

- Task 3: Collect feedback

- Task 4: Performance Reviews

Architecture Diagram

![image](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image1.png)

## Task 1: Quickly screen candidates

In this task, you will rapidly evaluate a large number of applications
for the Data Analyst position by using Microsoft 365 Copilot to analyze
resumes and filter candidates based on specific criteria such as
relevant experience, technical skills, and educational background,
allowing Copilot to highlight the top candidates for further review.

1.  Add a new tab in the Edge browser and open the Microsoft 365 Copilot
    app using the following link, and click on **Sign in** .

	+++https://m365.cloud.microsoft/+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image2.png)

2.  On the **Sign into Microsoft Azure tab**, you will see a login
    screen. Login using the below credentials.

	- Username - +++@lab.CloudPortalCredential(User1).Username+++

	- TAP - +++@lab.CloudPortalCredential(User1).AccessToken+++

3.  If you see the pop-up **Welcome to your Microsoft 365 Copilot app**,
    click **Get started**.

    ![A screenshot of a computer application AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image3.png)

4.  In the bottom left hand corner, select **Apps** , then click
    on **OneDrive** from the Apps section.

    >[!Note]: If you see the pop-up **Welcome to Apps**, click on **X** to
close pop-up.

    ![A screenshot of a computer application AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image5.png)


8.  Click on **+ Create or upload** and select **Folder
    upload**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image6.png)

9.  Navigate to C:\LabFiles\Day-1 , click on
    the data  file, and click on **Upload**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image9.png)

10. Select **Upload** on the Upload files to this site? pop-up.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image10.png)

11. Navigate back to **M365 Copilot**, Select **New Chat** on the left hand panel.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image11.png)

12. Click the **+ (Add)** icon  at the bottom of the chat pane
    and select **Upload images and files**.

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image12.png)

13. In the file explorer pop-up, navigate
    to C:\LabFiles\Day-1\data\CV  folder, select **first
    3**  files and click on **Open** .

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image13.png)

14. In the **Copilot chat**, once the **files** are uploaded
    successfully, click **enter**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image14.png)

15. In the active Copilot chat, click the **+ (Add)** icon below the
    message box, then select **Upload images and files**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image15.png)

16. In the file explorer pop-up, navigate
    to C:\LabFiles\Day-1\Data\CV  folder, select **last
    2**  files and click on **Open** .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image16.png)

17. In the **Copilot chat**, once the **files** are uploaded
    successfully click on **enter** .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image17.png)

18. In the Chat box, provide the following prompt  and hit
    the **Sent** button:

	```
    Microsoft 365 Copilot, please help me filter and shortlist resumes of
	Data Analyst candidates based on required qualifications such as
	experience in SQL, Python, and data visualization tools.
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image18.png)

19. Following up with the below prompt and hitting the **Sent** button

Create a summary report of top Data Analyst candidates, including
their skills, work experience, and educational background.

![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image19.png)

**Outcome**: The HR team efficiently identifies the most qualified
candidates, saving time and ensuring a focused recruitment effort.

## Task 2: Develop training materials.

In this task, you will prepare comprehensive training materials for the
new hire by using Microsoft Copilot to create personalized onboarding
content, including role-specific guides, company policies, and an
overview of the tools and technologies used, ensuring that the training
materials are thorough, well-structured, and tailored to the employee’s
role.

1.  In the Chat box, provide the following prompt  and hit
    the **Sent** button:

	```
    Generate a comprehensive onboarding training plan for the new Data
	Analyst, including topics like company policies, data tools training,
	and team introductions.
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image20.png)

	![A screenshot of a web page
AI-generated content may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image21.png)

2.  Following up with the below prompt  and hitting the **Sent** button.

	```
    Create an interactive training presentation covering data analysis
	best practices and key performance metrics and generate a downloadable
	PPT.
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image22.png)

    >[!Note]: After executing this prompt, you will get a PowerPoint
presentation to be downloaded, and then you can edit or design it. If
the file was not downloaded, please try to find the hyperlink with the
presentation title as shown in the screenshot.

    >[!Note]: After executing this prompt, the PowerPoint presentation to be
downloaded option is not showing up. Please rerun the above prompt.

Outcome: The new hire receives well-organized training materials,
enabling them to get up to speed and effectively perform their duties
quickly.

## Task 3: Collect feedback

In this task, you will gather feedback from new employees and
interviewers by using Microsoft Copilot to generate and distribute
feedback surveys, collect and analyze the responses, and gain insights
into the strengths of the recruitment and onboarding process as well as
areas that need improvement.

1.  In the Chat box, provide the following prompt and hit
    the **Sent** button:

	```
    Create a feedback form for interviewers to evaluate Data Analyst
	candidates based on technical skills, problem-solving abilities, and
	cultural fit Generate a downloadable Word or PDF version of this
	feedback form.
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image22.png)

2.  Following up with the below prompt and hitting the **Sent** button.

	```
    Send out a survey to new hires to gather feedback on their onboarding
	experience and identify areas for improvement Generate a downloadable
	Word or PDF version of the survey.
    ```

    ![A screenshot of a survey AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image23.png)

Outcome: The HR department gains valuable feedback, allowing them to
refine their recruitment and onboarding practices, ensuring a better
experience for future hires.

## Task 4: Performance Reviews

In this task, you will conduct regular performance reviews to assess the
new employee’s progress and development by using Microsoft Copilot to
create performance review templates, schedule review meetings, track
achievements, gather feedback from colleagues, and compile structured
performance reports.

1.  In the Chat box, provide the following prompt and hit
    the **Sent** button:

	```
    Set up a performance review schedule for the new Data Analyst, with
	quarterly reviews and goal-setting sessions and Generate a calender
	CSV file.
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image24.png)

2.  Following up with the below prompt and hitting the **Sent** button.

	```
    Generate a template for performance review reports, including sections
	for achievements, areas of improvement, and future goals Generate a
	performance review template.
    ```

    ![A screenshot of a report AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%201/media/image25.png)

Outcome: The new employee receives constructive feedback and support,
aiding their professional growth and contributing to their long-term
success within the company.

**Summary**

In this lab, you successfully built an HR Assistant Agent using
Microsoft 365 Copilot to streamline employee recruitment and
onboarding processes. You learned how to quickly screen Data Analyst
candidates by analyzing resumes and filtering based on technical
skills like SQL, Python, and data visualization, then created
comprehensive onboarding training plans and interactive presentations
for new hires. You generated feedback forms for interviewers and
surveys for new employees to assess and improve the recruitment
process, and set up quarterly performance review schedules with
structured templates to track achievements and goals. By leveraging
AI-powered tools, you demonstrated how organizations can automate HR
workflows, enhance efficiency, and ensure a smooth transition process
for new employees.

You have successfully completed this lab. Kindly click Next \\to
proceed further.

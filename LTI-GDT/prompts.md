# Prompt 1

## IN

    Prepare a prompt to perform the following tasks using Claude Code on VS Code.

    ### Task details

    You've now learned the basics of software systems analysis and design, as well as some of the most relevant diagramming tools.

    It's your turn to try out the prompts we've provided as examples so you can start gaining confidence using AI assistants in this initial phase of software development.

    In this exercise your mission will be to design and document a software system following the phases of:

    - Research and analysis  
    - Use cases  
    - Data modeling  
    - High level design

    ### And what system? The **LTI** system.

    LTI is a startup that wants to develop the **ATS (Applicant-Tracking System)** of the future.

    Nothing's built yet, so it's time to put on your **product manager** hat and define those key features that will make LTI stand out from the competition:

    - Increasing efficiency for HR departments
    - Improve real-time collaboration between recruiters and managers 
    - Automations 
    - AI assistance in various tasks 

    It's time to brainstorm, research what the keys to success might be, and write it down for the rest of the team.

    ---

    ### Your mission is to design the first version of the system, delivering the following artifacts:

    - ✅ **Brief description of LTI software**, added value and competitive advantages.  
    - ✅ **Explanation of the main functions.**
    - ✅ **Add a Lean Canvas diagram** to understand the business model.  
    - ✅ **Description of the 3 main use cases**, with the diagram associated with each one. 
    - ✅ **Data model** that covers entities, attributes (name and type) and relationships.
    - ✅ **High-level system design**, both explained and with an attached diagram.  
    - ✅ **C4 diagram** that goes in depth into one of the system components, whichever you prefer.

    ---

    ### Reference questions you could use

    - **What basic functionalities does a [Applicant-Tracking System] have?** Describe them in a list, ordered from highest to lowest priority.
    - **What benefits does the client obtain from a [Applicant-Tracking System] to consider using it?**
    - **What alternatives are there to using a [Applicant-Tracking System] and when might they be relevant?**
    - **What is the typical customer journey of a client who uses a [Applicant-Tracking System]?**
    Describe step by step all the interactions.
    - **Which open-source [Applicant-Tracking Systems] are the most well-known?**
    - **Which commercial [Applicant-Tracking Systems] are the most well-known?**  
    Compare them based on [functionalities a, b, c] and assess which would be the better option.
    - **You are an expert software analyst. I am building an Applicant-Tracking System.**  
    List and briefly describe the most important use cases that must be implemented to achieve basic functionality.
    - **Represent these use cases in the most appropriate type of diagram using mermaid format.**  
    In line with best practices, define and describe whatever is necessary.
    - **You are an expert software architect.**  
    What are the 3 essential data-model entities in an Applicant-Tracking System?  
    Give me some essential fields for each one and explain how they relate.
    - **You are a brilliant software architect.**  
    I am building an Applicant-Tracking System. I have defined the entities A, B, C, etc.  
    What other data-model entities are important in an Applicant-Tracking System?  
    Give me the most important fields of each one and explain how they relate to the other entities.

# Prompt 2


    <Inputs>
    <system_name>LTI Applicant-Tracking System (ATS)</system_name>
    </Inputs>

    <Instructions Structure>
    1. Begin by carefully reading the full contents of system_name. Understand that this is the core subject of the task and all instructions will relate to it.
    2. Use scratchpad-style reasoning to simulate the actions of a capable AI collaborator (e.g., Claude Code) in a software analysis and design phase.
    3. Follow the phases of software system design outlined: research & analysis, use cases, data modeling, high-level design.
    4. For each phase, generate output artifacts as described: descriptions, diagrams, models, and explanations.
    5. Use appropriate diagramming formats (Mermaid for use cases, C4 for system design).
    6. Reference industry knowledge and conduct simulated benchmarking/comparison as requested.
    7. Output each artifact clearly and label each section of the output with markdown headers.
    </Instructions Structure>

    <Instructions>
    You are a highly capable software analyst and architect. Your job is to help design and document the initial version of the software system described in <system_name>.

    Simulate the early stages of software development by producing all of the following artifacts, following this structured outline:

    ---

    ### 🧠 1. Research & Analysis

    **a. Briefly describe the LTI software system.**
    Explain what it is, the problem it solves, and what makes it competitive in the ATS (Applicant-Tracking System) market.

    **b. Explain the client benefits of an Applicant-Tracking System.**
    Enumerate the main value points and user pain points solved.

    **c. Describe key alternatives to an ATS.**
    Include when these alternatives might be useful instead.

    **d. List the most common functionalities of an ATS.**
    Order them by priority (most to least important), and briefly explain each.

    **e. Describe a typical client journey using an ATS.**
    Outline the key steps in the process and major touchpoints.

    **f. Provide a competitive comparison of open-source and commercial ATS tools.**
    Compare them based on selected features you choose and assess their strengths and weaknesses.

    ---

    ### 📊 2. Business Model

    **a. Generate a Lean Canvas** to understand the business model of LTI’s ATS product. Include all 9 boxes: Problem, Solution, Key Metrics, Unique Value Proposition, Channels, Customer Segments, Cost Structure, Revenue Streams, Unfair Advantage.

    ---

    ### 🔁 3. Use Cases

    **a. List the most important use cases for the ATS.**
    Summarize each with a short description.

    **b. Choose 3 main use cases and provide:**

    * A textual description of the flow
    * A diagram in Mermaid format, using best practices for visual representation

    ---

    ### 🗃️ 4. Data Modeling

    **a. Describe the 3 most important entities in the system.**
    List their essential fields (name and type) and explain their relationships.

    **b. Expand the data model with other important entities you believe should be included.**
    For each, list fields and relationships clearly.

    ---

    ### 🏗️ 5. High-Level Design

    **a. Write a high-level explanation of the system architecture.**
    Describe components, services, interactions, and any external dependencies.

    **b. Accompany your explanation with a system design diagram.**
    Use Mermaid or textual pseudodiagramming to illustrate the structure.

    ---

    ### 🧱 6. C4 Component Diagram

    **a. Choose a component of the ATS (e.g., Job Posting, Candidate Pipeline, Resume Parser, AI Assistant, etc.) and go deeper.**
    Generate a Level 3 C4 diagram (Component level).

    **b. Explain the components shown, their responsibilities, and how they interact.**
    Use best practices and diagramming clarity.

    ---

    Write each section in clear markdown. Add appropriate subheadings, lists, and diagram blocks so it is readable and logically structured.

    If a diagram is requested, format it using mermaid inside triple backticks with `mermaid` specified.

    Think carefully before you write. If you are unsure about the most useful use cases, data models, or design choices, include a brief justification for your selection before the output.

    All should be documented properly in a single markdown file.

    </Instructions>


## Prompt 3

    look at @LTI-GDT/LTI-ATS-Design.md and verify if you have done all the required tasks

    Your mission is to design the first version of the system, delivering the following artifacts:

    - ✅ **Brief description of LTI software**, added value and competitive advantages.  
    - ✅ **Explanation of the main functions.**
    - ✅ **Add a Lean Canvas diagram** to understand the business model.  
    - ✅ **Description of the 3 main use cases**, with the diagram associated with each one. 
    - ✅ **Data model** that covers entities, attributes (name and type) and relationships.
    - ✅ **High-level system design**, both explained and with an attached diagram.  
    - ✅ **C4 diagram** that goes in depth into one of the system components, whichever you prefer.

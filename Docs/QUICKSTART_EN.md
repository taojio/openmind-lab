# OpenMind Lab Quick Start Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Environment Preparation](#environment-preparation)
3. [Installation Steps](#installation-steps)
4. [First Login](#first-login)
5. [Creating Your First Project](#creating-your-first-project)
6. [Uploading Datasets](#uploading-datasets)
7. [Running Compute Tasks](#running-compute-tasks)
8. [Collaboration Features](#collaboration-features)
9. [Common Issues](#common-issues)
10. [Next Steps](#next-steps)

---

## Introduction

OpenMind Lab is an open scientific collaboration platform designed to provide researchers with powerful computing resources, data management, and collaboration tools. This guide will help you quickly get started with OpenMind Lab, from installation to creating your first project, running compute tasks, and collaborating on research.

### What is OpenMind Lab?

OpenMind Lab is a comprehensive scientific collaboration platform that integrates the following features:

- **Project Management**: Create, organize, and manage your research projects
- **Data Management**: Upload, store, and share research datasets
- **Computing Resources**: Access high-performance computing resources to run scientific computing tasks
- **Collaboration Tools**: Collaborate with team members in real-time, sharing insights and results
- **Knowledge Base**: Build and manage research knowledge bases, documenting research processes and discoveries

### Who is it for?

OpenMind Lab is suitable for the following users:

- **Researchers**: Academic researchers who need computing resources and collaboration tools
- **Data Scientists**: Data scientists who need to process large datasets and run complex models
- **Students**: Students learning and practicing scientific computing
- **Institutions**: Academic institutions that need to centrally manage research resources and projects

---

## Environment Preparation

Before you start using OpenMind Lab, you need to prepare the following environment:

### System Requirements

- **Operating System**: Windows 10 or later, macOS 10.14 or later, Linux (Ubuntu 18.04 or later)
- **Memory**: At least 8GB RAM, recommended 16GB or more
- **Storage**: At least 10GB of available space
- **Network**: Stable internet connection, recommended bandwidth 10Mbps or higher

### Software Requirements

- **Web Browser**: Latest version of Chrome, Firefox, Safari, or Edge
- **Git**: For code management and version control (optional)
- **Docker**: For containerized deployment (optional, only required for advanced users)
- **Python**: Recommended Python 3.8 or later (optional, for custom scripts)

### Account Preparation

1. **Register an Account**: Visit the [OpenMind Lab official website](https://openmind-lab.org) and register a new account
2. **Verify Email**: After registration, check your email and click the verification link
3. **Complete Profile**: After logging in, complete your profile, including name, institution, research fields, etc.

---

## Installation Steps

OpenMind Lab provides multiple installation methods. You can choose the most suitable one based on your needs.

### Method 1: Web Version (Recommended)

The web version requires no installation, just access through a browser.

1. Open your web browser
2. Visit the [OpenMind Lab Web Version](https://app.openmind-lab.org)
3. Log in with your account

### Method 2: Desktop Application

The desktop application provides better performance and offline functionality support.

#### Windows Installation

1. Visit the [OpenMind Lab Download Page](https://openmind-lab.org/download)
2. Download the Windows version installer (.exe file)
3. Double-click to run the installer
4. Follow the installation wizard to complete the installation
5. Launch the OpenMind Lab application and log in

#### macOS Installation

1. Visit the [OpenMind Lab Download Page](https://openmind-lab.org/download)
2. Download the macOS version installer (.dmg file)
3. Double-click to open the .dmg file
4. Drag OpenMind Lab to the Applications folder
5. Launch the OpenMind Lab application and log in

#### Linux Installation

1. Visit the [OpenMind Lab Download Page](https://openmind-lab.org/download)
2. Download the Linux version installer (.deb or .rpm file)
3. Choose the appropriate installation package based on your Linux distribution
4. Install using the package manager (e.g., `sudo dpkg -i openmind-lab.deb` or `sudo rpm -i openmind-lab.rpm`)
5. Launch the OpenMind Lab application and log in

### Method 3: Command Line Tool

The command line tool is suitable for users who need automation and advanced features.

#### Installing the Command Line Tool

```bash
# Install using npm (recommended)
npm install -g openmind-lab-cli

# Or install using pip
pip install openmind-lab-cli

# Or install from source
git clone https://github.com/openmind-lab/cli.git
cd cli
npm install
npm link
```

#### Configuring the Command Line Tool

```bash
# Set API key
openmind config set api_key YOUR_API_KEY

# Set default project
openmind config set default_project YOUR_PROJECT_ID

# Verify configuration
openmind config list
```

---

## First Login

After completing the installation, you need to log in to OpenMind Lab to start using it.

### Web Version Login

1. Open your browser and visit the [OpenMind Lab Web Version](https://app.openmind-lab.org)
2. Click the "Login" button in the upper right corner
3. Enter your username and password
4. Click the "Login" button

### Desktop Application Login

1. Launch the OpenMind Lab desktop application
2. Enter your username and password on the login screen
3. Click the "Login" button
4. If two-factor authentication is enabled, enter the verification code

### Command Line Tool Login

```bash
# Login with username and password
openmind login --username YOUR_USERNAME --password YOUR_PASSWORD

# Or login with API key
openmind login --api-key YOUR_API_KEY
```

### Post-Login Setup

After logging in for the first time, the system will guide you through some basic settings:

1. **Profile Setup**: Complete your profile, including avatar, bio, research fields, etc.
2. **Notification Settings**: Configure the types and methods of notifications you want to receive
3. **Privacy Settings**: Set the visibility of your projects and data
4. **Team Settings**: If you are a team administrator, you can set up team information and permissions

---

## Creating Your First Project

Projects are the basic work units in OpenMind Lab, and all research activities are conducted within projects.

### Creating a New Project

#### Creating a Project via Web Interface

1. After logging in, click "Projects" in the left navigation bar
2. Click the "New Project" button
3. Fill in the project basic information:
   - **Project Name**: Enter a descriptive project name, e.g., "Climate Data Analysis"
   - **Project Description**: Briefly describe the project goals and content
   - **Project Category**: Select a suitable project category, e.g., "Environmental Science"
   - **Tags**: Add relevant tags, e.g., "Climate", "Data Analysis", "AI"
   - **Visibility**: Set project visibility (public, internal, or private)
4. Click the "Create" button to complete the project creation

#### Creating a Project via Command Line

```bash
# Create a new project
openmind project create \
  --name "Climate Data Analysis" \
  --description "Using artificial intelligence to analyze climate data and predict climate change patterns" \
  --category "Environmental Science" \
  --tags "Climate,Data Analysis,AI" \
  --visibility "private"
```

### Project Settings

After creating a project, you can further configure project settings:

1. **Project Members**: Add team members and set permissions
2. **Project Resources**: Configure computing resources and storage space available to the project
3. **Project Integrations**: Integrate external tools and services, such as GitHub, Slack, etc.
4. **Project Templates**: Set up project templates for standardizing project structure

### Project Structure

OpenMind Lab projects have the following standard structure:

```
Project Name/
├── Datasets/          # Store project-related datasets
├── Experiments/       # Store experiments and compute tasks
├── Code/              # Store project code and scripts
├── Documentation/     # Store project documents and notes
├── Results/           # Store experiment results and outputs
└── Collaboration/     # Store collaboration content and discussions
```

---

## Uploading Datasets

Data is the foundation of scientific research, and OpenMind Lab provides convenient dataset management features.

### Preparing Datasets

Before uploading a dataset, make sure your dataset meets the following requirements:

- **File Formats**: Supports common formats such as CSV, JSON, XML, HDF5, NetCDF, etc.
- **File Size**: Single file not exceeding 10GB, total dataset size not exceeding project storage quota
- **File Naming**: Use descriptive file names, avoid special characters
- **Data Organization**: If the dataset contains multiple files, it is recommended to package them using ZIP or TAR format

### Uploading Datasets

#### Uploading Datasets via Web Interface

1. Go to your project page
2. Click the "Datasets" tab
3. Click the "Upload Dataset" button
4. Fill in the dataset information:
   - **Dataset Name**: Enter a descriptive dataset name
   - **Dataset Description**: Briefly describe the dataset content and purpose
   - **Dataset Category**: Select a suitable dataset category
   - **Tags**: Add relevant tags
   - **Visibility**: Set dataset visibility
   - **License**: Select an appropriate license
5. Click the "Choose File" button to select the file to upload
6. Click the "Upload" button to start uploading

#### Uploading Datasets via Command Line

```bash
# Upload a single file
openmind dataset upload \
  --project-id YOUR_PROJECT_ID \
  --name "Global Temperature Data" \
  --description "Historical global temperature data from 1900 to 2023" \
  --category "Climate Data" \
  --tags "Temperature,Global,Historical" \
  --file "temperature_data.csv"

# Upload multiple files
openmind dataset upload \
  --project-id YOUR_PROJECT_ID \
  --name "Climate Dataset" \
  --description "Climate data including temperature, precipitation, and wind speed" \
  --category "Climate Data" \
  --file "temperature.csv" \
  --file "precipitation.csv" \
  --file "wind_speed.csv"
```

### Dataset Management

After uploading a dataset, you can perform the following management operations:

1. **View Dataset**: View basic information and metadata of the dataset
2. **Preview Data**: Preview dataset content (supports formats like CSV, JSON, etc.)
3. **Edit Metadata**: Edit the dataset's name, description, tags, etc.
4. **Download**: Download the original dataset file
5. **Share**: Generate a share link to share the dataset with other users
6. **Delete**: Delete datasets that are no longer needed

### Dataset Version Control

OpenMind Lab supports dataset version control, you can:

1. **Create New Version**: Upload a new version of the dataset
2. **Compare Versions**: Compare differences between different versions
3. **Rollback Version**: Rollback to a previous version
4. **Tag Version**: Tag important versions (e.g., "Final", "Release", etc.)

---

## Running Compute Tasks

OpenMind Lab provides powerful computing resources, you can run various scientific computing tasks.

### Preparing Compute Tasks

Before running a compute task, you need to:

1. **Choose a Compute Engine**: Select a suitable compute engine based on your needs (such as Python, R, MATLAB, etc.)
2. **Prepare Code**: Write or upload compute code
3. **Configure Parameters**: Set parameters for the compute task
4. **Select Resources**: Select appropriate computing resources (CPU, memory, GPU, etc.)

### Creating Compute Tasks

#### Creating a Compute Task via Web Interface

1. Go to your project page
2. Click the "Experiments" tab
3. Click the "New Experiment" button
4. Fill in the experiment basic information:
   - **Experiment Name**: Enter a descriptive experiment name
   - **Experiment Description**: Briefly describe the experiment goals and content
   - **Compute Type**: Select a compute type (such as machine learning, data analysis, etc.)
   - **Compute Engine**: Select a compute engine (such as Python, R, etc.)
5. Configure the compute environment:
   - **Environment Image**: Select a pre-configured environment image or custom environment
   - **Dependencies**: Add required dependencies and libraries
   - **Resource Requirements**: Set CPU, memory, GPU, and other resource requirements
6. Upload or write code:
   - **Upload Code**: Upload existing code files
   - **Online Editor**: Use the online editor to write code
7. Set input data:
   - **Select Dataset**: Select the dataset to use
   - **Set Parameters**: Set compute parameters
8. Click the "Run" button to start the compute task

#### Creating a Compute Task via Command Line

```bash
# Create a compute task
openmind compute create \
  --project-id YOUR_PROJECT_ID \
  --name "Climate Data Analysis" \
  --description "Using machine learning to analyze climate data" \
  --compute-type "machine_learning" \
  --engine "python" \
  --image "python:3.8-tensorflow" \
  --code "analysis.py" \
  --dataset "dts_1234567890" \
  --params "{\"epochs\": 100, \"batch_size\": 32}" \
  --resources "cpu=4,memory=8Gi,gpu=1"
```

### Monitoring Compute Tasks

After running a compute task, you can monitor the task status:

1. **Task Status**: View the current status of the task (queued, running, completed, failed, etc.)
2. **Progress Information**: View task progress and estimated completion time
3. **Resource Usage**: View CPU, memory, GPU, and other resource usage
4. **Log Output**: View task running logs and output information
5. **Performance Metrics**: View task performance metrics (such as runtime, resource consumption, etc.)

#### Monitoring Tasks via Web Interface

1. Go to your project page
2. Click the "Experiments" tab
3. Click on the experiment you want to monitor
4. View task status and logs on the experiment details page

#### Monitoring Tasks via Command Line

```bash
# View task status
openmind compute status --job-id YOUR_JOB_ID

# View task logs
openmind compute logs --job-id YOUR_JOB_ID

# View task list
openmind compute list --project-id YOUR_PROJECT_ID --status running
```

### Getting Compute Results

After the compute task is completed, you can get the results:

1. **Output Files**: Download output files generated by the task
2. **Result Data**: View and analyze result data
3. **Visualization**: Use built-in tools to visualize results
4. **Report Generation**: Generate experiment reports

#### Getting Results via Web Interface

1. Go to your project page
2. Click the "Experiments" tab
3. Click on the completed experiment
4. View and download results in the "Results" tab

#### Getting Results via Command Line

```bash
# Download result files
openmind compute download --job-id YOUR_JOB_ID --output-dir ./results

# View result summary
openmind compute results --job-id YOUR_JOB_ID
```

---

## Collaboration Features

OpenMind Lab provides rich collaboration features to help team members collaborate efficiently.

### Inviting Team Members

#### Inviting Members via Web Interface

1. Go to your project page
2. Click the "Members" tab
3. Click the "Invite Members" button
4. Enter the member's username or email
5. Select the member role (Owner, Admin, Editor, Viewer)
6. Click the "Send Invitation" button

#### Inviting Members via Command Line

```bash
# Invite a member
openmind project invite \
  --project-id YOUR_PROJECT_ID \
  --username "collaborator_username" \
  --role "editor"
```

### Project Roles and Permissions

OpenMind Lab supports the following project roles:

| Role | Permissions |
|------|-------------|
| Owner | Full control over the project, including deleting the project, managing members and resources |
| Admin | Manage project settings, members, and resources, but cannot delete the project |
| Editor | Create and edit project content, upload datasets, run compute tasks |
| Viewer | View project content, but cannot make modifications |

### Real-time Collaboration

OpenMind Lab supports real-time collaboration features:

1. **Real-time Editing**: Multiple people editing documents and code simultaneously
2. **Comment System**: Comment and discuss on project content
3. **Version Control**: Track content change history
4. **Activity Stream**: View project activity history

#### Using Real-time Collaboration via Web Interface

1. Go to your project page
2. Open the document or code you want to collaborate on
3. Click the "Collaborate" button to start real-time collaboration
4. Invite other members to join the collaboration

### Discussions and Notifications

#### Creating a Discussion

1. Go to your project page
2. Click the "Discussions" tab
3. Click the "New Discussion" button
4. Fill in the discussion title and content
5. Select relevant tags
6. Click the "Post" button

#### Managing Notifications

1. Click on your profile picture in the upper right corner
2. Select "Notification Settings"
3. Configure the types and methods of notifications you want to receive
4. Click the "Save" button

---

## Common Issues

### Installation and Login Issues

**Q: What should I do if I encounter errors while installing the desktop application?**

A: Please ensure your system meets the minimum requirements and try the following solutions:
- Check if your operating system version is supported
- Ensure you have enough disk space
- Temporarily disable antivirus software and try again
- If the problem persists, please contact technical support

**Q: What should I do if I forget my password?**

A: Click the "Forgot Password" link on the login page, enter your registered email, and the system will send a password reset link to your email.

**Q: What should I do if I cannot log in to the command line tool?**

A: Please check the following:
- Ensure you have installed the command line tool correctly
- Check if your username and password are correct
- Ensure your network connection is normal
- If two-factor authentication is enabled, make sure you enter the correct verification code

### Project and Data Issues

**Q: How do I delete a project?**

A: Only project owners can delete a project. To delete a project, follow these steps:
1. Go to project settings
2. Scroll to the bottom of the page
3. Click the "Delete Project" button
4. Confirm the deletion operation

**Q: What should I do if dataset upload fails?**

A: Dataset upload failure can have various reasons:
- Check if the file size exceeds the limit
- Ensure the file format is supported
- Check if your network connection is stable
- Ensure you have enough storage space
- If the problem persists, try using the command line tool to upload

**Q: How do I share a dataset?**

A: You can share a dataset in the following ways:
1. Go to the dataset details page
2. Click the "Share" button
3. Set sharing permissions and expiration time
4. Generate a share link and send it to others

### Compute Task Issues

**Q: What should I do if a compute task fails?**

A: If a compute task fails, please perform the following operations:
1. View the task logs to understand the reason for failure
2. Check if the code has syntax errors
3. Ensure all dependencies are correctly installed
4. Check if the input data is correct
5. If the problem persists, please contact technical support

**Q: How do I optimize compute task performance?**

A: Here are some suggestions for optimizing compute task performance:
- Use more efficient algorithms and data structures
- Reduce unnecessary data transfer
- Use parallel computing
- Select appropriate computing resources
- Cache intermediate results

**Q: What should I do if a compute task runs for too long?**

A: If a compute task runs for too long, consider the following solutions:
- Increase computing resources (such as CPU, memory, GPU)
- Optimize code and algorithms
- Use data sampling to reduce data volume
- Break the task into multiple small tasks to run in parallel

### Collaboration Issues

**Q: How do I invite external partners to join a project?**

A: You can invite external partners in the following ways:
1. Add external users in project settings
2. Generate a project invitation link and send it to partners
3. Use the command line tool to invite external users

**Q: How do I handle conflicts in a project?**

A: If conflicts occur in a project, try the following solutions:
- Use version control to view change history
- Negotiate solutions through the discussion feature
- If necessary, you can roll back to a previous version
- In extreme cases, you can contact the project owner or administrator for help

---

## Next Steps

Congratulations on completing the OpenMind Lab quick start! Now you can:

### Deepen Your Learning

1. **Read Full Documentation**: Visit the [OpenMind Lab Documentation Center](https://docs.openmind-lab.org) to learn more detailed information
2. **Watch Tutorial Videos**: Watch our video tutorials to learn advanced features and best practices
3. **Attend Training Courses**: Participate in our online training courses to improve your skills

### Join the Community

1. **User Forum**: Join our user forum to exchange experiences and tips with other users
2. **Developer Community**: If you are a developer, you can join our developer community to participate in platform development
3. **Social Media**: Follow our social media accounts to get the latest updates and news

### Explore Advanced Features

1. **API Integration**: Learn how to use the OpenMind Lab API to integrate into your workflow
2. **Custom Environments**: Create and use custom computing environments
3. **Automated Workflows**: Set up automated workflows to improve work efficiency

### Provide Feedback

We highly value your feedback! Please tell us your thoughts and suggestions through the following methods:

1. **User Feedback**: Submit feedback within the application
2. **Feature Requests**: Submit new feature suggestions on our feature request page
3. **Issue Reports**: Report issues you encounter in our issue tracking system

Thank you for choosing OpenMind Lab, and we look forward to seeing the scientific achievements you create on this platform!
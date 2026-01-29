<!DOCTYPE html>
<html>
<head>
</head>
<body>
    <h1>To-Do List Web Application</h1>
    <p>This project is a simple CRUD application for a To-Do List using the MVC pattern in C# .Net core.</p>
    <h2>Table of Contents</h2>
    <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#getting-started">Getting Started</a></li>
        <li><a href="#features">Features</a></li>
        <li><a href="#usage">Usage</a></li>
        <li><a href="#contributing">Contributing</a></li>
        <li><a href="#license">License</a></li>
    </ul>
    <h2 id="prerequisites">Prerequisites</h2>
    <p>Before you begin, ensure you have met the following requirements:</p>
    <ul>
        <li>Visual Studio or a compatible code editor.</li>
        <li>.NET Core SDK installed on your machine.</li>
        <li>A relational database (SQL Server, SQLite, etc.) for data storage.</li>
    </ul>
    <h2 id="getting-started">Getting Started</h2>
    <p>To get this project up and running, follow these steps:</p>
    <ol>
        <li>Clone the repository: <code>git clone &lt;repository_url&gt;</code></li>
        <li>Open the project in Visual Studio or your preferred code editor.</li>
        <li>Configure your database connection string in the <code>appsettings.json</code> file.</li>
        <li>Create the database using Entity Framework Core migrations. Run the following commands:</li>
    </ol>
    <pre>
        <code>
dotnet ef migrations add InitialCreate
dotnet ef database update
        </code>
    </pre>
    <ol start="5">
        <li>Run the application.</li>
        <li>Start using the ToDo application.</li>
    </ol>
    <h2 id="features">Features</h2>
    <ul>
        <li><strong>View ToDo List:</strong> Users can view a list of existing ToDo items.</li>
        <li><strong>Create ToDo Items:</strong> Users can create new ToDo items.</li>
        <li><strong>View Details:</strong> Users can view details of a specific ToDo item.</li>
        <li><strong>Edit ToDo Items:</strong> Users can edit existing ToDo items.</li>
        <li><strong>Delete ToDo Items:</strong> Users can delete ToDo items after confirmation.</li>
    </ul>
    <h2 id="usage">Usage</h2>
    <p>This section provides a brief overview of the functionality of the ToDo web application:</p>
    <ul>
        <li><strong>View ToDo List (Index):</strong> When you open the application, you'll see a list of existing ToDo items. You can click on an item to view its details, edit, or delete it.</li>
        <li><strong>Create New ToDo Item (Create):</strong> Click on the "Create New" button to open a form for creating a new ToDo item. Fill out the details and click "Save" to create it.</li>
        <li><strong>View Details (Details):</strong> Click on a ToDo item in the list to view its details.</li>
        <li><strong>Edit ToDo Item (Edit):</strong> Click on the "Edit" button when viewing the details of a ToDo item to make changes to it. Save your changes by clicking "Save."</li>
        <li><strong>Delete ToDo Item (Delete):</strong> Click on the "Delete" button when viewing the details of a ToDo item. You'll be asked for confirmation before deleting the item.</li>
    </ul>
    <!-- Continue with the rest of your README content -->
    <h2 id="contributing">Contributing</h2>
    <p>If you would like to contribute to this project, please follow these steps:</p>
    <ol>
        <li>Fork the repository.</li>
        <li>Create a new branch for your feature or bug fix.</li>
        <li>Make your changes and commit them.</li>
        <li>Push your changes to your fork.</li>
        <li>Create a pull request to the original repository.</li>
    </ol>

  <p>Here is a detailed **Project Evaluation** covering your "MovieFlix" CI/CD pipeline. You can use this text for your final report, presentation, or project documentation.

---

## **1. Objectives and Methodology**

### **1.1 Objectives**

The primary objective of this project was to modernize the software delivery lifecycle for the **MovieFlix** web application. Specifically, the project aimed to:

* **Automate Deployment:** Eliminate manual file transfers and server configuration by implementing a "Zero-Touch" deployment pipeline.
* **Ensure Consistency:** Use containerization (Docker) to guarantee that the application runs exactly the same on the developer's laptop, the build server, and the production cloud environment.
* **Reduce Downtime:** Implement an automated update strategy where new features can be pushed to production in minutes without taking the system offline manually.
* **Enhance Scalability:** Establish a cloud-native foundation on AWS that can be easily scaled up in the future.

### **1.2 Methodology**

The project adopted a **DevOps** methodology, specifically focusing on **Continuous Integration (CI)** and **Continuous Deployment (CD)**.

* **Containerization First:** We wrapped the React application and its web server (Nginx) into a single, portable unit (Docker Image).
* **Pipeline as Code:** The entire build and deploy logic was written in code (`Jenkinsfile`), ensuring the process is version-controlled and reproducible.
* **Cloud-Native Deployment:** We leveraged AWS EC2 for hosting, utilizing standard Linux security practices (SSH keys, Security Groups) to manage access.

---

## **2. Tools Used and Procedure**

### **2.1 Tools Stack**

| Category | Tool Used | Purpose |
| --- | --- | --- |
| **Version Control** | **GitHub** | Stores source code and triggers the pipeline on changes. |
| **Containerization** | **Docker** | Packages the React App and Nginx into a lightweight image. |
| **Registry** | **Docker Hub** | Stores the built images (Artifacts) for public access. |
| **Automation Server** | **Jenkins** | Orchestrates the entire flow (Build, Test, Deploy). *Running on Port 8081 via Docker.* |
| **Cloud Provider** | **AWS EC2** | The production server hosting the live application. |
| **Operating System** | **Linux (Ubuntu)** | The OS for both the Jenkins container and the AWS EC2 instance. |
| **Scripting** | **Groovy & Bash** | Used for the `Jenkinsfile` pipeline logic and shell commands. |

### **2.2 Step-by-Step Procedure**

1. **Infrastructure Setup:**
* Provisioned an AWS EC2 Instance (Ubuntu).
* Configured Security Groups to allow HTTP (80), Custom Jenkins (8081), and SSH (22).
* Installed Docker on the EC2 instance to serve as the production host.


2. **Jenkins Configuration (Docker-in-Docker):**
* Deployed Jenkins using a Docker container with the Docker Socket (`/var/run/docker.sock`) mounted.
* This unique setup allowed Jenkins (inside a container) to execute Docker commands (like `build` and `push`) on the host machine.


3. **Pipeline Development (`Jenkinsfile`):**
* Created a declarative pipeline with four stages:
* **Checkout:** Pulls code from GitHub.
* **Build:** Creates the Docker image with React environment variables.
* **Push:** Authenticates with Docker Hub and uploads the image.
* **Deploy:** SSHs into the AWS EC2 instance, stops the old container, pulls the new image, and restarts the app.




4. **Security Integration:**
* Configured **Jenkins Credentials** to securely store the Docker Hub Access Token and the AWS SSH Private Key (`.pem` file).
* Implemented strict file permissions (`chmod 600`) for SSH keys during the build process to satisfy Linux security protocols.



---

## **3. Project Demo (Detailed Walkthrough)**

*(This section describes exactly what happens when you run the project live).*

### **Phase 1: The Trigger**

* **Action:** A developer makes a code change (e.g., changing the background color or adding a new movie API key) in VS Code and pushes it to GitHub.
* **System Response:** GitHub stores the change. The Jenkins server (listening for changes or triggered manually) initiates a new job ID (e.g., Build #25).

### **Phase 2: Continuous Integration (The Build)**

* **Jenkins Console:** The pipeline starts.
* **Step A:** Jenkins pulls the latest code from the `main` branch.
* **Step B:** Jenkins executes `docker build`. It compiles the React code into static HTML/CSS/JS and places it inside an Nginx container.
* **Step C:** The image is tagged (e.g., `pavankumargit/react-app:latest`) and pushed to the Docker Hub registry.
* *Verification:* We can see the new image appear in the Docker Hub dashboard with the tag "Updated a few seconds ago."



### **Phase 3: Continuous Deployment (The Release)**

* **SSH Connection:** The Jenkins pipeline executes the "Deploy to EC2" stage. It retrieves the stored SSH key and connects to the AWS server (`3.218.252.248`).
* **Container Refresh:**
1. **Stop:** The script stops the currently running `react-app` container.
2. **Remove:** The old container is deleted to free up ports.
3. **Pull:** The server downloads the brand new image from Docker Hub.
4. **Run:** A new container starts on Port 80.



### **Phase 4: Final Output**

* **User Experience:** We open a web browser and navigate to `http://3.218.252.248`.
* **Result:** The MovieFlix application loads instantly. The React frontend communicates with the TMDB API to display the latest movies. The deployment required **zero manual intervention** on the server side.</p>

<p>Here is exactly how your "MovieFlix" project works, step-by-step, using all the tools involved:

### **The Workflow in 5 Steps**

1. **Developer (VS Code)  GitHub:**
You write code on your laptop and push it to **GitHub**. This acts as the "save point" and trigger for the project.
2. **Jenkins (The Manager):**
**Jenkins** detects the change (or you click "Build"). It downloads your code from GitHub to its own workspace.
3. **Docker (The Packer):**
Jenkins commands **Docker** to take your code and "pack" it into a container image. This image contains your React app and a web server (Nginx).
4. **Docker Hub (The Storage):**
Jenkins uploads this packed image to **Docker Hub**. This is like a cloud storage for your software, making it accessible from anywhere.
5. **AWS EC2 (The Server):**
Jenkins remotely connects to your **AWS EC2** server using SSH. It tells the server to:
* Stop the old version.
* Download the new image from Docker Hub.
* Run the new website live on the internet.



### **Short Summary**

**GitHub** holds the code. **Jenkins** runs the process. **Docker** packages the app. **Docker Hub** stores the package. **AWS** runs the website for the world to see.</p>
</body>
</html>

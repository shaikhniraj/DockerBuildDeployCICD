# Student Registration System

A Flask-based web application for managing student registrations with MongoDB as the database. This project includes CRUD (Create, Read, Update, Delete) operations, comprehensive testing, Docker containerization, and CI/CD pipeline integration.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Requirements](#requirements)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Usage](#usage)
- [API Routes](#api-routes)
- [Testing](#testing)
- [Docker Deployment](#docker-deployment)
- [License](#license)

## 🎯 Project Overview

The Student Registration System is a web application that allows users to manage student information. It provides a user-friendly interface to add, view, update, and delete student records stored in a MongoDB database. The project is built with Flask, uses PyMongo for database operations, and is containerized with Docker for easy deployment.

## ✨ Features

- **Student Management**: Add, view, update, and delete student records
- **Web Interface**: Clean HTML templates with responsive design
- **MongoDB Integration**: Persistent data storage with MongoDB
- **Health Check**: API endpoint for system health monitoring
- **Testing Suite**: Comprehensive pytest tests for all CRUD operations
- **Docker Support**: Containerized application for consistent deployment
- **Environment Configuration**: Secure configuration management with `.env` files
- **Code Quality**: Integration with pylint, black, and bandit for code standards
- **SSM Deployment**: GitHub Actions deploys the Docker container to EC2 through AWS Systems Manager

## 📦 Requirements

### Python Packages

- **Flask** (3.x) - Web framework
- **Flask-PyMongo** - MongoDB integration for Flask
- **PyMongo** - Python MongoDB driver
- **python-dotenv** - Environment variable management
- **certifi** - SSL/TLS certificate bundle (cross-platform compatibility)
- **pytest** - Testing framework
- **pylint** - Code analysis
- **black** - Code formatter
- **bandit** - Security issue scanner

### System Requirements

- Python 3.11 or higher
- MongoDB (local or cloud instance via MongoDB Atlas)
- Docker (optional, for containerized deployment)
- Docker Compose (optional)

## 📁 Folder Structure

```
flask_Practice/
├── app.py                      # Main Flask application
├── test_app.py                 # Pytest test suite
├── requirements.txt            # Project dependencies
├── Dockerfile                  # Docker container configuration
├── README.md                   # Project documentation
├── LICENSE                     # License file
├── .env                        # Environment variables (not in repo)
├── .venv/                      # Virtual environment (local only)
│
└── templates/                  # HTML templates directory
    ├── base.html               # Base template (shared layout)
    ├── index.html              # Home page (student list)
    ├── add_student.html        # Form to add new student
    └── update_student.html     # Form to update student info
```

### Key Files Description

- **app.py**: Core Flask application with routes for CRUD operations
- **test_app.py**: Unit tests using pytest framework
- **requirements.txt**: List of Python package dependencies
- **Dockerfile**: Instructions for building Docker image
- **templates/**: HTML templates for web interface

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd flask_Practice
```

### 2. Create Virtual Environment

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Setup Environment Variables

Create a `.env` file in the project root directory:

```bash
MONGO_URI=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/student_db
SECRET_KEY=your-secret-key-here
```

**Example for Local MongoDB:**
```
MONGO_URI=mongodb://localhost:27017/student_db
SECRET_KEY=dev-secret-key
```

## 🔐 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `MONGO_URI` | MongoDB connection string | `mongodb+srv://user:pass@cluster.mongodb.net/student_db` |
| `SECRET_KEY` | Flask secret key for sessions | `your-random-secret-key` |

> **Note**: The `.env` file should never be committed to version control. Add it to `.gitignore`.

## 📖 Usage

### Running Locally

```bash
# Activate virtual environment (if not already activated)
.venv\Scripts\activate  # Windows
# or
source .venv/bin/activate  # macOS/Linux

# Run the Flask application
python app.py
```

The application will start at `http://localhost:5000`

### Web Interface

- **Home Page** (`/`): View all registered students
- **Add Student** (`/add`): Form to add a new student
- **Update Student** (`/update/<student_id>`): Form to update student information
- **Delete Student** (`/delete/<student_id>`): Remove a student record

## 🔌 API Routes

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/` | Display all students |
| GET | `/add` | Show add student form |
| POST | `/add` | Submit new student data |
| GET | `/update/<student_id>` | Show update form for student |
| POST | `/update/<student_id>` | Submit updated student data |
| GET | `/delete/<student_id>` | Delete a student record |
| GET/POST | `/health` | Health check endpoint |

## 🧪 Testing

Run the test suite using pytest:

```bash
# Run all tests
pytest test_app.py

# Run with verbose output
pytest test_app.py -v

# Run specific test
pytest test_app.py::test_home_page -v
```

### Test Coverage

- **test_home_page**: Verify home page loads and displays students
- **test_add_student**: Test adding new student functionality
- **test_update_student**: Test updating existing student data
- **test_delete_student**: Test deleting student records
- **test_health_route**: Verify health check endpoint

> **Note**: Tests use a separate test MongoDB database (`test_student_db`)

## 🐳 Docker Deployment

### GitHub Actions SSM Deployment

The workflow builds and pushes the image to the manually created ECR repository, then deploys it through AWS Systems Manager Session Manager. The EC2 instance must be registered as an SSM managed instance, have the SSM agent running, and have an instance role with `AmazonSSMManagedInstanceCore` and permission to pull from ECR (for example, `AmazonEC2ContainerRegistryReadOnly`).

Add these GitHub repository **secrets**:

| Secret | Purpose |
|--------|---------|
| `AWS_ACCESS_KEY_ID` | IAM user access key used by GitHub Actions |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key used by GitHub Actions |
| `EC2_INSTANCE_ID` | Target EC2 instance ID, such as `i-0123456789abcdef0` |
| `EC2_HOST` | Public hostname or IP used by the health check |
| `MONGO_URI` | MongoDB connection string passed to the container |
| `SECRET_KEY` | Flask secret key passed to the container |
| `SMTP_SERVER` | SMTP server for deployment notifications |
| `SMTP_PORT` | SMTP port for deployment notifications |
| `SMTP_USERNAME` | SMTP username for deployment notifications |
| `SMTP_PASSWORD` | SMTP password for deployment notifications |
| `SMTP_FROM_EMAIL` | Sender address for deployment notifications |
| `NOTIFICATION_EMAIL` | Recipient address for deployment notifications |

Set `ECR_REPOSITORY` in `.github/workflows/main.yaml` to the ECR repository created manually. The GitHub IAM identity needs `ecr:GetAuthorizationToken`, ECR image push permissions, and SSM `SendCommand`/`GetCommandInvocation` permissions for the target instance. SSH secrets such as `EC2_USER`, `EC2_SSH_KEY`, and `EC2_PORT` are no longer required.

#### GitHub Actions IAM user policy

Attach the following inline policy to the IAM user or role whose credentials are stored in `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Replace the account ID, region, repository name, and instance ID placeholders.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EcrLogin",
      "Effect": "Allow",
      "Action": "ecr:GetAuthorizationToken",
      "Resource": "*"
    },
    {
      "Sid": "PushToManuallyCreatedRepository",
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:CompleteLayerUpload",
        "ecr:InitiateLayerUpload",
        "ecr:PutImage",
        "ecr:UploadLayerPart"
      ],
      "Resource": "arn:aws:ecr:<AWS_REGION>:<AWS_ACCOUNT_ID>:repository/<ECR_REPOSITORY>"
    },
    {
      "Sid": "DeployThroughSsm",
      "Effect": "Allow",
      "Action": "ssm:SendCommand",
      "Resource": [
        "arn:aws:ssm:<AWS_REGION>:<AWS_ACCOUNT_ID>:document/AWS-RunShellScript",
        "arn:aws:ec2:<AWS_REGION>:<AWS_ACCOUNT_ID>:instance/<EC2_INSTANCE_ID>"
      ]
    },
    {
      "Sid": "ReadSsmCommandResult",
      "Effect": "Allow",
      "Action": [
        "ssm:GetCommandInvocation",
        "ssm:ListCommandInvocations",
        "ssm:ListCommands"
      ],
      "Resource": "*"
    }
  ]
}
```

The `ssm:SendCommand` resource format can vary by AWS account and document configuration. If AWS rejects the resource-scoped statement, use `"Resource": "*"` for that statement and restrict access with an IAM condition or a dedicated deployment role.

#### EC2 instance role policy

Attach this policy to the EC2 instance profile role. It gives the instance permission to register with Systems Manager and pull the manually created ECR repository. The instance role does not need ECR push permissions.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SystemsManagerCore",
      "Effect": "Allow",
      "Action": [
        "ssm:DescribeAssociation",
        "ssm:UpdateInstanceInformation",
        "ssmmessages:CreateControlChannel",
        "ssmmessages:CreateDataChannel",
        "ssmmessages:OpenControlChannel",
        "ssmmessages:OpenDataChannel",
        "ec2messages:AcknowledgeMessage",
        "ec2messages:DeleteMessage",
        "ec2messages:FailMessage",
        "ec2messages:GetEndpoint",
        "ec2messages:GetMessages",
        "ec2messages:SendReply"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EcrPull",
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ],
      "Resource": "arn:aws:ecr:<AWS_REGION>:<AWS_ACCOUNT_ID>:repository/<ECR_REPOSITORY>"
    },
    {
      "Sid": "EcrLogin",
      "Effect": "Allow",
      "Action": "ecr:GetAuthorizationToken",
      "Resource": "*"
    }
  ]
}
```

The EC2 role trust relationship must also allow EC2 to assume it:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

The EC2 instance also needs network access to AWS SSM and ECR endpoints, either through internet/NAT access or VPC interface endpoints. Docker must be installed and the SSM Agent must be running.

#### Manual AWS setup steps

1. **Create the ECR repository manually.** In the AWS Console, open **Amazon ECR**, choose **Private registry > Repositories**, select **Create repository**, and create the repository name used by `ECR_REPOSITORY` in the workflow. Keep the repository in the same AWS account and region as the EC2 instance.

2. **Create the GitHub Actions IAM identity.** Create an IAM user or deployment role for GitHub Actions, attach the GitHub Actions policy above, and create an access key if using an IAM user. Do not use the EC2 instance role credentials in GitHub Actions.

3. **Create the EC2 role.** Create an IAM role with **AWS service** as the trusted entity and **EC2** as the use case. Attach `AmazonSSMManagedInstanceCore` and `AmazonEC2ContainerRegistryReadOnly`, or attach the scoped EC2 instance policy shown above.

4. **Attach the role to the instance.** In the EC2 Console, select the instance, choose **Actions > Security > Modify IAM role**, and attach the role through an instance profile. The instance must be running and in the same region configured by `AWS_REGION`.

5. **Prepare the EC2 instance.** Install and start Docker, confirm that the SSM Agent is running, and ensure the instance can reach SSM and ECR endpoints. For a private subnet, configure NAT or the required VPC endpoints.

6. **Confirm SSM registration.** In **Systems Manager > Fleet Manager**, confirm that the instance appears as a managed node with status **Online**. Until it is online, GitHub Actions cannot send the deployment command.

7. **Add GitHub repository secrets.** Add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `EC2_INSTANCE_ID`, `EC2_HOST`, `MONGO_URI`, `SECRET_KEY`, `SMTP_SERVER`, `SMTP_PORT`, `SMTP_USERNAME`, `SMTP_PASSWORD`, `SMTP_FROM_EMAIL`, and `NOTIFICATION_EMAIL` under **Settings > Secrets and variables > Actions > New repository secret**.

8. **Update the workflow repository name.** Set `ECR_REPOSITORY` in `.github/workflows/main.yaml` to the manually created ECR repository name, then push the workflow to the `main` branch. A push to `main` runs the tests, builds and pushes the image, deploys it through SSM, and runs the health check.

9. **Verify permissions before the first deployment.** From a machine configured with the GitHub Actions IAM credentials, run:

  ```bash
  aws ecr describe-repositories --repository-names <ECR_REPOSITORY> --region <AWS_REGION>
  aws ssm describe-instance-information --filters Key=InstanceIds,Values=<EC2_INSTANCE_ID> --region <AWS_REGION>
  ```

  The first command confirms the repository exists. The second should return the EC2 instance with `PingStatus` set to `Online`.

### Build Docker Image

```bash
docker build -t studentregsystem:latest .
```

### Run Docker Container

```bash
# Basic run
docker run -p 5000:5000 \
  -e MONGO_URI="mongodb+srv://user:pass@cluster.mongodb.net/student_db" \
  -e SECRET_KEY="your-secret-key" \
  studentregsystem:latest

# Run in background
docker run -d -p 5000:5000 \
  -e MONGO_URI="<your-mongo-uri>" \
  -e SECRET_KEY="<your-secret>" \
  --name student-app \
  studentregsystem:latest
```

### Docker Image Details

- **Base Image**: Python 3.11-slim
- **Working Directory**: `/app`
- **Exposed Port**: 5000
- **Entry Point**: `python app.py`

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Create a feature branch
2. Make your changes
3. Run tests to ensure everything works
4. Submit a pull request

## 📞 Support

For issues or questions, please open an issue in the repository or contact the project maintainer.


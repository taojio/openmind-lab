# OpenMind Lab API Documentation

## Table of Contents
1. [API Overview](#api-overview)
2. [Authentication and Authorization](#authentication-and-authorization)
3. [User Service API](#user-service-api)
4. [Project Service API](#project-service-api)
5. [Compute Service API](#compute-service-api)
6. [Data Service API](#data-service-api)
7. [Collaboration Service API](#collaboration-service-api)
8. [Knowledge Service API](#knowledge-service-api)
9. [Error Handling](#error-handling)
10. [API Usage Examples](#api-usage-examples)
11. [API Limits and Quotas](#api-limits-and-quotas)
12. [API Version Control](#api-version-control)

---

## API Overview

OpenMind Lab provides a comprehensive set of RESTful APIs that enable developers to interact with various platform features. The API design follows REST principles, uses JSON as the data exchange format, and supports standard HTTP methods (GET, POST, PUT, DELETE, etc.).

### Basic Information

- **Base URL**: `https://api.openmind-lab.org/v1`
- **Data Format**: JSON
- **Authentication**: Bearer Token (OAuth 2.0)
- **Response Format**: JSON
- **Character Encoding**: UTF-8

### Request Structure

All API requests should follow the following basic structure:

```http
GET /v1/resource?param1=value1&param2=value2 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json
Accept: application/json
```

### Response Structure

All API responses follow the following basic structure:

```json
{
  "success": true,
  "data": {},
  "message": "Operation successful",
  "timestamp": "2023-07-20T12:34:56.789Z",
  "request_id": "req_1234567890"
}
```

---

## Authentication and Authorization

OpenMind Lab API uses the OAuth 2.0 protocol for authentication and authorization. Developers need to register an application first, obtain a client ID and client secret, and then obtain an access token through the authorization process.

### Getting Access Tokens

#### 1. Client Credentials Mode (for server-to-server communication)

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id={client_id}&client_secret={client_secret}
```

Response example:

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  "message": "Token obtained successfully"
}
```

#### 2. Authorization Code Mode (for third-party applications)

First, redirect the user to the authorization page:

```
https://api.openmind-lab.org/v1/oauth/authorize?response_type=code&client_id={client_id}&redirect_uri={redirect_uri}&scope={scope}
```

After user authorization, they will be redirected to the specified redirect_uri with an authorization code. Then use the authorization code to get an access token:

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code={authorization_code}&redirect_uri={redirect_uri}&client_id={client_id}&client_secret={client_secret}
```

### Using Access Tokens

After obtaining an access token, include it in the Authorization header of each API request:

```http
GET /v1/user/profile HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

### Token Refresh

When the access token expires, use the refresh token to get a new access token:

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token={refresh_token}&client_id={client_id}&client_secret={client_secret}
```

---

## User Service API

The User Service API provides user registration, login, profile management, and other functions.

### User Registration

```http
POST /v1/users/register HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/json

{
  "username": "example_user",
  "email": "user@example.com",
  "password": "secure_password",
  "full_name": "Example User",
  "institution": "Example University",
  "research_fields": ["Computer Science", "Artificial Intelligence"]
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_id": "usr_1234567890",
    "username": "example_user",
    "email": "user@example.com",
    "full_name": "Example User",
    "institution": "Example University",
    "research_fields": ["Computer Science", "Artificial Intelligence"],
    "created_at": "2023-07-20T12:34:56.789Z",
    "email_verified": false
  },
  "message": "User registered successfully, please check your verification email"
}
```

### User Login

```http
POST /v1/users/login HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/json

{
  "username": "example_user",
  "password": "secure_password"
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_id": "usr_1234567890",
    "username": "example_user",
    "email": "user@example.com",
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 3600
  },
  "message": "Login successful"
}
```

### Get User Information

```http
GET /v1/users/{user_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_id": "usr_1234567890",
    "username": "example_user",
    "email": "user@example.com",
    "full_name": "Example User",
    "institution": "Example University",
    "research_fields": ["Computer Science", "Artificial Intelligence"],
    "academic_title": "PhD Student",
    "orcid_id": "0000-0000-0000-0000",
    "profile_image": "https://example.com/profile.jpg",
    "bio": "I am a researcher interested in AI and machine learning.",
    "created_at": "2023-07-20T12:34:56.789Z",
    "last_login": "2023-07-25T08:15:32.456Z",
    "email_verified": true,
    "is_active": true
  },
  "message": "User information retrieved successfully"
}
```

### Update User Information

```http
PUT /v1/users/{user_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "full_name": "Updated Name",
  "institution": "New University",
  "research_fields": ["Computer Science", "Artificial Intelligence", "Data Science"],
  "academic_title": "Postdoctoral Researcher",
  "bio": "I am a researcher interested in AI, machine learning, and data science."
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_id": "usr_1234567890",
    "username": "example_user",
    "email": "user@example.com",
    "full_name": "Updated Name",
    "institution": "New University",
    "research_fields": ["Computer Science", "Artificial Intelligence", "Data Science"],
    "academic_title": "Postdoctoral Researcher",
    "orcid_id": "0000-0000-0000-0000",
    "profile_image": "https://example.com/profile.jpg",
    "bio": "I am a researcher interested in AI, machine learning, and data science.",
    "created_at": "2023-07-20T12:34:56.789Z",
    "last_login": "2023-07-25T08:15:32.456Z",
    "email_verified": true,
    "is_active": true
  },
  "message": "User information updated successfully"
}
```

---

## Project Service API

The Project Service API provides project creation, management, collaboration, and other functions.

### Create Project

```http
POST /v1/projects HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "title": "AI for Climate Change Research",
  "description": "Using artificial intelligence to analyze climate data and predict climate change patterns.",
  "category": "Environmental Science",
  "tags": ["AI", "Climate Change", "Data Analysis"],
  "visibility": "public",
  "collaborators": ["usr_2345678901", "usr_3456789012"]
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "project_id": "prj_1234567890",
    "title": "AI for Climate Change Research",
    "description": "Using artificial intelligence to analyze climate data and predict climate change patterns.",
    "category": "Environmental Science",
    "tags": ["AI", "Climate Change", "Data Analysis"],
    "visibility": "public",
    "owner_id": "usr_1234567890",
    "collaborators": [
      {
        "user_id": "usr_2345678901",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      },
      {
        "user_id": "usr_3456789012",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      }
    ],
    "created_at": "2023-07-20T12:34:56.789Z",
    "updated_at": "2023-07-20T12:34:56.789Z",
    "status": "active"
  },
  "message": "Project created successfully"
}
```

### Get Project List

```http
GET /v1/projects?category=Environmental%20Science&visibility=public&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "projects": [
      {
        "project_id": "prj_1234567890",
        "title": "AI for Climate Change Research",
        "description": "Using artificial intelligence to analyze climate data and predict climate change patterns.",
        "category": "Environmental Science",
        "tags": ["AI", "Climate Change", "Data Analysis"],
        "visibility": "public",
        "owner_id": "usr_1234567890",
        "owner_name": "Example User",
        "collaborator_count": 2,
        "created_at": "2023-07-20T12:34:56.789Z",
        "updated_at": "2023-07-20T12:34:56.789Z",
        "status": "active"
      },
      {
        "project_id": "prj_2345678901",
        "title": "Ocean Temperature Analysis",
        "description": "Analyzing ocean temperature data to understand climate change impacts.",
        "category": "Environmental Science",
        "tags": ["Oceanography", "Climate Change", "Data Analysis"],
        "visibility": "public",
        "owner_id": "usr_2345678901",
        "owner_name": "Another User",
        "collaborator_count": 3,
        "created_at": "2023-07-18T10:15:32.456Z",
        "updated_at": "2023-07-19T14:22:18.901Z",
        "status": "active"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Project list retrieved successfully"
}
```

### Get Project Details

```http
GET /v1/projects/{project_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "project_id": "prj_1234567890",
    "title": "AI for Climate Change Research",
    "description": "Using artificial intelligence to analyze climate data and predict climate change patterns.",
    "category": "Environmental Science",
    "tags": ["AI", "Climate Change", "Data Analysis"],
    "visibility": "public",
    "owner_id": "usr_1234567890",
    "owner_name": "Example User",
    "collaborators": [
      {
        "user_id": "usr_2345678901",
        "username": "collaborator1",
        "full_name": "Collaborator One",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      },
      {
        "user_id": "usr_3456789012",
        "username": "collaborator2",
        "full_name": "Collaborator Two",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      }
    ],
    "datasets": [
      {
        "dataset_id": "dts_1234567890",
        "name": "Global Temperature Data",
        "description": "Historical global temperature data from 1900 to 2023",
        "size": "2.5 GB",
        "format": "CSV",
        "uploaded_at": "2023-07-20T13:45:12.345Z"
      }
    ],
    "experiments": [
      {
        "experiment_id": "exp_1234567890",
        "name": "Temperature Prediction Model",
        "description": "Machine learning model for predicting temperature changes",
        "status": "running",
        "created_at": "2023-07-21T09:30:15.678Z",
        "updated_at": "2023-07-21T10:45:23.901Z"
      }
    ],
    "publications": [
      {
        "publication_id": "pub_1234567890",
        "title": "AI-based Climate Change Prediction",
        "journal": "Journal of Climate Science",
        "authors": ["Example User", "Collaborator One"],
        "published_date": "2023-07-15",
        "doi": "10.1234/jcs.2023.12345"
      }
    ],
    "created_at": "2023-07-20T12:34:56.789Z",
    "updated_at": "2023-07-21T10:45:23.901Z",
    "status": "active"
  },
  "message": "Project details retrieved successfully"
}
```

### Update Project

```http
PUT /v1/projects/{project_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "title": "AI for Climate Change Research and Prediction",
  "description": "Using advanced artificial intelligence techniques to analyze climate data and predict climate change patterns with higher accuracy.",
  "tags": ["AI", "Climate Change", "Data Analysis", "Prediction"],
  "visibility": "public"
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "project_id": "prj_1234567890",
    "title": "AI for Climate Change Research and Prediction",
    "description": "Using advanced artificial intelligence techniques to analyze climate data and predict climate change patterns with higher accuracy.",
    "category": "Environmental Science",
    "tags": ["AI", "Climate Change", "Data Analysis", "Prediction"],
    "visibility": "public",
    "owner_id": "usr_1234567890",
    "collaborators": [
      {
        "user_id": "usr_2345678901",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      },
      {
        "user_id": "usr_3456789012",
        "role": "contributor",
        "joined_at": "2023-07-20T12:34:56.789Z"
      }
    ],
    "created_at": "2023-07-20T12:34:56.789Z",
    "updated_at": "2023-07-22T14:30:45.678Z",
    "status": "active"
  },
  "message": "Project updated successfully"
}
```

### Add Collaborator

```http
POST /v1/projects/{project_id}/collaborators HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "user_id": "usr_4567890123",
  "role": "contributor"
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_id": "usr_4567890123",
    "username": "new_collaborator",
    "full_name": "New Collaborator",
    "role": "contributor",
    "joined_at": "2023-07-22T15:45:12.345Z"
  },
  "message": "Collaborator added successfully"
}
```

---

## Compute Service API

The Compute Service API provides scientific computing resource management, job submission, result retrieval, and other functions.

### Submit Compute Job

```http
POST /v1/compute/jobs HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "name": "Climate Data Analysis",
  "description": "Analyze climate data using machine learning algorithms",
  "project_id": "prj_1234567890",
  "compute_type": "machine_learning",
  "engine": "tensorflow",
  "parameters": {
    "model_type": "lstm",
    "epochs": 100,
    "batch_size": 32,
    "learning_rate": 0.001
  },
  "input_datasets": ["dts_1234567890"],
  "resource_requirements": {
    "cpu": 4,
    "memory": "8Gi",
    "gpu": 1,
    "storage": "100Gi"
  }
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "job_id": "job_1234567890",
    "name": "Climate Data Analysis",
    "description": "Analyze climate data using machine learning algorithms",
    "project_id": "prj_1234567890",
    "compute_type": "machine_learning",
    "engine": "tensorflow",
    "status": "pending",
    "parameters": {
      "model_type": "lstm",
      "epochs": 100,
      "batch_size": 32,
      "learning_rate": 0.001
    },
    "input_datasets": ["dts_1234567890"],
    "resource_requirements": {
      "cpu": 4,
      "memory": "8Gi",
      "gpu": 1,
      "storage": "100Gi"
    },
    "submitted_by": "usr_1234567890",
    "submitted_at": "2023-07-22T16:30:45.678Z",
    "estimated_duration": "2h 30m"
  },
  "message": "Compute job submitted successfully"
}
```

### Get Compute Job Status

```http
GET /v1/compute/jobs/{job_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "job_id": "job_1234567890",
    "name": "Climate Data Analysis",
    "description": "Analyze climate data using machine learning algorithms",
    "project_id": "prj_1234567890",
    "compute_type": "machine_learning",
    "engine": "tensorflow",
    "status": "running",
    "progress": 45,
    "parameters": {
      "model_type": "lstm",
      "epochs": 100,
      "batch_size": 32,
      "learning_rate": 0.001
    },
    "input_datasets": ["dts_1234567890"],
    "resource_requirements": {
      "cpu": 4,
      "memory": "8Gi",
      "gpu": 1,
      "storage": "100Gi"
    },
    "submitted_by": "usr_1234567890",
    "submitted_at": "2023-07-22T16:30:45.678Z",
    "started_at": "2023-07-22T16:32:10.123Z",
    "estimated_duration": "2h 30m",
    "estimated_completion": "2023-07-22T19:00:45.678Z"
  },
  "message": "Compute job status retrieved successfully"
}
```

### Get Compute Job List

```http
GET /v1/compute/jobs?project_id=prj_1234567890&status=running&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "jobs": [
      {
        "job_id": "job_1234567890",
        "name": "Climate Data Analysis",
        "description": "Analyze climate data using machine learning algorithms",
        "project_id": "prj_1234567890",
        "compute_type": "machine_learning",
        "engine": "tensorflow",
        "status": "running",
        "progress": 45,
        "submitted_by": "usr_1234567890",
        "submitted_at": "2023-07-22T16:30:45.678Z",
        "started_at": "2023-07-22T16:32:10.123Z",
        "estimated_completion": "2023-07-22T19:00:45.678Z"
      },
      {
        "job_id": "job_2345678901",
        "name": "Temperature Prediction",
        "description": "Predict temperature changes using historical data",
        "project_id": "prj_1234567890",
        "compute_type": "machine_learning",
        "engine": "pytorch",
        "status": "pending",
        "progress": 0,
        "submitted_by": "usr_1234567890",
        "submitted_at": "2023-07-22T17:15:32.456Z",
        "estimated_duration": "1h 45m"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Compute job list retrieved successfully"
}
```

### Get Compute Job Results

```http
GET /v1/compute/jobs/{job_id}/results HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "job_id": "job_1234567890",
    "status": "completed",
    "results": {
      "output_files": [
        {
          "file_id": "fil_1234567890",
          "name": "model.h5",
          "description": "Trained machine learning model",
          "size": "125.6 MB",
          "format": "HDF5",
          "download_url": "https://api.openmind-lab.org/v1/files/fil_1234567890/download"
        },
        {
          "file_id": "fil_2345678901",
          "name": "results.csv",
          "description": "Analysis results",
          "size": "2.3 MB",
          "format": "CSV",
          "download_url": "https://api.openmind-lab.org/v1/files/fil_2345678901/download"
        }
      ],
      "metrics": {
        "accuracy": 0.92,
        "precision": 0.89,
        "recall": 0.94,
        "f1_score": 0.91,
        "mse": 0.08
      },
      "logs": "Training started...\nEpoch 1/100...\nEpoch 2/100...\n...\nTraining completed."
    },
    "completed_at": "2023-07-22T19:05:23.456Z",
    "duration": "2h 34m 38s"
  },
  "message": "Compute job results retrieved successfully"
}
```

### Cancel Compute Job

```http
DELETE /v1/compute/jobs/{job_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "job_id": "job_1234567890",
    "status": "cancelled",
    "cancelled_at": "2023-07-22T17:45:12.345Z",
    "cancelled_by": "usr_1234567890"
  },
  "message": "Compute job cancelled successfully"
}
```

---

## Data Service API

The Data Service API provides dataset upload, download, management, query, and other functions.

### Upload Dataset

```http
POST /v1/datasets HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="metadata"
Content-Type: application/json

{
  "name": "Global Temperature Data",
  "description": "Historical global temperature data from 1900 to 2023",
  "category": "Climate Data",
  "tags": ["Temperature", "Global", "Historical"],
  "project_id": "prj_1234567890",
  "visibility": "project",
  "license": "CC-BY-4.0"
}
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="temperature_data.csv"
Content-Type: text/csv

(year,month,temperature)
(1900,1,13.5)
(1900,2,14.2)
...
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

Response example:

```json
{
  "success": true,
  "data": {
    "dataset_id": "dts_1234567890",
    "name": "Global Temperature Data",
    "description": "Historical global temperature data from 1900 to 2023",
    "category": "Climate Data",
    "tags": ["Temperature", "Global", "Historical"],
    "project_id": "prj_1234567890",
    "visibility": "project",
    "license": "CC-BY-4.0",
    "owner_id": "usr_1234567890",
    "file_info": {
      "file_id": "fil_1234567890",
      "name": "temperature_data.csv",
      "size": "2.5 GB",
      "format": "CSV",
      "uploaded_at": "2023-07-22T18:30:45.678Z"
    },
    "created_at": "2023-07-22T18:30:45.678Z",
    "updated_at": "2023-07-22T18:30:45.678Z"
  },
  "message": "Dataset uploaded successfully"
}
```

### Get Dataset List

```http
GET /v1/datasets?category=Climate%20Data&visibility=public&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "datasets": [
      {
        "dataset_id": "dts_1234567890",
        "name": "Global Temperature Data",
        "description": "Historical global temperature data from 1900 to 2023",
        "category": "Climate Data",
        "tags": ["Temperature", "Global", "Historical"],
        "visibility": "public",
        "owner_id": "usr_1234567890",
        "owner_name": "Example User",
        "file_info": {
          "name": "temperature_data.csv",
          "size": "2.5 GB",
          "format": "CSV"
        },
        "download_count": 125,
        "created_at": "2023-07-22T18:30:45.678Z",
        "updated_at": "2023-07-22T18:30:45.678Z"
      },
      {
        "dataset_id": "dts_2345678901",
        "name": "Ocean Temperature Data",
        "description": "Historical ocean temperature data from 1950 to 2023",
        "category": "Climate Data",
        "tags": ["Ocean", "Temperature", "Historical"],
        "visibility": "public",
        "owner_id": "usr_2345678901",
        "owner_name": "Another User",
        "file_info": {
          "name": "ocean_temperature.csv",
          "size": "1.8 GB",
          "format": "CSV"
        },
        "download_count": 98,
        "created_at": "2023-07-20T14:15:32.456Z",
        "updated_at": "2023-07-20T14:15:32.456Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Dataset list retrieved successfully"
}
```

### Get Dataset Details

```http
GET /v1/datasets/{dataset_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "dataset_id": "dts_1234567890",
    "name": "Global Temperature Data",
    "description": "Historical global temperature data from 1900 to 2023",
    "category": "Climate Data",
    "tags": ["Temperature", "Global", "Historical"],
    "project_id": "prj_1234567890",
    "visibility": "public",
    "license": "CC-BY-4.0",
    "owner_id": "usr_1234567890",
    "owner_name": "Example User",
    "file_info": {
      "file_id": "fil_1234567890",
      "name": "temperature_data.csv",
      "size": "2.5 GB",
      "format": "CSV",
      "uploaded_at": "2023-07-22T18:30:45.678Z"
    },
    "metadata": {
      "records": 1488,
      "columns": ["year", "month", "temperature"],
      "data_types": {
        "year": "integer",
        "month": "integer",
        "temperature": "float"
      },
      "date_range": {
        "start": "1900-01-01",
        "end": "2023-12-31"
      }
    },
    "download_count": 125,
    "created_at": "2023-07-22T18:30:45.678Z",
    "updated_at": "2023-07-22T18:30:45.678Z"
  },
  "message": "Dataset details retrieved successfully"
}
```

### Download Dataset

```http
GET /v1/datasets/{dataset_id}/download HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response: File stream

### Update Dataset

```http
PUT /v1/datasets/{dataset_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "name": "Global Temperature Data (1900-2023)",
  "description": "Historical global temperature data from 1900 to 2023, including monthly averages",
  "tags": ["Temperature", "Global", "Historical", "Monthly"],
  "visibility": "public"
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "dataset_id": "dts_1234567890",
    "name": "Global Temperature Data (1900-2023)",
    "description": "Historical global temperature data from 1900 to 2023, including monthly averages",
    "category": "Climate Data",
    "tags": ["Temperature", "Global", "Historical", "Monthly"],
    "project_id": "prj_1234567890",
    "visibility": "public",
    "license": "CC-BY-4.0",
    "owner_id": "usr_1234567890",
    "file_info": {
      "file_id": "fil_1234567890",
      "name": "temperature_data.csv",
      "size": "2.5 GB",
      "format": "CSV",
      "uploaded_at": "2023-07-22T18:30:45.678Z"
    },
    "created_at": "2023-07-22T18:30:45.678Z",
    "updated_at": "2023-07-22T19:15:32.456Z"
  },
  "message": "Dataset updated successfully"
}
```

### Delete Dataset

```http
DELETE /v1/datasets/{dataset_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "dataset_id": "dts_1234567890",
    "deleted_at": "2023-07-22T19:30:45.678Z",
    "deleted_by": "usr_1234567890"
  },
  "message": "Dataset deleted successfully"
}
```

---

## Collaboration Service API

The Collaboration Service API provides real-time collaboration, comments, notifications, and other functions.

### Create Comment

```http
POST /v1/collaboration/comments HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "content": "I think we should consider using a different model for this analysis. The current model seems to have high variance.",
  "target_type": "project",
  "target_id": "prj_1234567890",
  "parent_comment_id": null
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "comment_id": "cmt_1234567890",
    "content": "I think we should consider using a different model for this analysis. The current model seems to have high variance.",
    "target_type": "project",
    "target_id": "prj_1234567890",
    "parent_comment_id": null,
    "author_id": "usr_1234567890",
    "author_name": "Example User",
    "created_at": "2023-07-22T20:15:32.456Z",
    "updated_at": "2023-07-22T20:15:32.456Z",
    "replies": []
  },
  "message": "Comment created successfully"
}
```

### Get Comment List

```http
GET /v1/collaboration/comments?target_type=project&target_id=prj_1234567890&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "comments": [
      {
        "comment_id": "cmt_1234567890",
        "content": "I think we should consider using a different model for this analysis. The current model seems to have high variance.",
        "target_type": "project",
        "target_id": "prj_1234567890",
        "parent_comment_id": null,
        "author_id": "usr_1234567890",
        "author_name": "Example User",
        "created_at": "2023-07-22T20:15:32.456Z",
        "updated_at": "2023-07-22T20:15:32.456Z",
        "replies": [
          {
            "comment_id": "cmt_2345678901",
            "content": "I agree. We could try using a random forest model instead.",
            "target_type": "project",
            "target_id": "prj_1234567890",
            "parent_comment_id": "cmt_1234567890",
            "author_id": "usr_2345678901",
            "author_name": "Another User",
            "created_at": "2023-07-22T20:45:12.345Z",
            "updated_at": "2023-07-22T20:45:12.345Z"
          }
        ]
      },
      {
        "comment_id": "cmt_3456789012",
        "content": "Has anyone tried using ensemble methods for this problem?",
        "target_type": "project",
        "target_id": "prj_1234567890",
        "parent_comment_id": null,
        "author_id": "usr_3456789012",
        "author_name": "Third User",
        "created_at": "2023-07-22T19:30:45.678Z",
        "updated_at": "2023-07-22T19:30:45.678Z",
        "replies": []
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Comment list retrieved successfully"
}
```

### Create Notification

```http
POST /v1/collaboration/notifications HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "title": "Project Meeting",
  "content": "We will have a project meeting tomorrow at 3 PM to discuss the progress and next steps.",
  "type": "meeting",
  "priority": "normal",
  "recipients": ["usr_2345678901", "usr_3456789012"],
  "project_id": "prj_1234567890"
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "notification_id": "ntf_1234567890",
    "title": "Project Meeting",
    "content": "We will have a project meeting tomorrow at 3 PM to discuss the progress and next steps.",
    "type": "meeting",
    "priority": "normal",
    "sender_id": "usr_1234567890",
    "sender_name": "Example User",
    "recipients": [
      {
        "user_id": "usr_2345678901",
        "username": "collaborator1",
        "status": "unread",
        "read_at": null
      },
      {
        "user_id": "usr_3456789012",
        "username": "collaborator2",
        "status": "unread",
        "read_at": null
      }
    ],
    "project_id": "prj_1234567890",
    "created_at": "2023-07-22T21:15:32.456Z",
    "updated_at": "2023-07-22T21:15:32.456Z"
  },
  "message": "Notification created successfully"
}
```

### Get Notification List

```http
GET /v1/collaboration/notifications?type=meeting&status=unread&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "notifications": [
      {
        "notification_id": "ntf_1234567890",
        "title": "Project Meeting",
        "content": "We will have a project meeting tomorrow at 3 PM to discuss the progress and next steps.",
        "type": "meeting",
        "priority": "normal",
        "sender_id": "usr_1234567890",
        "sender_name": "Example User",
        "status": "unread",
        "project_id": "prj_1234567890",
        "created_at": "2023-07-22T21:15:32.456Z",
        "updated_at": "2023-07-22T21:15:32.456Z"
      },
      {
        "notification_id": "ntf_2345678901",
        "title": "Dataset Update",
        "content": "New climate data has been added to the project. Please check the updated dataset.",
        "type": "update",
        "priority": "low",
        "sender_id": "usr_2345678901",
        "sender_name": "Another User",
        "status": "unread",
        "project_id": "prj_1234567890",
        "created_at": "2023-07-22T18:30:45.678Z",
        "updated_at": "2023-07-22T18:30:45.678Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Notification list retrieved successfully"
}
```

### Mark Notification as Read

```http
PUT /v1/collaboration/notifications/{notification_id}/read HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "notification_id": "ntf_1234567890",
    "status": "read",
    "read_at": "2023-07-22T21:30:45.678Z"
  },
  "message": "Notification marked as read"
}
```

---

## Knowledge Service API

The Knowledge Service API provides knowledge base, paper management, literature retrieval, and other functions.

### Create Knowledge Entry

```http
POST /v1/knowledge/entries HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "title": "Machine Learning for Climate Prediction",
  "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry summarizes the latest research and methods in this field.",
  "category": "Research Summary",
  "tags": ["Machine Learning", "Climate Prediction", "AI"],
  "project_id": "prj_1234567890",
  "visibility": "project",
  "references": [
    {
      "type": "paper",
      "title": "Deep Learning for Climate Science",
      "authors": ["Author A", "Author B"],
      "year": 2022,
      "doi": "10.1234/climate.2022.12345"
    }
  ]
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "entry_id": "kne_1234567890",
    "title": "Machine Learning for Climate Prediction",
    "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry summarizes the latest research and methods in this field.",
    "category": "Research Summary",
    "tags": ["Machine Learning", "Climate Prediction", "AI"],
    "project_id": "prj_1234567890",
    "visibility": "project",
    "author_id": "usr_1234567890",
    "author_name": "Example User",
    "references": [
      {
        "type": "paper",
        "title": "Deep Learning for Climate Science",
        "authors": ["Author A", "Author B"],
        "year": 2022,
        "doi": "10.1234/climate.2022.12345"
      }
    ],
    "created_at": "2023-07-22T22:15:32.456Z",
    "updated_at": "2023-07-22T22:15:32.456Z"
  },
  "message": "Knowledge entry created successfully"
}
```

### Get Knowledge Entry List

```http
GET /v1/knowledge/entries?category=Research%20Summary&project_id=prj_1234567890&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "entries": [
      {
        "entry_id": "kne_1234567890",
        "title": "Machine Learning for Climate Prediction",
        "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry summarizes the latest research and methods in this field.",
        "category": "Research Summary",
        "tags": ["Machine Learning", "Climate Prediction", "AI"],
        "project_id": "prj_1234567890",
        "visibility": "project",
        "author_id": "usr_1234567890",
        "author_name": "Example User",
        "created_at": "2023-07-22T22:15:32.456Z",
        "updated_at": "2023-07-22T22:15:32.456Z"
      },
      {
        "entry_id": "kne_2345678901",
        "title": "Climate Data Sources and Quality",
        "content": "This entry provides an overview of various climate data sources and discusses data quality issues and solutions.",
        "category": "Research Summary",
        "tags": ["Climate Data", "Data Quality", "Sources"],
        "project_id": "prj_1234567890",
        "visibility": "project",
        "author_id": "usr_2345678901",
        "author_name": "Another User",
        "created_at": "2023-07-21T15:30:45.678Z",
        "updated_at": "2023-07-21T15:30:45.678Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 2,
      "pages": 1
    }
  },
  "message": "Knowledge entry list retrieved successfully"
}
```

### Get Knowledge Entry Details

```http
GET /v1/knowledge/entries/{entry_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "entry_id": "kne_1234567890",
    "title": "Machine Learning for Climate Prediction",
    "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry summarizes the latest research and methods in this field.",
    "category": "Research Summary",
    "tags": ["Machine Learning", "Climate Prediction", "AI"],
    "project_id": "prj_1234567890",
    "visibility": "project",
    "author_id": "usr_1234567890",
    "author_name": "Example User",
    "references": [
      {
        "type": "paper",
        "title": "Deep Learning for Climate Science",
        "authors": ["Author A", "Author B"],
        "year": 2022,
        "doi": "10.1234/climate.2022.12345"
      },
      {
        "type": "paper",
        "title": "Ensemble Methods for Climate Prediction",
        "authors": ["Author C", "Author D"],
        "year": 2021,
        "doi": "10.1234/climate.2021.67890"
      }
    ],
    "attachments": [
      {
        "file_id": "fil_1234567890",
        "name": "ml_climate_prediction.pdf",
        "description": "Research paper on machine learning for climate prediction",
        "size": "2.3 MB",
        "format": "PDF"
      }
    ],
    "created_at": "2023-07-22T22:15:32.456Z",
    "updated_at": "2023-07-22T22:15:32.456Z"
  },
  "message": "Knowledge entry details retrieved successfully"
}
```

### Update Knowledge Entry

```http
PUT /v1/knowledge/entries/{entry_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "title": "Machine Learning for Climate Prediction: A Comprehensive Review",
  "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry provides a comprehensive review of the latest research and methods in this field, including deep learning, ensemble methods, and hybrid approaches.",
  "tags": ["Machine Learning", "Climate Prediction", "AI", "Deep Learning", "Ensemble Methods"]
}
```

Response example:

```json
{
  "success": true,
  "data": {
    "entry_id": "kne_1234567890",
    "title": "Machine Learning for Climate Prediction: A Comprehensive Review",
    "content": "Machine learning techniques have shown great promise in climate prediction tasks. This entry provides a comprehensive review of the latest research and methods in this field, including deep learning, ensemble methods, and hybrid approaches.",
    "category": "Research Summary",
    "tags": ["Machine Learning", "Climate Prediction", "AI", "Deep Learning", "Ensemble Methods"],
    "project_id": "prj_1234567890",
    "visibility": "project",
    "author_id": "usr_1234567890",
    "author_name": "Example User",
    "references": [
      {
        "type": "paper",
        "title": "Deep Learning for Climate Science",
        "authors": ["Author A", "Author B"],
        "year": 2022,
        "doi": "10.1234/climate.2022.12345"
      },
      {
        "type": "paper",
        "title": "Ensemble Methods for Climate Prediction",
        "authors": ["Author C", "Author D"],
        "year": 2021,
        "doi": "10.1234/climate.2021.67890"
      }
    ],
    "attachments": [
      {
        "file_id": "fil_1234567890",
        "name": "ml_climate_prediction.pdf",
        "description": "Research paper on machine learning for climate prediction",
        "size": "2.3 MB",
        "format": "PDF"
      }
    ],
    "created_at": "2023-07-22T22:15:32.456Z",
    "updated_at": "2023-07-22T22:45:12.345Z"
  },
  "message": "Knowledge entry updated successfully"
}
```

### Delete Knowledge Entry

```http
DELETE /v1/knowledge/entries/{entry_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "entry_id": "kne_1234567890",
    "deleted_at": "2023-07-22T23:15:32.456Z",
    "deleted_by": "usr_1234567890"
  },
  "message": "Knowledge entry deleted successfully"
}
```

---

## Error Handling

OpenMind Lab API uses standard HTTP status codes to indicate the success or failure of requests. In addition, the response includes detailed error information to help developers understand and resolve issues.

### HTTP Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 | Request successful |
| 201 | Resource created successfully |
| 204 | Request successful, no content returned |
| 400 | Invalid request parameters |
| 401 | Unauthorized, authentication required |
| 403 | Forbidden, insufficient permissions |
| 404 | Resource not found |
| 422 | Request parameter validation failed |
| 429 | Request rate limit exceeded |
| 500 | Internal server error |
| 503 | Service temporarily unavailable |

### Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "Invalid request parameter",
    "details": {
      "field": "email",
      "issue": "Invalid format"
    }
  },
  "timestamp": "2023-07-20T12:34:56.789Z",
  "request_id": "req_1234567890"
}
```

### Common Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_PARAMETER | Invalid request parameter |
| MISSING_PARAMETER | Required parameter missing |
| UNAUTHORIZED | Unauthorized access |
| FORBIDDEN | Access forbidden |
| NOT_FOUND | Resource not found |
| ALREADY_EXISTS | Resource already exists |
| QUOTA_EXCEEDED | Quota exceeded |
| RATE_LIMIT_EXCEEDED | Request rate limit exceeded |
| INTERNAL_ERROR | Internal server error |
| SERVICE_UNAVAILABLE | Service temporarily unavailable |

---

## API Usage Examples

The following are example codes for calling the OpenMind Lab API using different programming languages.

### JavaScript (Node.js)

```javascript
const axios = require('axios');

// Configure API base URL
const api = axios.create({
  baseURL: 'https://api.openmind-lab.org/v1',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
});

// Get access token
async function getAccessToken(clientId, clientSecret) {
  try {
    const response = await api.post('/oauth/token', {
      grant_type: 'client_credentials',
      client_id: clientId,
      client_secret: clientSecret
    });
    
    return response.data.data.access_token;
  } catch (error) {
    console.error('Failed to get access token:', error.response.data);
    throw error;
  }
}

// Create project
async function createProject(accessToken, projectData) {
  try {
    const response = await api.post('/projects', projectData, {
      headers: {
        'Authorization': `Bearer ${accessToken}`
      }
    });
    
    return response.data.data;
  } catch (error) {
    console.error('Failed to create project:', error.response.data);
    throw error;
  }
}

// Usage example
(async () => {
  const clientId = 'your_client_id';
  const clientSecret = 'your_client_secret';
  
  try {
    // Get access token
    const accessToken = await getAccessToken(clientId, clientSecret);
    console.log('Access token:', accessToken);
    
    // Create project
    const projectData = {
      title: 'AI for Climate Change Research',
      description: 'Using artificial intelligence to analyze climate data and predict climate change patterns.',
      category: 'Environmental Science',
      tags: ['AI', 'Climate Change', 'Data Analysis'],
      visibility: 'public'
    };
    
    const project = await createProject(accessToken, projectData);
    console.log('Created project:', project);
  } catch (error) {
    console.error('Operation failed:', error.message);
  }
})();
```

### Python

```python
import requests
import json

# Configure API base URL
BASE_URL = 'https://api.openmind-lab.org/v1'

# Get access token
def get_access_token(client_id, client_secret):
    url = f'{BASE_URL}/oauth/token'
    data = {
        'grant_type': 'client_credentials',
        'client_id': client_id,
        'client_secret': client_secret
    }
    
    try:
        response = requests.post(url, data=data)
        response.raise_for_status()
        return response.json()['data']['access_token']
    except requests.exceptions.RequestException as e:
        print(f'Failed to get access token: {e}')
        raise

# Create project
def create_project(access_token, project_data):
    url = f'{BASE_URL}/projects'
    headers = {
        'Authorization': f'Bearer {access_token}',
        'Content-Type': 'application/json'
    }
    
    try:
        response = requests.post(url, headers=headers, json=project_data)
        response.raise_for_status()
        return response.json()['data']
    except requests.exceptions.RequestException as e:
        print(f'Failed to create project: {e}')
        raise

# Usage example
if __name__ == '__main__':
    client_id = 'your_client_id'
    client_secret = 'your_client_secret'
    
    try:
        # Get access token
        access_token = get_access_token(client_id, client_secret)
        print(f'Access token: {access_token}')
        
        # Create project
        project_data = {
            'title': 'AI for Climate Change Research',
            'description': 'Using artificial intelligence to analyze climate data and predict climate change patterns.',
            'category': 'Environmental Science',
            'tags': ['AI', 'Climate Change', 'Data Analysis'],
            'visibility': 'public'
        }
        
        project = create_project(access_token, project_data)
        print(f'Created project: {json.dumps(project, indent=2)}')
    except Exception as e:
        print(f'Operation failed: {e}')
```

### Java

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.http.HttpRequest.BodyPublishers;
import java.net.http.HttpResponse.BodyHandlers;
import com.fasterxml.jackson.databind.ObjectMapper;

public class OpenMindLabApiClient {
    private static final String BASE_URL = "https://api.openmind-lab.org/v1";
    private static final HttpClient client = HttpClient.newHttpClient();
    private static final ObjectMapper mapper = new ObjectMapper();
    
    // Get access token
    public static String getAccessToken(String clientId, String clientSecret) throws IOException, InterruptedException {
        String url = BASE_URL + "/oauth/token";
        String body = String.format(
            "grant_type=client_credentials&client_id=%s&client_secret=%s",
            clientId, clientSecret
        );
        
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .header("Content-Type", "application/x-www-form-urlencoded")
            .POST(BodyPublishers.ofString(body))
            .build();
        
        HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
        
        if (response.statusCode() != 200) {
            throw new RuntimeException("Failed to get access token: " + response.body());
        }
        
        JsonNode responseJson = mapper.readTree(response.body());
        return responseJson.get("data").get("access_token").asText();
    }
    
    // Create project
    public static JsonNode createProject(String accessToken, JsonNode projectData) throws IOException, InterruptedException {
        String url = BASE_URL + "/projects";
        
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(url))
            .header("Authorization", "Bearer " + accessToken)
            .header("Content-Type", "application/json")
            .POST(BodyPublishers.ofString(projectData.toString()))
            .build();
        
        HttpResponse<String> response = client.send(request, BodyHandlers.ofString());
        
        if (response.statusCode() != 201) {
            throw new RuntimeException("Failed to create project: " + response.body());
        }
        
        return mapper.readTree(response.body()).get("data");
    }
    
    // Usage example
    public static void main(String[] args) {
        String clientId = "your_client_id";
        String clientSecret = "your_client_secret";
        
        try {
            // Get access token
            String accessToken = getAccessToken(clientId, clientSecret);
            System.out.println("Access token: " + accessToken);
            
            // Create project data
            ObjectNode projectData = mapper.createObjectNode();
            projectData.put("title", "AI for Climate Change Research");
            projectData.put("description", "Using artificial intelligence to analyze climate data and predict climate change patterns.");
            projectData.put("category", "Environmental Science");
            
            ArrayNode tags = projectData.putArray("tags");
            tags.add("AI");
            tags.add("Climate Change");
            tags.add("Data Analysis");
            
            projectData.put("visibility", "public");
            
            // Create project
            JsonNode project = createProject(accessToken, projectData);
            System.out.println("Created project: " + project.toPrettyString());
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }
}
```

---

## API Limits and Quotas

To ensure system stability and fairness, the OpenMind Lab API implements the following limits and quota policies:

### Request Rate Limits

| User Type | Limit |
|-----------|-------|
| Unauthenticated Users | 60 requests/minute |
| Regular Users | 600 requests/minute |
| Advanced Users | 6000 requests/minute |
| Enterprise Users | Custom |

### Resource Quotas

| Resource Type | Regular User Quota | Advanced User Quota | Enterprise User Quota |
|---------------|-------------------|--------------------|----------------------|
| Number of Projects | 10 | 100 | Custom |
| Dataset Storage | 10 GB | 100 GB | Custom |
| Compute Jobs/Month | 50 hours | 500 hours | Custom |
| Collaborators/Project | 5 | 50 | Custom |

### Quota Query

```http
GET /v1/user/quota HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

Response example:

```json
{
  "success": true,
  "data": {
    "user_type": "Regular User",
    "rate_limit": {
      "requests_per_minute": 600,
      "current_usage": 45,
      "reset_time": "2023-07-20T13:00:00.000Z"
    },
    "resource_quota": {
      "projects": {
        "limit": 10,
        "used": 3,
        "remaining": 7
      },
      "storage": {
        "limit": "10 GB",
        "used": "2.5 GB",
        "remaining": "7.5 GB"
      },
      "compute_time": {
        "limit": "50 hours",
        "used": "12.5 hours",
        "remaining": "37.5 hours",
        "reset_date": "2023-08-01"
      },
      "collaborators": {
        "limit": 5,
        "used": 2,
        "remaining": 3
      }
    }
  },
  "message": "Quota information retrieved successfully"
}
```

---

## API Version Control

OpenMind Lab API uses version control to manage API evolution and compatibility.

### Version Format

API versions follow the semantic versioning format: `v{major}.{minor}.{patch}`

- **Major version (major)**: Incompatible API changes
- **Minor version (minor)**: Backward compatible functional changes
- **Patch version (patch)**: Backward compatible bug fixes

### Current Version

Current stable version: `v1.0.0`

### Version Strategy

- **Stable Version**: Fully supported, backward compatible, recommended for production use
- **Beta Version**: Feature complete, but may have some issues, suitable for testing and evaluation
- **Development Version**: Latest features, but unstable, only for development and testing

### Version Compatibility

- When the major version number increases, there may be incompatible changes
- When the minor version and patch version numbers increase, backward compatibility is guaranteed
- Deprecated APIs will be removed after at least 6 months

### Version Selection

Specify the API version to use in the request URL:

```http
GET /v1/projects HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

### Version Update Notifications

When a new version is released, the system will notify developers through the following methods:

1. **API Response Headers**: `API-Version: v1.1.0`
2. **Email Notifications**: Send version update notifications to registered developers
3. **Developer Portal**: Publish version update announcements on the developer portal
4. **API Documentation**: Update API documentation, marking version differences

### Version Migration Guide

When the major version number is updated, we will provide a detailed migration guide to help developers smoothly transition to the new version. The migration guide will include:

1. **Change Summary**: List all incompatible changes
2. **Migration Steps**: Provide specific migration steps and code examples
3. **Compatibility Check**: Provide tools to help check code compatibility
4. **Timeline**: Clearly define the deprecation time of the old version and the recommended use time of the new version
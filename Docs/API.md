# OpenMind Lab API 文档

## 目录
1. [API 概述](#api-概述)
2. [认证与授权](#认证与授权)
3. [用户服务 API](#用户服务-api)
4. [项目服务 API](#项目服务-api)
5. [计算服务 API](#计算服务-api)
6. [数据服务 API](#数据服务-api)
7. [协作服务 API](#协作服务-api)
8. [知识服务 API](#知识服务-api)
9. [错误处理](#错误处理)
10. [API 使用示例](#api-使用示例)
11. [API 限制与配额](#api-限制与配额)
12. [API 版本控制](#api-版本控制)

---

## API 概述

OpenMind Lab 提供了一套全面的 RESTful API，使开发者能够与平台的各种功能进行交互。API 设计遵循 REST 原则，使用 JSON 作为数据交换格式，并支持标准的 HTTP 方法（GET、POST、PUT、DELETE 等）。

### 基础信息

- **基础 URL**: `https://api.openmind-lab.org/v1`
- **数据格式**: JSON
- **认证方式**: Bearer Token (OAuth 2.0)
- **响应格式**: JSON
- **字符编码**: UTF-8

### 请求结构

所有 API 请求应遵循以下基本结构：

```http
GET /v1/resource?param1=value1&param2=value2 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
Content-Type: application/json
Accept: application/json
```

### 响应结构

所有 API 响应都遵循以下基本结构：

```json
{
  "success": true,
  "data": {},
  "message": "操作成功",
  "timestamp": "2023-07-20T12:34:56.789Z",
  "request_id": "req_1234567890"
}
```

---

## 认证与授权

OpenMind Lab API 使用 OAuth 2.0 协议进行认证和授权。开发者需要先注册应用，获取客户端 ID 和客户端密钥，然后通过授权流程获取访问令牌。

### 获取访问令牌

#### 1. 客户端凭证模式（适用于服务器到服务器的通信）

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id={client_id}&client_secret={client_secret}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  "message": "令牌获取成功"
}
```

#### 2. 授权码模式（适用于第三方应用）

首先，重定向用户到授权页面：

```
https://api.openmind-lab.org/v1/oauth/authorize?response_type=code&client_id={client_id}&redirect_uri={redirect_uri}&scope={scope}
```

用户授权后，将重定向到指定的 redirect_uri，并附带授权码。然后使用授权码获取访问令牌：

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code={authorization_code}&redirect_uri={redirect_uri}&client_id={client_id}&client_secret={client_secret}
```

### 使用访问令牌

获取访问令牌后，在每个 API 请求的 Authorization 头中包含该令牌：

```http
GET /v1/user/profile HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

### 令牌刷新

访问令牌过期后，可以使用刷新令牌获取新的访问令牌：

```http
POST /v1/oauth/token HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token={refresh_token}&client_id={client_id}&client_secret={client_secret}
```

---

## 微服务架构下的 API 设计

OpenMind Lab 采用现代化的微服务架构，这对 API 设计提出了特殊要求。本章节介绍微服务环境下的 API 设计原则、通信模式和最佳实践。

### API 设计原则

OpenMind Lab 的 API 设计遵循以下核心原则：

1. **RESTful 架构**: API 遵循 RESTful 设计原则，使用标准 HTTP 方法（GET、POST、PUT、DELETE）与资源交互。
2. **无状态**: 每个请求包含所有必要信息，保证 API 的无状态性和可扩展性。
3. **版本控制**: 实现 API 版本控制，确保系统演进时的向后兼容性。
4. **分页**: 对大型结果集实施分页，提高性能和可用性。
5. **过滤与排序**: API 支持过滤和排序参数，高效检索特定数据。
6. **速率限制**: 为保护系统资源，API 对客户端请求实施速率限制。
7. **错误处理**: 一致的错误处理机制，提供描述性错误消息和适当的 HTTP 状态码。
8. **文档化**: 为所有端点提供全面的 API 文档。
9. **安全性**: 实现认证、授权和数据加密，保护敏感信息。
10. **性能优化**: 优化 API 性能，适当使用缓存机制。
11. **服务导向**: API 设计与系统微服务架构保持一致，服务间 API 边界清晰。
12. **服务间通信**: 标准化服务间交互的通信模式，包括同步（HTTP/gRPC）和异步（消息队列）方法。

### API 网关设计

API 网关作为所有外部客户端请求的单一入口点，提供以下功能：

- **请求路由**: 将请求路由到适当的微服务
- **认证与授权**: 集中式安全管理
- **速率限制**: 保护服务免受过载
- **缓存**: 提高频繁访问数据的响应时间
- **请求/响应转换**: 适配客户端和服务协议
- **负载均衡**: 在服务实例之间分配流量
- **熔断器**: 防止微服务生态系统中的级联故障

### 服务间通信

在微服务生态系统中，服务使用以下方式通信：

1. **同步通信**: 
   - HTTP/HTTPS 用于服务间的 RESTful API 调用
   - gRPC 用于高性能、低延迟通信
   - GraphQL 用于灵活的数据查询

2. **异步通信**: 
   - 消息队列（Kafka/RabbitMQ）用于事件驱动的交互
   - 发布/订阅模式用于广播通知
   - 事件溯源用于捕获和重放领域事件

### API 文档标准

OpenMind Lab 中的所有 API 遵循严格的文档标准，以确保一致性和易用性：

- **OpenAPI 规范**: 使用 OpenAPI 3.0 规范记录 API
- **示例**: 全面的请求和响应示例
- **错误代码**: 详细的错误代码描述和故障排除信息
- **版本历史**: API 版本变更的文档记录
- **交互式测试**: 直接从文档进行 API 测试的内置功能

有关微服务架构和通信模式的更多详细信息，请参阅 [微服务架构设计文档](architecture/MICROSERVICE_ARCHITECTURE.md)。

---

## 用户服务 API

用户服务 API 提供用户注册、登录、个人信息管理等功能。

### 用户注册

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

响应示例：

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
  "message": "用户注册成功，请查收验证邮件"
}
```

### 用户登录

```http
POST /v1/users/login HTTP/1.1
Host: api.openmind-lab.org
Content-Type: application/json

{
  "username": "example_user",
  "password": "secure_password"
}
```

响应示例：

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
  "message": "登录成功"
}
```

### 获取用户信息

```http
GET /v1/users/{user_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取用户信息成功"
}
```

### 更新用户信息

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

响应示例：

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
  "message": "用户信息更新成功"
}
```

---

## 项目服务 API

项目服务 API 提供项目创建、管理、协作等功能。

### 创建项目

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

响应示例：

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
  "message": "项目创建成功"
}
```

### 获取项目列表

```http
GET /v1/projects?category=Environmental%20Science&visibility=public&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取项目列表成功"
}
```

### 获取项目详情

```http
GET /v1/projects/{project_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取项目详情成功"
}
```

### 更新项目

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

响应示例：

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
  "message": "项目更新成功"
}
```

### 添加协作者

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

响应示例：

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
  "message": "协作者添加成功"
}
```

---

## 计算服务 API

计算服务 API 提供科学计算资源的管理、任务提交、结果获取等功能。

### 提交计算任务

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

响应示例：

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
  "message": "计算任务提交成功"
}
```

### 获取计算任务状态

```http
GET /v1/compute/jobs/{job_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取计算任务状态成功"
}
```

### 获取计算任务列表

```http
GET /v1/compute/jobs?project_id=prj_1234567890&status=running&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取计算任务列表成功"
}
```

### 获取计算任务结果

```http
GET /v1/compute/jobs/{job_id}/results HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取计算任务结果成功"
}
```

### 取消计算任务

```http
DELETE /v1/compute/jobs/{job_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "job_id": "job_1234567890",
    "status": "cancelled",
    "cancelled_at": "2023-07-22T17:45:12.345Z",
    "cancelled_by": "usr_1234567890"
  },
  "message": "计算任务取消成功"
}
```

---

## 数据服务 API

数据服务 API 提供数据集上传、下载、管理、查询等功能。

### 上传数据集

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

响应示例：

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
  "message": "数据集上传成功"
}
```

### 获取数据集列表

```http
GET /v1/datasets?category=Climate%20Data&visibility=public&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取数据集列表成功"
}
```

### 获取数据集详情

```http
GET /v1/datasets/{dataset_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取数据集详情成功"
}
```

### 下载数据集

```http
GET /v1/datasets/{dataset_id}/download HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应：文件流

### 更新数据集

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

响应示例：

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
  "message": "数据集更新成功"
}
```

### 删除数据集

```http
DELETE /v1/datasets/{dataset_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "dataset_id": "dts_1234567890",
    "deleted_at": "2023-07-22T19:30:45.678Z",
    "deleted_by": "usr_1234567890"
  },
  "message": "数据集删除成功"
}
```

---

## 协作服务 API

协作服务 API 提供实时协作、评论、通知等功能。

### 创建评论

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

响应示例：

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
  "message": "评论创建成功"
}
```

### 获取评论列表

```http
GET /v1/collaboration/comments?target_type=project&target_id=prj_1234567890&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取评论列表成功"
}
```

### 创建通知

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

响应示例：

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
  "message": "通知创建成功"
}
```

### 获取通知列表

```http
GET /v1/collaboration/notifications?type=meeting&status=unread&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取通知列表成功"
}
```

### 标记通知为已读

```http
PUT /v1/collaboration/notifications/{notification_id}/read HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "notification_id": "ntf_1234567890",
    "status": "read",
    "read_at": "2023-07-22T21:30:45.678Z"
  },
  "message": "通知已标记为已读"
}
```

---

## 知识服务 API

知识服务 API 提供知识库、论文管理、文献检索等功能。

### 创建知识条目

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

响应示例：

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
  "message": "知识条目创建成功"
}
```

### 获取知识条目列表

```http
GET /v1/knowledge/entries?category=Research%20Summary&project_id=prj_1234567890&page=1&limit=10 HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取知识条目列表成功"
}
```

### 获取知识条目详情

```http
GET /v1/knowledge/entries/{entry_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

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
  "message": "获取知识条目详情成功"
}
```

### 更新知识条目

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

响应示例：

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
  "message": "知识条目更新成功"
}
```

### 删除知识条目

```http
DELETE /v1/knowledge/entries/{entry_id} HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "entry_id": "kne_1234567890",
    "deleted_at": "2023-07-22T23:15:32.456Z",
    "deleted_by": "usr_1234567890"
  },
  "message": "知识条目删除成功"
}
```

---

## 错误处理

OpenMind Lab API 使用标准的 HTTP 状态码来表示请求的成功或失败。此外，响应中包含详细的错误信息，帮助开发者理解和解决问题。

### HTTP 状态码

| 状态码 | 说明 |
|--------|------|
| 200 | 请求成功 |
| 201 | 资源创建成功 |
| 204 | 请求成功，无返回内容 |
| 400 | 请求参数错误 |
| 401 | 未授权，需要认证 |
| 403 | 禁止访问，权限不足 |
| 404 | 资源不存在 |
| 422 | 请求参数验证失败 |
| 429 | 请求频率超限 |
| 500 | 服务器内部错误 |
| 503 | 服务暂时不可用 |

### 错误响应格式

```json
{
  "success": false,
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "请求参数无效",
    "details": {
      "field": "email",
      "issue": "格式无效"
    }
  },
  "timestamp": "2023-07-20T12:34:56.789Z",
  "request_id": "req_1234567890"
}
```

### 常见错误码

| 错误码 | 说明 |
|--------|------|
| INVALID_PARAMETER | 请求参数无效 |
| MISSING_PARAMETER | 缺少必要参数 |
| UNAUTHORIZED | 未授权访问 |
| FORBIDDEN | 禁止访问 |
| NOT_FOUND | 资源不存在 |
| ALREADY_EXISTS | 资源已存在 |
| QUOTA_EXCEEDED | 配额已超出 |
| RATE_LIMIT_EXCEEDED | 请求频率超限 |
| INTERNAL_ERROR | 服务器内部错误 |
| SERVICE_UNAVAILABLE | 服务暂时不可用 |

---

## API 使用示例

以下是使用不同编程语言调用 OpenMind Lab API 的示例代码。

### JavaScript (Node.js)

```javascript
const axios = require('axios');

// 配置 API 基础 URL
const api = axios.create({
  baseURL: 'https://api.openmind-lab.org/v1',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
});

// 获取访问令牌
async function getAccessToken(clientId, clientSecret) {
  try {
    const response = await api.post('/oauth/token', {
      grant_type: 'client_credentials',
      client_id: clientId,
      client_secret: clientSecret
    });
    
    return response.data.data.access_token;
  } catch (error) {
    console.error('获取访问令牌失败:', error.response.data);
    throw error;
  }
}

// 创建项目
async function createProject(accessToken, projectData) {
  try {
    const response = await api.post('/projects', projectData, {
      headers: {
        'Authorization': `Bearer ${accessToken}`
      }
    });
    
    return response.data.data;
  } catch (error) {
    console.error('创建项目失败:', error.response.data);
    throw error;
  }
}

// 使用示例
(async () => {
  const clientId = 'your_client_id';
  const clientSecret = 'your_client_secret';
  
  try {
    // 获取访问令牌
    const accessToken = await getAccessToken(clientId, clientSecret);
    console.log('访问令牌:', accessToken);
    
    // 创建项目
    const projectData = {
      title: 'AI for Climate Change Research',
      description: 'Using artificial intelligence to analyze climate data and predict climate change patterns.',
      category: 'Environmental Science',
      tags: ['AI', 'Climate Change', 'Data Analysis'],
      visibility: 'public'
    };
    
    const project = await createProject(accessToken, projectData);
    console.log('创建的项目:', project);
  } catch (error) {
    console.error('操作失败:', error.message);
  }
})();
```

### Python

```python
import requests
import json

# 配置 API 基础 URL
BASE_URL = 'https://api.openmind-lab.org/v1'

# 获取访问令牌
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
        print(f'获取访问令牌失败: {e}')
        raise

# 创建项目
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
        print(f'创建项目失败: {e}')
        raise

# 使用示例
if __name__ == '__main__':
    client_id = 'your_client_id'
    client_secret = 'your_client_secret'
    
    try:
        # 获取访问令牌
        access_token = get_access_token(client_id, client_secret)
        print(f'访问令牌: {access_token}')
        
        # 创建项目
        project_data = {
            'title': 'AI for Climate Change Research',
            'description': 'Using artificial intelligence to analyze climate data and predict climate change patterns.',
            'category': 'Environmental Science',
            'tags': ['AI', 'Climate Change', 'Data Analysis'],
            'visibility': 'public'
        }
        
        project = create_project(access_token, project_data)
        print(f'创建的项目: {json.dumps(project, indent=2)}')
    except Exception as e:
        print(f'操作失败: {e}')
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
    
    // 获取访问令牌
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
            throw new RuntimeException("获取访问令牌失败: " + response.body());
        }
        
        JsonNode responseJson = mapper.readTree(response.body());
        return responseJson.get("data").get("access_token").asText();
    }
    
    // 创建项目
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
            throw new RuntimeException("创建项目失败: " + response.body());
        }
        
        return mapper.readTree(response.body()).get("data");
    }
    
    // 使用示例
    public static void main(String[] args) {
        String clientId = "your_client_id";
        String clientSecret = "your_client_secret";
        
        try {
            // 获取访问令牌
            String accessToken = getAccessToken(clientId, clientSecret);
            System.out.println("访问令牌: " + accessToken);
            
            // 创建项目数据
            ObjectNode projectData = mapper.createObjectNode();
            projectData.put("title", "AI for Climate Change Research");
            projectData.put("description", "Using artificial intelligence to analyze climate data and predict climate change patterns.");
            projectData.put("category", "Environmental Science");
            
            ArrayNode tags = projectData.putArray("tags");
            tags.add("AI");
            tags.add("Climate Change");
            tags.add("Data Analysis");
            
            projectData.put("visibility", "public");
            
            // 创建项目
            JsonNode project = createProject(accessToken, projectData);
            System.out.println("创建的项目: " + project.toPrettyString());
        } catch (Exception e) {
            System.err.println("操作失败: " + e.getMessage());
        }
    }
}
```

---

## API 限制与配额

为了确保系统的稳定性和公平性，OpenMind Lab API 实施了以下限制和配额策略：

### 请求频率限制

| 用户类型 | 限制 |
|--------|------|
| 未认证用户 | 60 请求/分钟 |
| 普通用户 | 600 请求/分钟 |
| 高级用户 | 6000 请求/分钟 |
| 企业用户 | 自定义 |

### 资源配额

| 资源类型 | 普通用户配额 | 高级用户配额 | 企业用户配额 |
|--------|------------|------------|------------|
| 项目数量 | 10 | 100 | 自定义 |
| 数据集存储 | 10 GB | 100 GB | 自定义 |
| 计算任务/月 | 50 小时 | 500 小时 | 自定义 |
| 协作者/项目 | 5 | 50 | 自定义 |

### 配额查询

```http
GET /v1/user/quota HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

响应示例：

```json
{
  "success": true,
  "data": {
    "user_type": "普通用户",
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
  "message": "获取配额信息成功"
}
```

---

## API 版本控制

OpenMind Lab API 使用版本控制来管理 API 的演进和兼容性。

### 版本格式

API 版本遵循语义化版本控制格式：`v{major}.{minor}.{patch}`

- **主版本号 (major)**: 不兼容的 API 更改
- **次版本号 (minor)**: 向后兼容的功能性更改
- **修订号 (patch)**: 向后兼容的问题修复

### 当前版本

当前稳定版本：`v1.0.0`

### 版本策略

- **稳定版本**: 完全支持，向后兼容，推荐生产环境使用
- **测试版本**: 功能完整，但可能存在一些问题，适合测试和评估
- **开发版本**: 最新功能，但不稳定，仅用于开发和测试

### 版本兼容性

- 主版本号增加时，可能会有不兼容的更改
- 次版本号和修订号增加时，保证向后兼容
- 废弃的 API 会在至少 6 个月后移除

### 版本选择

在请求 URL 中指定要使用的 API 版本：

```http
GET /v1/projects HTTP/1.1
Host: api.openmind-lab.org
Authorization: Bearer {access_token}
```

### 版本更新通知

当有新版本发布时，系统会通过以下方式通知开发者：

1. **API 响应头**：`API-Version: v1.1.0`
2. **邮件通知**：向注册的开发者发送版本更新通知
3. **开发者门户**：在开发者门户发布版本更新公告
4. **API 文档**：更新 API 文档，标注版本差异

### 版本迁移指南

当主版本号更新时，我们会提供详细的迁移指南，帮助开发者平滑过渡到新版本。迁移指南将包括：

1. **变更摘要**：列出所有不兼容的更改
2. **迁移步骤**：提供具体的迁移步骤和代码示例
3. **兼容性检查**：提供工具帮助检查代码兼容性
4. **时间表**：明确旧版本的废弃时间和新版本的推荐使用时间
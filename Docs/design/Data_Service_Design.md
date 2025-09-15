# OpenMind Lab 数据服务设计文档

## 1. 模块概述

数据服务（Data Service）是 OpenMind Lab 的核心服务之一，负责科学数据的存储、管理、共享和访问等功能。本文档详细描述数据服务的设计方案、API 接口、数据模型和实现细节。

## 2. 功能架构

```
┌────────────────────────────────────────────────────────────────────┐
│                          数据服务架构                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  数据存储   │  │  数据管理   │  │  数据共享   │  │  数据访问   │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │                │                │                │        │
│  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐ │
│  │  元数据管理  │  │  数据处理   │  │  数据版本   │  │  数据索引   │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

## 3. 核心功能模块

### 3.1 数据存储

**主要功能：**
- 数据上传：支持各种类型科学数据的上传
- 数据存储：将数据存储到适当的存储系统
- 数据检索：快速检索和定位数据
- 数据备份：定期备份数据，确保数据安全
- 数据迁移：在不同存储系统之间迁移数据
- 存储策略：实现分层存储策略，优化存储成本

**存储类型：**
- 对象存储：用于存储非结构化数据，如原始数据文件、结果文件等
- 文件存储：用于存储文件系统数据，支持文件层次结构
- 数据库：用于存储结构化数据，如元数据、索引信息等
- 缓存存储：用于缓存频繁访问的数据，提高访问速度

### 3.2 数据管理

**主要功能：**
- 数据创建：创建数据集和数据项
- 数据查询：查询数据列表和详情
- 数据更新：修改数据信息和元数据
- 数据删除：删除不再需要的数据
- 数据分类：对数据进行分类和标签管理
- 数据归档：归档不常用的数据，节省存储资源
- 数据恢复：从备份或归档中恢复数据

**数据类型：**
- 原始数据：实验或观测获取的原始数据
- 处理数据：经过处理和转换的数据
- 分析结果：数据分析产生的结果数据
- 元数据：描述数据的数据，如数据来源、格式、创建时间等

### 3.3 数据共享

**主要功能：**
- 权限管理：设置数据的访问权限
- 数据共享：在项目成员间共享数据
- 公开共享：将数据公开给所有用户或特定用户组
- 数据授权：授权第三方访问特定数据
- 共享链接：生成数据的共享链接
- 共享审计：记录数据共享和访问日志

**权限级别：**
- 私有：仅数据所有者可访问
- 项目内共享：项目成员可访问
- 机构内共享：同一机构用户可访问
- 公开：所有用户可访问

### 3.4 数据访问

**主要功能：**
- 数据下载：下载数据文件
- 数据预览：在线预览数据内容
- 数据流式传输：支持大数据的流式传输
- 数据API：提供程序化访问数据的API
- 数据访问统计：统计数据的访问情况
- 访问控制：根据用户权限控制数据访问

### 3.5 元数据管理

**主要功能：**
- 元数据创建：创建和编辑数据的元数据
- 元数据标准化：遵循元数据标准，确保元数据的一致性
- 元数据索引：为元数据建立索引，支持快速检索
- 元数据验证：验证元数据的完整性和准确性
- 元数据版本：管理元数据的版本变更
- 元数据导出：导出元数据为标准格式

**元数据类型：**
- 技术元数据：描述数据的技术特性，如格式、大小、编码等
- 业务元数据：描述数据的业务含义和用途
- 管理元数据：描述数据的管理信息，如所有权、访问权限等
- 科学元数据：特定科学领域的元数据标准

### 3.6 数据处理

**主要功能：**
- 数据转换：转换数据格式和结构
- 数据清洗：清理和预处理数据
- 数据整合：整合来自不同来源的数据
- 数据压缩：压缩数据，减少存储空间
- 数据加密：加密敏感数据，保护数据安全
- 数据处理任务：创建和管理数据处理任务

### 3.7 数据版本

**主要功能：**
- 版本创建：创建数据的新版本
- 版本管理：管理数据的所有版本
- 版本回滚：回滚到数据的 previous 版本
- 版本比较：比较不同版本的数据差异
- 版本历史：查看数据的版本历史记录

### 3.8 数据索引

**主要功能：**
- 索引创建：为数据和元数据创建索引
- 索引更新：实时更新索引，确保数据检索的准确性
- 全文检索：支持数据内容的全文检索
- 高级搜索：支持复杂查询和过滤条件
- 搜索优化：优化搜索性能和结果相关性

## 4. 数据模型设计

### 4.1 数据实体（Data）

```typescript
interface Data {
  id: string;             // 数据唯一标识
  name: string;           // 数据名称
  description?: string;   // 数据描述
  projectId?: string;     // 所属项目ID
  userId: string;         // 创建者ID
  type: DataType;         // 数据类型
  format?: string;        // 数据格式
  sizeBytes: number;      // 数据大小（字节）
  storagePath: string;    // 存储路径
  storageType: StorageType; // 存储类型
  visibility: DataVisibility; // 数据可见性
  sharingPermission: SharingPermission; // 共享权限
  status: DataStatus;     // 数据状态
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  lastAccessedAt?: Date;  // 最后访问时间
  tags?: string[];        // 数据标签
  metadata: Record<string, any>; // 元数据
  version: number;        // 数据版本
  parentId?: string;      // 父数据ID（如果是版本数据）
  checksum?: string;      // 数据校验和
}

enum DataType {
  RAW_DATA = 'RAW_DATA',        // 原始数据
  PROCESSED_DATA = 'PROCESSED_DATA', // 处理后的数据
  ANALYSIS_RESULT = 'ANALYSIS_RESULT', // 分析结果
  DOCUMENT = 'DOCUMENT',        // 文档
  MODEL = 'MODEL',              // 模型文件
  OTHER = 'OTHER'               // 其他类型
}

enum StorageType {
  OBJECT_STORAGE = 'OBJECT_STORAGE', // 对象存储
  FILE_SYSTEM = 'FILE_SYSTEM',       // 文件系统
  DATABASE = 'DATABASE',             // 数据库
  EXTERNAL = 'EXTERNAL'              // 外部存储
}

enum DataVisibility {
  PRIVATE = 'PRIVATE',        // 私有
  PROJECT = 'PROJECT',        // 项目内可见
  ORGANIZATION = 'ORGANIZATION', // 机构内可见
  PUBLIC = 'PUBLIC'           // 公开
}

enum SharingPermission {
  VIEW = 'VIEW',              // 仅查看
  DOWNLOAD = 'DOWNLOAD',      // 可下载
  EDIT = 'EDIT',              // 可编辑
  MANAGE = 'MANAGE'           // 可管理
}

enum DataStatus {
  ACTIVE = 'ACTIVE',          // 活跃
  ARCHIVED = 'ARCHIVED',      // 已归档
  DELETED = 'DELETED'         // 已删除
}
```

### 4.2 数据集实体（DataSet）

```typescript
interface DataSet {
  id: string;             // 数据集唯一标识
  name: string;           // 数据集名称
  description?: string;   // 数据集描述
  projectId?: string;     // 所属项目ID
  userId: string;         // 创建者ID
  dataIds: string[];      // 包含的数据ID列表
  visibility: DataVisibility; // 数据集可见性
  sharingPermission: SharingPermission; // 共享权限
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  tags?: string[];        // 数据集标签
  metadata: Record<string, any>; // 数据集元数据
  version: number;        // 数据集版本
}
```

### 4.3 数据访问记录实体（DataAccessLog）

```typescript
interface DataAccessLog {
  id: string;             // 记录唯一标识
  dataId: string;         // 数据ID
  userId: string;         // 访问用户ID
  action: DataAccessAction; // 访问操作
  timestamp: Date;        // 访问时间
  ipAddress?: string;     // IP地址
  userAgent?: string;     // 用户代理
  details?: Record<string, any>; // 访问详情
}

enum DataAccessAction {
  VIEW = 'VIEW',              // 查看
  DOWNLOAD = 'DOWNLOAD',      // 下载
  UPDATE = 'UPDATE',          // 更新
  DELETE = 'DELETE',          // 删除
  SHARE = 'SHARE',            // 共享
  PREVIEW = 'PREVIEW'         // 预览
}
```

### 4.4 数据处理任务实体（DataProcessingTask）

```typescript
interface DataProcessingTask {
  id: string;             // 任务唯一标识
  name: string;           // 任务名称
  description?: string;   // 任务描述
  dataId: string;         // 目标数据ID
  userId: string;         // 创建者ID
  projectId?: string;     // 所属项目ID
  type: ProcessingType;   // 处理类型
  status: ProcessingStatus; // 处理状态
  configuration: Record<string, any>; // 处理配置
  resultDataId?: string;  // 结果数据ID
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  startedAt?: Date;       // 开始时间
  completedAt?: Date;     // 完成时间
  errorMessage?: string;  // 错误信息
}

enum ProcessingType {
  CONVERSION = 'CONVERSION',   // 格式转换
  CLEANING = 'CLEANING',       // 数据清洗
  AGGREGATION = 'AGGREGATION', // 数据聚合
  COMPRESSION = 'COMPRESSION', // 数据压缩
  ENCRYPTION = 'ENCRYPTION',   // 数据加密
  DECRYPTION = 'DECRYPTION',   // 数据解密
  ANALYSIS = 'ANALYSIS'        // 数据分析
}

enum ProcessingStatus {
  PENDING = 'PENDING',    // 等待中
  RUNNING = 'RUNNING',    // 运行中
  COMPLETED = 'COMPLETED', // 已完成
  FAILED = 'FAILED'       // 失败
}
```

## 5. API 接口设计

### 5.1 RESTful API

#### 数据管理

- **创建数据（上传）**
  - 路径：`POST /api/v1/data`
  - 请求体：`{ name, description?, projectId?, type, format?, visibility?, sharingPermission?, tags?, metadata? }` (与文件上传一起)
  - 响应：`201 Created` with data info
  - 权限：已登录用户

- **获取数据列表**
  - 路径：`GET /api/v1/data`
  - 查询参数：`page`, `pageSize`, `type?`, `format?`, `projectId?`, `visibility?`, `search?`, `sort?`
  - 响应：`200 OK` with data list and pagination info
  - 权限：已登录用户（根据数据可见性过滤）

- **获取数据详情**
  - 路径：`GET /api/v1/data/{dataId}`
  - 响应：`200 OK` with data details
  - 权限：数据所有者、项目成员或具有访问权限的用户

- **更新数据信息**
  - 路径：`PUT /api/v1/data/{dataId}`
  - 请求体：`{ name?, description?, tags?, metadata? }`
  - 响应：`200 OK` with updated data info
  - 权限：数据所有者或项目管理员

- **删除数据**
  - 路径：`DELETE /api/v1/data/{dataId}`
  - 响应：`204 No Content`
  - 权限：数据所有者或项目管理员

- **下载数据**
  - 路径：`GET /api/v1/data/{dataId}/download`
  - 响应：`200 OK` with data file (或重定向到下载链接)
  - 权限：数据所有者、项目成员或具有下载权限的用户

- **预览数据**
  - 路径：`GET /api/v1/data/{dataId}/preview`
  - 响应：`200 OK` with preview data or HTML
  - 权限：数据所有者、项目成员或具有访问权限的用户

#### 数据集管理

- **创建数据集**
  - 路径：`POST /api/v1/data-sets`
  - 请求体：`{ name, description?, projectId?, dataIds, visibility?, sharingPermission?, tags?, metadata? }`
  - 响应：`201 Created` with dataset info
  - 权限：已登录用户

- **获取数据集列表**
  - 路径：`GET /api/v1/data-sets`
  - 查询参数：`page`, `pageSize`, `projectId?`, `visibility?`, `search?`, `sort?`
  - 响应：`200 OK` with dataset list and pagination info
  - 权限：已登录用户（根据数据集可见性过滤）

- **获取数据集详情**
  - 路径：`GET /api/v1/data-sets/{dataSetId}`
  - 响应：`200 OK` with dataset details
  - 权限：数据集所有者、项目成员或具有访问权限的用户

- **更新数据集**
  - 路径：`PUT /api/v1/data-sets/{dataSetId}`
  - 请求体：`{ name?, description?, dataIds?, tags?, metadata? }`
  - 响应：`200 OK` with updated dataset info
  - 权限：数据集所有者或项目管理员

- **删除数据集**
  - 路径：`DELETE /api/v1/data-sets/{dataSetId}`
  - 响应：`204 No Content`
  - 权限：数据集所有者或项目管理员

#### 数据共享与权限

- **更新数据权限**
  - 路径：`PUT /api/v1/data/{dataId}/permissions`
  - 请求体：`{ visibility, sharingPermission }`
  - 响应：`200 OK` with updated permissions
  - 权限：数据所有者或项目管理员

- **生成共享链接**
  - 路径：`POST /api/v1/data/{dataId}/share-link`
  - 请求体：`{ expirationTime?, permission? }`
  - 响应：`200 OK` with share link info
  - 权限：数据所有者或项目管理员

- **撤销共享链接**
  - 路径：`DELETE /api/v1/data/{dataId}/share-link/{linkId}`
  - 响应：`204 No Content`
  - 权限：数据所有者或项目管理员

- **获取数据访问日志**
  - 路径：`GET /api/v1/data/{dataId}/access-logs`
  - 查询参数：`startTime?`, `endTime?`, `action?`, `page?`, `pageSize?`
  - 响应：`200 OK` with access logs
  - 权限：数据所有者或项目管理员

#### 数据处理

- **创建数据处理任务**
  - 路径：`POST /api/v1/data-processing-tasks`
  - 请求体：`{ name, description?, dataId, type, configuration }`
  - 响应：`201 Created` with task info
  - 权限：数据所有者或项目成员

- **获取数据处理任务列表**
  - 路径：`GET /api/v1/data-processing-tasks`
  - 查询参数：`page`, `pageSize`, `dataId?`, `type?`, `status?`, `sort?`
  - 响应：`200 OK` with task list
  - 权限：已登录用户（根据任务所有权过滤）

- **获取数据处理任务详情**
  - 路径：`GET /api/v1/data-processing-tasks/{taskId}`
  - 响应：`200 OK` with task details
  - 权限：任务创建者、项目成员或管理员

## 6. 服务实现

### 6.1 服务层实现

```typescript
// 数据服务实现示例
class DataService {
  private dataRepository: DataRepository;
  private dataSetRepository: DataSetRepository;
  private accessLogRepository: DataAccessLogRepository;
  private processingTaskRepository: DataProcessingTaskRepository;
  private storageService: StorageService;
  private indexingService: IndexingService;
  private notificationService: NotificationService;

  constructor(
    dataRepository: DataRepository,
    dataSetRepository: DataSetRepository,
    accessLogRepository: DataAccessLogRepository,
    processingTaskRepository: DataProcessingTaskRepository,
    storageService: StorageService,
    indexingService: IndexingService,
    notificationService: NotificationService
  ) {
    this.dataRepository = dataRepository;
    this.dataSetRepository = dataSetRepository;
    this.accessLogRepository = accessLogRepository;
    this.processingTaskRepository = processingTaskRepository;
    this.storageService = storageService;
    this.indexingService = indexingService;
    this.notificationService = notificationService;
  }

  // 上传数据
  async uploadData(uploadDataDto: UploadDataDto, file: File, userId: string): Promise<Data> {
    // 验证数据大小和类型
    if (file.size > uploadDataDto.maxSize) {
      throw new BadRequestException('File size exceeds maximum limit');
    }

    // 生成存储路径
    const storagePath = this.generateStoragePath(file.name, userId);

    // 上传文件到存储系统
    const storageResult = await this.storageService.uploadFile(storagePath, file);

    // 创建数据记录
    const data: Data = {
      id: uuidv4(),
      name: uploadDataDto.name || file.name,
      description: uploadDataDto.description,
      projectId: uploadDataDto.projectId,
      userId,
      type: uploadDataDto.type || DataType.RAW_DATA,
      format: uploadDataDto.format || this.determineFormat(file.name),
      sizeBytes: file.size,
      storagePath: storageResult.path,
      storageType: StorageType.OBJECT_STORAGE,
      visibility: uploadDataDto.visibility || DataVisibility.PRIVATE,
      sharingPermission: uploadDataDto.sharingPermission || SharingPermission.VIEW,
      status: DataStatus.ACTIVE,
      createdAt: new Date(),
      updatedAt: new Date(),
      tags: uploadDataDto.tags || [],
      metadata: uploadDataDto.metadata || {},
      version: 1,
      checksum: storageResult.checksum
    };

    // 保存数据记录
    const savedData = await this.dataRepository.save(data);

    // 为数据创建索引
    await this.indexingService.indexData(savedData);

    // 记录数据访问日志
    await this.logDataAccess(savedData.id, userId, DataAccessAction.UPLOAD);

    // 如果有项目关联，通知项目成员
    if (savedData.projectId) {
      await this.notificationService.sendDataUploadNotification(savedData.projectId, savedData.id);
    }

    return savedData;
  }

  // 获取数据详情
  async getDataById(dataId: string, userId: string): Promise<Data> {
    const data = await this.dataRepository.findById(dataId);
    if (!data) {
      throw new NotFoundException('Data not found');
    }

    // 验证用户是否有权限访问数据
    if (!await this.hasDataAccessPermission(data, userId)) {
      throw new ForbiddenException('You do not have permission to access this data');
    }

    // 更新最后访问时间
    data.lastAccessedAt = new Date();
    await this.dataRepository.save(data);

    // 记录数据访问日志
    await this.logDataAccess(dataId, userId, DataAccessAction.VIEW);

    return data;
  }

  // 下载数据
  async downloadData(dataId: string, userId: string): Promise<DownloadResult> {
    const data = await this.getDataById(dataId, userId);

    // 验证用户是否有下载权限
    if (!await this.hasDataDownloadPermission(data, userId)) {
      throw new ForbiddenException('You do not have permission to download this data');
    }

    // 从存储系统获取文件
    const downloadResult = await this.storageService.downloadFile(data.storagePath);

    // 记录数据下载日志
    await this.logDataAccess(dataId, userId, DataAccessAction.DOWNLOAD);

    return downloadResult;
  }

  // 创建数据集
  async createDataSet(createDataSetDto: CreateDataSetDto, userId: string): Promise<DataSet> {
    // 验证数据是否存在且用户有权访问
    for (const dataId of createDataSetDto.dataIds) {
      const data = await this.dataRepository.findById(dataId);
      if (!data || !await this.hasDataAccessPermission(data, userId)) {
        throw new BadRequestException(`You do not have permission to access data with id ${dataId}`);
      }
    }

    // 创建数据集
    const dataSet: DataSet = {
      id: uuidv4(),
      name: createDataSetDto.name,
      description: createDataSetDto.description,
      projectId: createDataSetDto.projectId,
      userId,
      dataIds: createDataSetDto.dataIds,
      visibility: createDataSetDto.visibility || DataVisibility.PRIVATE,
      sharingPermission: createDataSetDto.sharingPermission || SharingPermission.VIEW,
      createdAt: new Date(),
      updatedAt: new Date(),
      tags: createDataSetDto.tags || [],
      metadata: createDataSetDto.metadata || {},
      version: 1
    };

    // 保存数据集
    const savedDataSet = await this.dataSetRepository.save(dataSet);

    // 为数据集创建索引
    await this.indexingService.indexDataSet(savedDataSet);

    return savedDataSet;
  }

  // 更新数据权限
  async updateDataPermissions(dataId: string, permissionsDto: DataPermissionsDto, userId: string): Promise<Data> {
    const data = await this.dataRepository.findById(dataId);
    if (!data) {
      throw new NotFoundException('Data not found');
    }

    // 验证用户是否有管理权限
    if (!await this.hasDataManagePermission(data, userId)) {
      throw new ForbiddenException('You do not have permission to manage this data');
    }

    // 更新权限
    if (permissionsDto.visibility !== undefined) {
      data.visibility = permissionsDto.visibility;
    }
    if (permissionsDto.sharingPermission !== undefined) {
      data.sharingPermission = permissionsDto.sharingPermission;
    }
    data.updatedAt = new Date();

    const updatedData = await this.dataRepository.save(data);

    // 更新索引
    await this.indexingService.updateDataIndex(updatedData);

    return updatedData;
  }

  // 创建数据处理任务
  async createProcessingTask(createTaskDto: CreateProcessingTaskDto, userId: string): Promise<DataProcessingTask> {
    // 验证数据存在且用户有权访问
    const data = await this.dataRepository.findById(createTaskDto.dataId);
    if (!data || !await this.hasDataAccessPermission(data, userId)) {
      throw new BadRequestException('Invalid data ID or insufficient permissions');
    }

    // 创建处理任务
    const task: DataProcessingTask = {
      id: uuidv4(),
      name: createTaskDto.name,
      description: createTaskDto.description,
      dataId: createTaskDto.dataId,
      userId,
      projectId: data.projectId,
      type: createTaskDto.type,
      status: ProcessingStatus.PENDING,
      configuration: createTaskDto.configuration,
      createdAt: new Date(),
      updatedAt: new Date()
    };

    // 保存任务
    const savedTask = await this.processingTaskRepository.save(task);

    // 提交任务到处理队列
    // await this.processingQueue.submitTask(savedTask);

    return savedTask;
  }

  // 检查用户是否有数据访问权限
  private async hasDataAccessPermission(data: Data, userId: string): Promise<boolean> {
    // 数据所有者有访问权限
    if (data.userId === userId) {
      return true;
    }

    // 公开数据所有人可访问
    if (data.visibility === DataVisibility.PUBLIC) {
      return true;
    }

    // 项目内可见数据，检查用户是否为项目成员
    if (data.visibility === DataVisibility.PROJECT && data.projectId) {
      // 假设存在一个projectService来检查项目成员关系
      // return await projectService.isProjectMember(data.projectId, userId);
    }

    // 机构内可见数据，检查用户是否为机构成员
    if (data.visibility === DataVisibility.ORGANIZATION) {
      // 检查用户机构关系
      // ...
    }

    return false;
  }

  // 检查用户是否有数据下载权限
  private async hasDataDownloadPermission(data: Data, userId: string): Promise<boolean> {
    // 首先检查是否有访问权限
    if (!await this.hasDataAccessPermission(data, userId)) {
      return false;
    }

    // 检查共享权限是否允许下载
    return data.sharingPermission === SharingPermission.DOWNLOAD || 
           data.sharingPermission === SharingPermission.EDIT || 
           data.sharingPermission === SharingPermission.MANAGE;
  }

  // 检查用户是否有数据管理权限
  private async hasDataManagePermission(data: Data, userId: string): Promise<boolean> {
    // 数据所有者有管理权限
    if (data.userId === userId) {
      return true;
    }

    // 如果数据属于项目，项目管理员有管理权限
    if (data.projectId) {
      // 假设存在一个projectService来检查项目管理员关系
      // return await projectService.isProjectAdmin(data.projectId, userId);
    }

    return false;
  }

  // 记录数据访问日志
  private async logDataAccess(dataId: string, userId: string, action: DataAccessAction): Promise<void> {
    await this.accessLogRepository.save({
      id: uuidv4(),
      dataId,
      userId,
      action,
      timestamp: new Date()
    });
  }

  // 生成存储路径
  private generateStoragePath(filename: string, userId: string): string {
    const timestamp = Date.now();
    const randomString = Math.random().toString(36).substring(2, 10);
    return `${userId}/${timestamp}-${randomString}-${filename}`;
  }

  // 确定文件格式
  private determineFormat(filename: string): string {
    const extension = filename.split('.').pop()?.toLowerCase() || '';
    return extension;
  }

  // 其他数据服务方法...
}
```

### 6.2 控制器实现

```typescript
// 数据控制器实现示例
class DataController {
  private dataService: DataService;
  
  constructor(dataService: DataService) {
    this.dataService = dataService;
  }

  // 上传数据
  @Post('/data')
  @AuthGuard()
  @UseInterceptors(FileInterceptor('file'))
  async uploadData(@UploadedFile() file: Express.Multer.File, @Body() uploadDataDto: UploadDataDto, @Request() req, @Response() res) {
    try {
      if (!file) {
        return res.status(HttpStatus.BAD_REQUEST).json({ message: 'File is required' });
      }
      
      const userId = req.user.id;
      const data = await this.dataService.uploadData(uploadDataDto, file, userId);
      res.status(HttpStatus.CREATED).json(data);
    } catch (error) {
      if (error instanceof BadRequestException) {
        res.status(HttpStatus.BAD_REQUEST).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 获取数据列表
  @Get('/data')
  @AuthGuard()
  async getDataList(@Query() query: DataQueryDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const dataList = await this.dataService.getDataList(userId, query);
      res.status(HttpStatus.OK).json(dataList);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 获取数据详情
  @Get('/data/:dataId')
  @AuthGuard()
  async getDataById(@Param('dataId') dataId: string, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const data = await this.dataService.getDataById(dataId, userId);
      res.status(HttpStatus.OK).json(data);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 下载数据
  @Get('/data/:dataId/download')
  @AuthGuard()
  async downloadData(@Param('dataId') dataId: string, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const downloadResult = await this.dataService.downloadData(dataId, userId);
      
      // 设置响应头并发送文件
      res.setHeader('Content-Disposition', `attachment; filename=${downloadResult.filename}`);
      res.setHeader('Content-Type', downloadResult.contentType);
      res.send(downloadResult.data);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 创建数据集
  @Post('/data-sets')
  @AuthGuard()
  async createDataSet(@Body() createDataSetDto: CreateDataSetDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const dataSet = await this.dataService.createDataSet(createDataSetDto, userId);
      res.status(HttpStatus.CREATED).json(dataSet);
    } catch (error) {
      if (error instanceof BadRequestException) {
        res.status(HttpStatus.BAD_REQUEST).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 其他控制器方法...
}
```

## 7. 安全考虑

### 7.1 数据访问控制

- 严格的权限模型：基于角色和所有权的数据访问控制
- 多层权限检查：对每个数据操作进行多层权限验证
- 最小权限原则：用户仅获得完成其工作所需的最小权限
- 动态权限调整：根据项目和用户关系动态调整权限

### 7.2 数据保护

- 传输加密：所有数据传输必须使用HTTPS/TLS加密
- 存储加密：敏感数据在存储时进行加密
- 数据备份：定期进行数据备份，并存储在安全位置
- 灾难恢复：建立完善的数据灾难恢复机制
- 数据保留策略：根据数据类型和法规要求制定数据保留策略

### 7.3 防止未授权访问

- 认证机制：所有数据访问必须经过身份认证
- API安全：API接口必须进行严格的认证和授权检查
- 会话管理：实施安全的会话管理，防止会话劫持
- 审计日志：记录所有数据访问和操作日志，便于追踪
- 访问限制：对异常访问模式实施限制和告警

### 7.4 数据隐私

- 数据脱敏：对敏感数据进行脱敏处理
- 隐私合规：遵循相关数据隐私法规（如GDPR、CCPA等）
- 匿名化处理：对需要共享但包含个人信息的数据进行匿名化处理
- 用户授权：在收集和使用用户数据前获得明确授权

## 8. 性能优化

### 8.1 存储优化

- 分层存储：根据数据访问频率和重要性实施分层存储
- 数据压缩：对数据进行压缩，减少存储空间占用
- 去重处理：识别并删除重复数据，节省存储空间
- 存储缓存：使用缓存技术提高频繁访问数据的读取速度

### 8.2 数据索引与检索

- 高效索引：使用高性能索引技术（如Elasticsearch）
- 全文检索：支持快速的全文检索功能
- 索引优化：定期优化索引结构，提高检索性能
- 缓存机制：缓存查询结果，减少重复查询

### 8.3 数据传输

- 断点续传：支持大文件的断点续传功能
- 分片上传/下载：对大文件进行分片处理，提高传输效率
- CDN加速：使用CDN加速数据传输，减少延迟
- 压缩传输：在传输过程中压缩数据，减少带宽占用

### 8.4 并发处理

- 并发上传/下载：支持多个用户同时上传和下载数据
- 异步处理：非关键操作使用异步处理，提高系统响应速度
- 队列处理：使用消息队列处理大量数据操作请求
- 资源隔离：不同用户和项目的数据操作相互隔离，防止资源竞争

## 9. 总结

数据服务是OpenMind Lab系统的核心服务之一，负责科学数据的存储、管理、共享和访问等功能。本文档详细描述了数据服务的设计方案，包括功能架构、数据模型、API接口、服务实现、安全考虑和性能优化等方面。通过合理的设计和实现，数据服务能够为科研人员提供高效、安全、可靠的数据管理平台，支持各种科学数据的全生命周期管理。未来，数据服务可以根据业务需求进行扩展，如支持更多数据类型、优化存储策略、增强数据处理能力、集成更多数据分析工具等，进一步提升用户体验和系统价值。
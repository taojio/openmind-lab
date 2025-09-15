# OpenMind Lab 项目服务设计文档

## 1. 模块概述

项目服务（Project Service）是 OpenMind Lab 的核心服务之一，负责项目的创建、管理、协作和资源分配等功能。本文档详细描述项目服务的设计方案、API 接口、数据模型和实现细节。

## 2. 功能架构

```
┌────────────────────────────────────────────────────────────────────┐
│                          项目服务架构                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  项目管理   │  │  成员管理   │  │  资源管理   │  │  权限控制   │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │                │                │                │        │
│  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐ │
│  │  项目配置   │  │  项目统计   │  │  项目版本   │  │  项目模板   │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

## 3. 核心功能模块

### 3.1 项目管理

**主要功能：**
- 项目创建：创建新的科研项目
- 项目查询：查询项目列表和详情
- 项目更新：修改项目信息
- 项目删除：删除项目（支持软删除和永久删除）
- 项目状态管理：控制项目的生命周期状态
- 项目归档：归档不再活跃的项目

**关键流程：**
1. 项目创建流程：
   - 接收创建请求（项目名称、描述、负责人等）
   - 验证项目信息的合法性
   - 创建项目记录
   - 设置项目初始配置
   - 为创建者分配项目管理员权限
   - 返回项目创建成功信息

2. 项目更新流程：
   - 接收更新请求（项目ID、待更新字段）
   - 验证用户是否有权限更新项目
   - 更新项目记录
   - 返回更新成功信息

### 3.2 成员管理

**主要功能：**
- 成员邀请：邀请用户加入项目
- 成员添加：直接添加用户到项目
- 成员移除：从项目中移除成员
- 成员角色管理：分配和修改项目成员的角色
- 成员列表查询：查询项目成员列表
- 成员权限查询：查询项目成员的具体权限

**项目角色类型：**
- 项目管理员：拥有项目的所有权限，可管理成员和资源
- 项目负责人：负责项目的日常管理和协调
- 核心成员：参与项目核心工作的成员
- 普通成员：参与项目部分工作的成员
- 观察者：仅可查看项目信息的成员

### 3.3 资源管理

**主要功能：**
- 资源关联：将数据、计算任务、文档等资源关联到项目
- 资源查询：查询项目关联的所有资源
- 资源分配：为项目分配计算资源、存储资源等
- 资源使用监控：监控项目资源的使用情况
- 资源配额管理：设置和管理项目的资源使用配额

**资源类型：**
- 数据资源：数据集、原始数据、处理后的数据分析结果等
- 计算资源：计算任务、计算集群、GPU资源等
- 文档资源：项目文档、实验记录、报告等
- 协作资源：协作会话、讨论记录、共享笔记等

### 3.4 权限控制

**主要功能：**
- 项目级权限：控制用户对整个项目的操作权限
- 资源级权限：控制用户对项目内特定资源的操作权限
- 权限继承：实现权限的继承机制
- 权限检查：验证用户是否有权执行特定操作

**权限类型：**
- 查看权限：查看项目信息和资源
- 创建权限：创建项目资源
- 编辑权限：编辑项目信息和资源
- 删除权限：删除项目资源
- 管理权限：管理项目成员和权限

### 3.5 项目配置

**主要功能：**
- 项目设置：配置项目的基本设置
- 通知设置：配置项目的通知规则
- 安全设置：配置项目的安全选项
- 集成设置：配置第三方服务的集成
- 自定义字段：定义项目的自定义字段

### 3.6 项目统计

**主要功能：**
- 项目活跃度统计：统计项目的活跃程度
- 资源使用统计：统计项目资源的使用情况
- 成员贡献统计：统计项目成员的贡献情况
- 项目进度统计：统计项目的进度和里程碑完成情况
- 导出统计报表：生成和导出项目统计报表

## 4. 数据模型设计

### 4.1 项目实体（Project）

```typescript
interface Project {
  id: string;             // 项目唯一标识
  name: string;           // 项目名称
  description?: string;   // 项目描述
  creatorId: string;      // 创建者ID
  ownerId: string;        // 项目负责人ID
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  startDate?: Date;       // 项目开始日期
  endDate?: Date;         // 项目结束日期
  status: ProjectStatus;  // 项目状态
  visibility: ProjectVisibility; // 项目可见性
  tags?: string[];        // 项目标签
  settings?: Record<string, any>; // 项目设置
  metadata?: Record<string, any>; // 项目元数据
  thumbnailUrl?: string;  // 项目缩略图URL
}

enum ProjectStatus {
  PLANNING = 'PLANNING',    // 规划中
  ACTIVE = 'ACTIVE',        // 进行中
  ON_HOLD = 'ON_HOLD',      // 暂停
  COMPLETED = 'COMPLETED',  // 已完成
  CANCELLED = 'CANCELLED'   // 已取消
}

enum ProjectVisibility {
  PRIVATE = 'PRIVATE',      // 私有项目
  INTERNAL = 'INTERNAL',    // 内部可见
  PUBLIC = 'PUBLIC'         // 公开项目
}
```

### 4.2 项目成员实体（ProjectMember）

```typescript
interface ProjectMember {
  id: string;             // 成员关系唯一标识
  projectId: string;      // 项目ID
  userId: string;         // 用户ID
  role: ProjectRole;      // 项目角色
  joinedAt: Date;         // 加入时间
  lastActiveAt?: Date;    // 最后活跃时间
  permissions?: string[]; // 自定义权限列表（如果有）
}

enum ProjectRole {
  ADMIN = 'ADMIN',          // 管理员
  OWNER = 'OWNER',          // 负责人
  CORE_MEMBER = 'CORE_MEMBER', // 核心成员
  MEMBER = 'MEMBER',        // 普通成员
  OBSERVER = 'OBSERVER'     // 观察者
}
```

### 4.3 项目资源实体（ProjectResource）

```typescript
interface ProjectResource {
  id: string;             // 资源关联唯一标识
  projectId: string;      // 项目ID
  resourceId: string;     // 资源ID
  resourceType: ResourceType; // 资源类型
  addedBy: string;        // 添加者ID
  addedAt: Date;          // 添加时间
  metadata?: Record<string, any>; // 资源元数据
}

enum ResourceType {
  DATA = 'DATA',           // 数据资源
  COMPUTING_TASK = 'COMPUTING_TASK', // 计算任务
  DOCUMENT = 'DOCUMENT',   // 文档
  COLLABORATION = 'COLLABORATION', // 协作会话
  // 其他资源类型...
}
```

### 4.4 项目里程碑实体（ProjectMilestone）

```typescript
interface ProjectMilestone {
  id: string;             // 里程碑唯一标识
  projectId: string;      // 项目ID
  name: string;           // 里程碑名称
  description?: string;   // 里程碑描述
  dueDate: Date;          // 截止日期
  completedDate?: Date;   // 完成日期
  status: MilestoneStatus; // 里程碑状态
  createdBy: string;      // 创建者ID
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
}

enum MilestoneStatus {
  PENDING = 'PENDING',    // 待完成
  IN_PROGRESS = 'IN_PROGRESS', // 进行中
  COMPLETED = 'COMPLETED', // 已完成
  DELAYED = 'DELAYED'     // 已延迟
}
```

## 5. API 接口设计

### 5.1 RESTful API

#### 项目管理

- **创建项目**
  - 路径：`POST /api/v1/projects`
  - 请求体：`{ name, description?, startDate?, endDate?, visibility?, tags? }`
  - 响应：`201 Created` with project data
  - 权限：已登录用户

- **获取项目列表**
  - 路径：`GET /api/v1/projects`
  - 查询参数：`page`, `pageSize`, `status?`, `visibility?`, `search?`, `sort?`
  - 响应：`200 OK` with project list and pagination info
  - 权限：已登录用户（根据项目可见性过滤）

- **获取项目详情**
  - 路径：`GET /api/v1/projects/{projectId}`
  - 响应：`200 OK` with project details
  - 权限：项目成员或公开项目

- **更新项目**
  - 路径：`PUT /api/v1/projects/{projectId}`
  - 请求体：`{ name?, description?, startDate?, endDate?, status?, visibility?, tags? }`
  - 响应：`200 OK` with updated project data
  - 权限：项目管理员或负责人

- **删除项目**
  - 路径：`DELETE /api/v1/projects/{projectId}`
  - 响应：`204 No Content`
  - 权限：项目管理员或负责人

#### 成员管理

- **邀请成员**
  - 路径：`POST /api/v1/projects/{projectId}/members/invite`
  - 请求体：`{ email, role }`
  - 响应：`200 OK` with invitation info
  - 权限：项目管理员或负责人

- **添加成员**
  - 路径：`POST /api/v1/projects/{projectId}/members`
  - 请求体：`{ userId, role }`
  - 响应：`201 Created` with member info
  - 权限：项目管理员或负责人

- **获取成员列表**
  - 路径：`GET /api/v1/projects/{projectId}/members`
  - 查询参数：`role?`
  - 响应：`200 OK` with member list
  - 权限：项目成员

- **更新成员角色**
  - 路径：`PUT /api/v1/projects/{projectId}/members/{memberId}`
  - 请求体：`{ role }`
  - 响应：`200 OK` with updated member info
  - 权限：项目管理员或负责人

- **移除成员**
  - 路径：`DELETE /api/v1/projects/{projectId}/members/{memberId}`
  - 响应：`204 No Content`
  - 权限：项目管理员或负责人

#### 资源管理

- **关联资源**
  - 路径：`POST /api/v1/projects/{projectId}/resources`
  - 请求体：`{ resourceId, resourceType, metadata? }`
  - 响应：`201 Created` with resource association info
  - 权限：项目成员（根据资源类型和权限）

- **获取资源列表**
  - 路径：`GET /api/v1/projects/{projectId}/resources`
  - 查询参数：`resourceType?`, `sort?`
  - 响应：`200 OK` with resource list
  - 权限：项目成员

- **移除资源关联**
  - 路径：`DELETE /api/v1/projects/{projectId}/resources/{resourceAssociationId}`
  - 响应：`204 No Content`
  - 权限：项目成员（根据资源类型和权限）

#### 项目配置

- **更新项目设置**
  - 路径：`PUT /api/v1/projects/{projectId}/settings`
  - 请求体：`{ settings }`
  - 响应：`200 OK` with updated settings
  - 权限：项目管理员或负责人

- **获取项目设置**
  - 路径：`GET /api/v1/projects/{projectId}/settings`
  - 响应：`200 OK` with project settings
  - 权限：项目成员

#### 项目统计

- **获取项目统计信息**
  - 路径：`GET /api/v1/projects/{projectId}/stats`
  - 查询参数：`timeRange?`, `type?`
  - 响应：`200 OK` with project statistics
  - 权限：项目成员（根据统计类型和权限）

## 6. 服务实现

### 6.1 服务层实现

```typescript
// 项目服务实现示例
class ProjectService {
  private projectRepository: ProjectRepository;
  private projectMemberRepository: ProjectMemberRepository;
  private projectResourceRepository: ProjectResourceRepository;
  private userService: UserService;
  
  constructor(
    projectRepository: ProjectRepository,
    projectMemberRepository: ProjectMemberRepository,
    projectResourceRepository: ProjectResourceRepository,
    userService: UserService
  ) {
    this.projectRepository = projectRepository;
    this.projectMemberRepository = projectMemberRepository;
    this.projectResourceRepository = projectResourceRepository;
    this.userService = userService;
  }

  // 创建项目
  async createProject(createProjectDto: CreateProjectDto, userId: string): Promise<Project> {
    // 验证项目名称是否已存在
    const existingProject = await this.projectRepository.findByName(createProjectDto.name);
    if (existingProject) {
      throw new ConflictException('Project name already exists');
    }

    // 创建项目
    const project: Project = {
      id: uuidv4(),
      name: createProjectDto.name,
      description: createProjectDto.description,
      creatorId: userId,
      ownerId: userId,
      createdAt: new Date(),
      updatedAt: new Date(),
      startDate: createProjectDto.startDate,
      endDate: createProjectDto.endDate,
      status: ProjectStatus.PLANNING,
      visibility: createProjectDto.visibility || ProjectVisibility.PRIVATE,
      tags: createProjectDto.tags || [],
      settings: createProjectDto.settings || {}
    };

    // 保存项目
    const savedProject = await this.projectRepository.save(project);

    // 添加创建者为项目管理员
    await this.addProjectMember(savedProject.id, userId, ProjectRole.ADMIN);

    return savedProject;
  }

  // 获取项目详情
  async getProjectById(projectId: string, userId: string): Promise<Project> {
    const project = await this.projectRepository.findById(projectId);
    if (!project) {
      throw new NotFoundException('Project not found');
    }

    // 验证用户是否有权查看项目
    if (project.visibility === ProjectVisibility.PRIVATE) {
      const isMember = await this.isProjectMember(projectId, userId);
      if (!isMember) {
        throw new ForbiddenException('You do not have permission to view this project');
      }
    }

    return project;
  }

  // 更新项目信息
  async updateProject(projectId: string, updateProjectDto: UpdateProjectDto, userId: string): Promise<Project> {
    const project = await this.getProjectById(projectId, userId);

    // 验证用户是否有权更新项目
    const isAdmin = await this.isProjectAdmin(projectId, userId);
    if (!isAdmin) {
      throw new ForbiddenException('You do not have permission to update this project');
    }

    // 更新项目信息
    if (updateProjectDto.name) project.name = updateProjectDto.name;
    if (updateProjectDto.description) project.description = updateProjectDto.description;
    if (updateProjectDto.startDate) project.startDate = updateProjectDto.startDate;
    if (updateProjectDto.endDate) project.endDate = updateProjectDto.endDate;
    if (updateProjectDto.status) project.status = updateProjectDto.status;
    if (updateProjectDto.visibility) project.visibility = updateProjectDto.visibility;
    if (updateProjectDto.tags) project.tags = updateProjectDto.tags;
    if (updateProjectDto.settings) project.settings = { ...project.settings, ...updateProjectDto.settings };
    project.updatedAt = new Date();

    return this.projectRepository.save(project);
  }

  // 添加项目成员
  async addProjectMember(projectId: string, userId: string, role: ProjectRole, addedBy: string): Promise<ProjectMember> {
    // 验证项目存在
    const project = await this.projectRepository.findById(projectId);
    if (!project) {
      throw new NotFoundException('Project not found');
    }

    // 验证用户存在
    const user = await this.userService.getUserById(userId);
    if (!user) {
      throw new NotFoundException('User not found');
    }

    // 验证添加者权限
    const canAddMember = await this.isProjectAdmin(projectId, addedBy);
    if (!canAddMember) {
      throw new ForbiddenException('You do not have permission to add members to this project');
    }

    // 检查用户是否已经是项目成员
    const existingMember = await this.projectMemberRepository.findByProjectIdAndUserId(projectId, userId);
    if (existingMember) {
      throw new ConflictException('User is already a member of this project');
    }

    // 创建项目成员记录
    const projectMember: ProjectMember = {
      id: uuidv4(),
      projectId,
      userId,
      role,
      joinedAt: new Date()
    };

    return this.projectMemberRepository.save(projectMember);
  }

  // 关联资源到项目
  async associateResource(projectId: string, resourceId: string, resourceType: ResourceType, addedBy: string): Promise<ProjectResource> {
    // 验证项目存在
    const project = await this.projectRepository.findById(projectId);
    if (!project) {
      throw new NotFoundException('Project not found');
    }

    // 验证用户是否为项目成员
    const isMember = await this.isProjectMember(projectId, addedBy);
    if (!isMember) {
      throw new ForbiddenException('You are not a member of this project');
    }

    // 检查资源是否已经关联
    const existingAssociation = await this.projectResourceRepository.findByProjectIdAndResourceId(projectId, resourceId);
    if (existingAssociation) {
      throw new ConflictException('Resource is already associated with this project');
    }

    // 创建资源关联记录
    const projectResource: ProjectResource = {
      id: uuidv4(),
      projectId,
      resourceId,
      resourceType,
      addedBy,
      addedAt: new Date()
    };

    return this.projectResourceRepository.save(projectResource);
  }

  // 检查用户是否为项目成员
  async isProjectMember(projectId: string, userId: string): Promise<boolean> {
    const member = await this.projectMemberRepository.findByProjectIdAndUserId(projectId, userId);
    return !!member;
  }

  // 检查用户是否为项目管理员
  async isProjectAdmin(projectId: string, userId: string): Promise<boolean> {
    const member = await this.projectMemberRepository.findByProjectIdAndUserId(projectId, userId);
    return member && (member.role === ProjectRole.ADMIN || member.role === ProjectRole.OWNER);
  }

  // 其他项目服务方法...
}
```

### 6.2 控制器实现

```typescript
// 项目控制器实现示例
class ProjectController {
  private projectService: ProjectService;
  
  constructor(projectService: ProjectService) {
    this.projectService = projectService;
  }

  // 创建项目
  @Post('/projects')
  @AuthGuard()
  async createProject(@Body() createProjectDto: CreateProjectDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const project = await this.projectService.createProject(createProjectDto, userId);
      res.status(HttpStatus.CREATED).json(project);
    } catch (error) {
      if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 获取项目列表
  @Get('/projects')
  @AuthGuard()
  async getProjectList(@Query() query: ProjectQueryDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const projects = await this.projectService.getProjects(userId, query);
      res.status(HttpStatus.OK).json(projects);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 获取项目详情
  @Get('/projects/:projectId')
  @AuthGuard()
  async getProjectById(@Param('projectId') projectId: string, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const project = await this.projectService.getProjectById(projectId, userId);
      res.status(HttpStatus.OK).json(project);
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

  // 添加项目成员
  @Post('/projects/:projectId/members')
  @AuthGuard()
  async addProjectMember(@Param('projectId') projectId: string, @Body() addMemberDto: AddProjectMemberDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const member = await this.projectService.addProjectMember(
        projectId, 
        addMemberDto.userId, 
        addMemberDto.role, 
        userId
      );
      res.status(HttpStatus.CREATED).json(member);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 其他控制器方法...
}
```

## 7. 安全考虑

### 7.1 权限控制

- 严格的权限检查：对所有项目操作进行权限验证
- 基于角色的访问控制：实现细粒度的角色权限管理
- 最小权限原则：用户只能获取完成其工作所需的最小权限
- 动态权限：支持根据项目阶段和用户贡献动态调整权限

### 7.2 数据保护

- 项目数据加密：敏感项目数据在传输和存储时进行加密
- 数据隔离：不同项目的数据进行逻辑或物理隔离
- 访问日志：记录所有项目操作日志，便于审计和追踪
- 数据备份：定期对项目数据进行备份，确保数据安全

### 7.3 防止未授权访问

- API 安全：所有API接口必须进行身份认证和权限检查
- 会话管理：实施安全的会话管理机制，防止会话劫持
- 跨域保护：配置适当的CORS策略，防止跨域攻击
- 输入验证：对所有用户输入进行严格验证，防止注入攻击

## 8. 性能优化

### 8.1 数据库优化

- 索引优化：为常用查询字段创建索引
- 查询优化：优化复杂查询，减少数据库负载
- 连接池：使用数据库连接池，提高数据库访问效率
- 缓存策略：缓存项目详情、成员列表等频繁访问的数据

### 8.2 服务性能优化

- 异步处理：非关键操作使用异步处理，提高响应速度
- 批量操作：支持批量添加成员、批量关联资源等操作
- 分页处理：大列表查询实现分页，避免返回过多数据
- 数据预加载：优化数据加载策略，减少N+1查询问题

### 8.3 扩展性考虑

- 水平扩展：设计支持水平扩展的架构
- 微服务架构：采用微服务架构，提高系统的可扩展性
- 资源隔离：不同项目的资源使用进行隔离，防止资源竞争
- 自动扩缩容：根据负载情况自动调整服务实例数量

## 9. 总结

项目服务是OpenMind Lab系统的核心服务之一，负责项目的创建、管理、成员协作和资源分配等功能。本文档详细描述了项目服务的设计方案，包括功能架构、数据模型、API接口、服务实现、安全考虑和性能优化等方面。通过合理的设计和实现，项目服务能够为科研人员提供高效、安全、可靠的项目管理平台，促进团队协作和科研创新。未来，项目服务可以根据业务需求进行扩展，如增强项目模板功能、优化项目统计分析、集成更多第三方工具等，进一步提升用户体验和系统价值。
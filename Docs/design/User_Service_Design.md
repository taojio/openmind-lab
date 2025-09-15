# OpenMind Lab 用户服务设计文档

## 1. 模块概述

用户服务（User Service）是 OpenMind Lab 的核心服务之一，负责用户身份管理、认证授权、个人信息维护等功能。本文档详细描述用户服务的设计方案、API 接口、数据模型和实现细节。

## 2. 功能架构

```
┌─────────────────────────────────────┐
│            用户服务架构             │
│  ┌─────────────┐  ┌─────────────┐   │
│  │  用户管理   │  │  认证授权   │   │
│  └──────┬──────┘  └──────┬──────┘   │
│         │                │          │
│  ┌──────▼──────┐  ┌──────▼──────┐   │
│  │  角色管理   │  │  权限管理   │   │
│  └─────────────┘  └─────────────┘   │
└─────────────────────────────────────┘
```

## 3. 核心功能模块

### 3.1 用户管理

**主要功能：**
- 用户注册：新用户账号创建与验证
- 用户登录：账号密码验证与会话管理
- 用户信息管理：个人资料、联系方式等信息的查看与修改
- 用户状态管理：启用/禁用用户账号
- 用户删除：永久或临时删除用户账号

**关键流程：**
1. 用户注册流程：
   - 接收注册请求（邮箱/手机号、密码、用户名等）
   - 验证用户信息的合法性
   - 对密码进行加密处理
   - 创建用户记录
   - 发送验证邮件/短信
   - 返回注册成功信息

2. 用户信息更新流程：
   - 接收更新请求（用户ID、待更新字段）
   - 验证用户身份和权限
   - 更新用户记录
   - 返回更新成功信息

### 3.2 认证授权

**主要功能：**
- 身份认证：验证用户身份的真实性
- 会话管理：创建和维护用户会话
- 令牌管理：生成和验证访问令牌
- 多因素认证：支持二次验证机制
- 第三方登录集成：支持OAuth等第三方认证

**认证流程：**
1. 账号密码认证：
   - 接收认证请求（用户名/邮箱、密码）
   - 验证用户名和密码
   - 生成JWT令牌
   - 返回令牌和用户信息

2. 令牌验证流程：
   - 接收带令牌的请求
   - 验证令牌的有效性（签名、过期时间等）
   - 解析令牌中的用户信息
   - 返回验证结果

### 3.3 角色管理

**主要功能：**
- 角色创建：创建系统角色
- 角色查询：查询系统角色列表和详情
- 角色更新：修改角色名称和描述
- 角色删除：删除不再使用的角色
- 角色分配：为用户分配角色

**角色类型：**
- 超级管理员：系统最高权限，可管理所有功能
- 管理员：管理用户、项目等系统资源
- 普通用户：基本的使用权限
- 访客：有限的浏览权限

### 3.4 权限管理

**主要功能：**
- 权限定义：定义系统操作权限
- 权限分配：为角色分配权限
- 权限检查：验证用户是否有权执行操作
- 权限集管理：批量管理权限

**权限类型：**
- 功能权限：执行特定功能的权限
- 数据权限：访问特定数据的权限
- 操作权限：执行特定操作的权限

## 4. 数据模型设计

### 4.1 用户实体（User）

```typescript
interface User {
  id: string;             // 用户唯一标识
  username: string;       // 用户名
  email: string;          // 电子邮箱
  phone?: string;         // 手机号码（可选）
  passwordHash: string;   // 密码哈希值
  salt: string;           // 密码盐值
  firstName?: string;     // 名字
  lastName?: string;      // 姓氏
  avatarUrl?: string;     // 头像URL
  bio?: string;           // 个人简介
  affiliation?: string;   // 所属机构
  position?: string;      // 职位
  researchAreas?: string[]; // 研究领域
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  lastLoginAt?: Date;     // 最后登录时间
  status: UserStatus;     // 用户状态
  isEmailVerified: boolean; // 邮箱是否已验证
  isPhoneVerified?: boolean; // 手机号是否已验证
  preferences?: Record<string, any>; // 用户偏好设置
}

enum UserStatus {
  ACTIVE = 'ACTIVE',      // 活跃状态
  INACTIVE = 'INACTIVE',  // 非活跃状态
  SUSPENDED = 'SUSPENDED', // 被暂停
  DELETED = 'DELETED'     // 已删除
}
```

### 4.2 角色实体（Role）

```typescript
interface Role {
  id: string;             // 角色唯一标识
  name: string;           // 角色名称
  description?: string;   // 角色描述
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  permissions: string[];  // 权限列表（权限ID）
}
```

### 4.3 用户角色关联（UserRole）

```typescript
interface UserRole {
  userId: string;         // 用户ID
  roleId: string;         // 角色ID
  createdAt: Date;        // 创建时间
}
```

### 4.4 权限实体（Permission）

```typescript
interface Permission {
  id: string;             // 权限唯一标识
  name: string;           // 权限名称
  description?: string;   // 权限描述
  resource: string;       // 资源类型
  action: string;         // 操作类型
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
}
```

## 5. API 接口设计

### 5.1 RESTful API

#### 用户管理

- **创建用户（注册）**
  - 路径：`POST /api/v1/users`
  - 请求体：`{ username, email, password, firstName?, lastName? }`
  - 响应：`201 Created` with user data
  - 权限：公开

- **获取用户列表**
  - 路径：`GET /api/v1/users`
  - 查询参数：`page`, `pageSize`, `status?`, `search?`
  - 响应：`200 OK` with user list and pagination info
  - 权限：管理员

- **获取用户详情**
  - 路径：`GET /api/v1/users/{userId}`
  - 响应：`200 OK` with user details
  - 权限：用户自己或管理员

- **更新用户信息**
  - 路径：`PUT /api/v1/users/{userId}`
  - 请求体：`{ firstName?, lastName?, bio?, affiliation?, position?, researchAreas? }`
  - 响应：`200 OK` with updated user data
  - 权限：用户自己或管理员

- **删除用户**
  - 路径：`DELETE /api/v1/users/{userId}`
  - 响应：`204 No Content`
  - 权限：用户自己或管理员

#### 认证授权

- **用户登录**
  - 路径：`POST /api/v1/auth/login`
  - 请求体：`{ email, password }`
  - 响应：`200 OK` with JWT token and user info
  - 权限：公开

- **用户登出**
  - 路径：`POST /api/v1/auth/logout`
  - 响应：`200 OK`
  - 权限：已登录用户

- **刷新令牌**
  - 路径：`POST /api/v1/auth/refresh`
  - 请求体：`{ refreshToken }`
  - 响应：`200 OK` with new JWT token
  - 权限：已登录用户

- **验证令牌**
  - 路径：`GET /api/v1/auth/verify`
  - 请求头：`Authorization: Bearer {token}`
  - 响应：`200 OK` with user info
  - 权限：已登录用户

#### 角色管理

- **创建角色**
  - 路径：`POST /api/v1/roles`
  - 请求体：`{ name, description?, permissions? }`
  - 响应：`201 Created` with role data
  - 权限：超级管理员

- **获取角色列表**
  - 路径：`GET /api/v1/roles`
  - 查询参数：`page`, `pageSize`, `search?`
  - 响应：`200 OK` with role list
  - 权限：管理员

- **获取角色详情**
  - 路径：`GET /api/v1/roles/{roleId}`
  - 响应：`200 OK` with role details
  - 权限：管理员

- **更新角色**
  - 路径：`PUT /api/v1/roles/{roleId}`
  - 请求体：`{ name?, description?, permissions? }`
  - 响应：`200 OK` with updated role data
  - 权限：超级管理员

- **删除角色**
  - 路径：`DELETE /api/v1/roles/{roleId}`
  - 响应：`204 No Content`
  - 权限：超级管理员

#### 权限管理

- **获取权限列表**
  - 路径：`GET /api/v1/permissions`
  - 查询参数：`resource?`, `action?`
  - 响应：`200 OK` with permission list
  - 权限：管理员

- **检查权限**
  - 路径：`GET /api/v1/users/{userId}/permissions/{permissionId}`
  - 响应：`200 OK` with permission status
  - 权限：管理员

## 6. 服务实现

### 6.1 服务层实现

```typescript
// 用户服务实现示例
class UserService {
  private userRepository: UserRepository;
  private roleRepository: RoleRepository;
  private authService: AuthService;

  constructor(
    userRepository: UserRepository,
    roleRepository: RoleRepository,
    authService: AuthService
  ) {
    this.userRepository = userRepository;
    this.roleRepository = roleRepository;
    this.authService = authService;
  }

  // 创建用户
  async createUser(createUserDto: CreateUserDto): Promise<User> {
    // 验证邮箱是否已存在
    const existingUser = await this.userRepository.findByEmail(createUserDto.email);
    if (existingUser) {
      throw new ConflictException('Email already in use');
    }

    // 密码加密
    const { passwordHash, salt } = this.authService.generatePasswordHash(createUserDto.password);

    // 创建用户
    const user: User = {
      id: uuidv4(),
      username: createUserDto.username,
      email: createUserDto.email,
      passwordHash,
      salt,
      firstName: createUserDto.firstName,
      lastName: createUserDto.lastName,
      createdAt: new Date(),
      updatedAt: new Date(),
      status: UserStatus.ACTIVE,
      isEmailVerified: false,
      researchAreas: createUserDto.researchAreas || []
    };

    // 保存用户
    const savedUser = await this.userRepository.save(user);

    // 发送验证邮件
    // ...

    return savedUser;
  }

  // 获取用户详情
  async getUserById(userId: string): Promise<User> {
    const user = await this.userRepository.findById(userId);
    if (!user) {
      throw new NotFoundException('User not found');
    }
    return user;
  }

  // 更新用户信息
  async updateUser(userId: string, updateUserDto: UpdateUserDto): Promise<User> {
    const user = await this.getUserById(userId);

    // 更新用户信息
    if (updateUserDto.firstName) user.firstName = updateUserDto.firstName;
    if (updateUserDto.lastName) user.lastName = updateUserDto.lastName;
    if (updateUserDto.bio) user.bio = updateUserDto.bio;
    if (updateUserDto.affiliation) user.affiliation = updateUserDto.affiliation;
    if (updateUserDto.position) user.position = updateUserDto.position;
    if (updateUserDto.researchAreas) user.researchAreas = updateUserDto.researchAreas;
    if (updateUserDto.preferences) user.preferences = updateUserDto.preferences;
    user.updatedAt = new Date();

    return this.userRepository.save(user);
  }

  // 分配角色给用户
  async assignRoleToUser(userId: string, roleId: string): Promise<void> {
    const user = await this.getUserById(userId);
    const role = await this.roleRepository.findById(roleId);
    
    if (!role) {
      throw new NotFoundException('Role not found');
    }

    await this.userRepository.assignRole(userId, roleId);
  }

  // 验证用户权限
  async hasPermission(userId: string, permissionName: string): Promise<boolean> {
    const userRoles = await this.userRepository.getUserRoles(userId);
    const roleIds = userRoles.map(ur => ur.roleId);
    
    for (const roleId of roleIds) {
      const role = await this.roleRepository.findById(roleId);
      if (role && role.permissions.includes(permissionName)) {
        return true;
      }
    }
    
    return false;
  }

  // 其他用户服务方法...
}
```

### 6.2 控制器实现

```typescript
// 用户控制器实现示例
class UserController {
  private userService: UserService;
  private authService: AuthService;
  
  constructor(userService: UserService, authService: AuthService) {
    this.userService = userService;
    this.authService = authService;
  }

  // 用户注册
  @Post('/users')
  async register(@Body() createUserDto: CreateUserDto, @Response() res) {
    try {
      const user = await this.userService.createUser(createUserDto);
      res.status(HttpStatus.CREATED).json(user);
    } catch (error) {
      if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 用户登录
  @Post('/auth/login')
  async login(@Body() loginDto: LoginDto, @Response() res) {
    try {
      const { email, password } = loginDto;
      const user = await this.authService.authenticate(email, password);
      const token = this.authService.generateToken(user);
      
      res.status(HttpStatus.OK).json({
        user,
        token
      });
    } catch (error) {
      if (error instanceof UnauthorizedException) {
        res.status(HttpStatus.UNAUTHORIZED).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 获取用户详情
  @Get('/users/:userId')
  @AuthGuard()
  async getUserById(@Param('userId') userId: string, @Request() req, @Response() res) {
    try {
      // 检查权限：用户只能查看自己的信息，管理员可以查看所有用户信息
      const currentUser = req.user;
      if (currentUser.id !== userId && !await this.userService.hasPermission(currentUser.id, 'view:all_users')) {
        throw new ForbiddenException('You do not have permission to view this user');
      }
      
      const user = await this.userService.getUserById(userId);
      res.status(HttpStatus.OK).json(user);
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

  // 其他控制器方法...
}
```

## 7. 安全考虑

### 7.1 密码安全

- 密码存储：使用bcrypt或Argon2等算法对密码进行哈希处理，添加随机盐值
- 密码策略：强制实施强密码策略（长度、复杂度要求）
- 密码传输：所有密码传输必须通过HTTPS加密
- 密码重置：实现安全的密码重置流程，包括身份验证和链接过期机制

### 7.2 会话安全

- 令牌过期：设置合理的令牌过期时间
- 令牌存储：建议客户端使用安全的方式存储令牌（如HTTP-only cookie）
- 会话失效：用户注销或修改密码后，立即失效相关会话
- 防止CSRF：实施CSRF防护措施

### 7.3 数据隐私

- 数据加密：敏感用户数据（如身份证号、银行卡信息）在存储时进行加密
- 最小权限原则：用户服务只访问完成其功能所需的最小数据集合
- 数据脱敏：在日志和监控中对敏感信息进行脱敏处理
- 合规性：遵循相关的数据隐私法规（如GDPR、CCPA等）

## 8. 性能优化

### 8.1 数据库优化

- 索引优化：为常用查询字段创建索引
- 查询优化：优化复杂查询，避免全表扫描
- 连接池：使用数据库连接池，减少连接建立和关闭的开销
- 缓存策略：缓存频繁访问的用户数据

### 8.2 服务性能优化

- 请求合并：合并相似请求，减少数据库访问次数
- 异步处理：非关键操作使用异步处理，提高响应速度
- 限流措施：实施API限流，防止系统过载
- 监控与调优：持续监控服务性能，及时进行优化

## 9. 总结

用户服务是OpenMind Lab系统的重要组成部分，负责用户身份管理、认证授权、角色权限管理等核心功能。本文档详细描述了用户服务的设计方案，包括功能架构、数据模型、API接口、服务实现、安全考虑和性能优化等方面。通过合理的设计和实现，用户服务能够提供安全、高效、可靠的用户管理功能，为OpenMind Lab的整体运行提供坚实的基础。未来，用户服务可以根据业务需求进行扩展，如支持更多的认证方式、增强用户资料管理功能、优化用户体验等。
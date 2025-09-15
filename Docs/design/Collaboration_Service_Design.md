# OpenMind Lab 协作服务设计文档

## 1. 模块概述

协作服务（Collaboration Service）是 OpenMind Lab 的核心服务之一，负责提供实时交流、项目协作、文档协作和团队协作等功能。本文档详细描述协作服务的设计方案、API 接口、数据模型和实现细节。

## 2. 功能架构

```
┌────────────────────────────────────────────────────────────────────┐
│                          协作服务架构                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  消息通信   │  │  实时协作   │  │  协作文档   │  │  团队协作   │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │                │                │                │        │
│  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐ │
│  │  聊天功能   │  │  协同编辑   │  │  版本控制   │  │  权限管理   │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

## 3. 核心功能模块

### 3.1 消息通信

**主要功能：**
- 一对一聊天：用户之间的私密聊天
- 群组聊天：项目内或特定群组的聊天
- 频道聊天：基于主题的频道聊天
- 文件共享：在聊天中共享文件
- 消息状态：显示消息的发送、已读状态
- 消息通知：新消息提醒
- 消息搜索：搜索历史消息
- 消息归档：归档重要消息

**消息类型：**
- 文本消息：普通文本内容
- 系统消息：系统自动发送的通知
- 文件消息：共享的文件
- 数据消息：共享的数据引用
- 任务消息：任务相关的通知
- 提及消息：提及特定用户的消息

### 3.2 实时协作

**主要功能：**
- 实时编辑：多用户同时编辑同一份文档
- 操作同步：同步用户的编辑操作
- 冲突解决：自动解决编辑冲突
- 协作状态：显示其他用户的编辑状态和位置
- 版本历史：记录文档的编辑历史
- 撤销/重做：支持协作编辑的撤销和重做操作

**协作模式：**
- 实时协同编辑：多用户同时编辑
- 锁定编辑：特定区域的锁定编辑
- 建议模式：提出编辑建议，由作者确认
- 评论模式：在文档中添加评论和批注

### 3.3 协作文档

**主要功能：**
- 文档创建：创建协作文档
- 文档编辑：在线编辑文档内容
- 文档组织：管理文档的分类和结构
- 文档版本：管理文档的版本历史
- 文档分享：分享文档给团队成员
- 文档权限：设置文档的访问和编辑权限
- 文档导出：导出文档为不同格式
- 模板管理：管理常用文档模板

**文档类型：**
- 富文本文档：支持格式、图片、表格等
- Markdown文档：支持Markdown语法
- 代码文档：支持代码高亮和语法检查
- 演示文档：支持幻灯片演示
- 电子表格：支持数据计算和分析
- 白板文档：支持自由绘制和草图

### 3.4 团队协作

**主要功能：**
- 团队创建：创建和管理团队
- 成员管理：添加、移除和管理团队成员
- 角色分配：为成员分配不同的角色和权限
- 团队日历：共享团队日程和事件
- 团队文档：共享团队文档和资源
- 团队通知：团队范围内的通知
- 团队活动：记录团队活动历史
- 团队设置：配置团队的各项设置

**团队类型：**
- 项目团队：针对特定项目的团队
- 部门团队：基于组织结构的部门团队
- 兴趣小组：基于共同兴趣的小组
- 临时团队：针对短期任务的临时团队

### 3.5 聊天功能

**主要功能：**
- 聊天会话管理：创建、列出和关闭聊天会话
- 消息发送：发送各种类型的消息
- 消息接收：接收并展示消息
- 消息管理：编辑、删除和转发消息
- 表情反应：对消息添加表情反应
- 消息引用：引用之前的消息
- 离线消息：处理用户离线时的消息
- 消息标记：标记重要或未读的消息

### 3.6 协同编辑

**主要功能：**
- 实时同步：实时同步用户的编辑操作
- 光标追踪：显示其他用户的光标位置和选择范围
- 用户标识：标识不同用户的编辑内容
- 操作转换：使用操作转换算法解决冲突
- 编辑历史：记录和回放编辑历史
- 离线编辑：支持用户离线时的编辑
- 变更合并：合并离线编辑的变更
- 编辑锁：防止关键部分的并发编辑

### 3.7 版本控制

**主要功能：**
- 版本创建：自动或手动创建文档版本
- 版本管理：管理和查看所有版本
- 版本对比：对比不同版本的差异
- 版本回滚：回滚到之前的版本
- 版本标记：为重要版本添加标记和描述
- 版本分支：支持版本分支管理
- 版本合并：合并不同分支的版本

### 3.8 权限管理

**主要功能：**
- 角色定义：定义不同的协作角色
- 权限分配：为角色分配具体权限
- 访问控制：基于角色和权限控制访问
- 权限继承：支持权限的继承机制
- 动态权限：根据上下文动态调整权限
- 权限审计：记录权限变更和访问日志

## 4. 数据模型设计

### 4.1 协作消息实体（CollaborationMessage）

```typescript
interface CollaborationMessage {
  id: string;             // 消息唯一标识
  senderId: string;       // 发送者ID
  receiverId?: string;    // 接收者ID（一对一聊天）
  conversationId: string; // 会话ID
  type: MessageType;      // 消息类型
  content: string;        // 消息内容
  status: MessageStatus;  // 消息状态
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  readAt?: Date;          // 阅读时间
  attachments?: MessageAttachment[]; // 附件列表
  metadata?: Record<string, any>; // 元数据
  parentMessageId?: string; // 父消息ID（回复消息）
}

enum MessageType {
  TEXT = 'TEXT',          // 文本消息
  SYSTEM = 'SYSTEM',      // 系统消息
  FILE = 'FILE',          // 文件消息
  DATA = 'DATA',          // 数据消息
  TASK = 'TASK',          // 任务消息
  MENTION = 'MENTION'     // 提及消息
}

enum MessageStatus {
  SENT = 'SENT',          // 已发送
  DELIVERED = 'DELIVERED', // 已送达
  READ = 'READ',          // 已读
  FAILED = 'FAILED'       // 发送失败
}

interface MessageAttachment {
  id: string;             // 附件唯一标识
  name: string;           // 附件名称
  type: string;           // 附件类型
  size: number;           // 附件大小（字节）
  url: string;            // 附件URL
}
```

### 4.2 对话实体（Conversation）

```typescript
interface Conversation {
  id: string;             // 对话唯一标识
  type: ConversationType; // 对话类型
  name?: string;          // 对话名称（群组/频道）
  participants: ConversationParticipant[]; // 参与者列表
  lastMessageId?: string; // 最后一条消息ID
  lastMessageAt?: Date;   // 最后一条消息时间
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  avatar?: string;        // 对话头像
  metadata?: Record<string, any>; // 元数据
  isArchived: boolean;    // 是否已归档
  isMuted: boolean;       // 是否静音
}

enum ConversationType {
  PRIVATE = 'PRIVATE',    // 私人对话
  GROUP = 'GROUP',        // 群组对话
  CHANNEL = 'CHANNEL'     // 频道对话
}

interface ConversationParticipant {
  userId: string;         // 用户ID
  joinedAt: Date;         // 加入时间
  role: ParticipantRole;  // 参与者角色
  lastReadMessageId?: string; // 最后阅读的消息ID
  lastReadAt?: Date;      // 最后阅读时间
  isActive: boolean;      // 是否活跃
}

enum ParticipantRole {
  ADMIN = 'ADMIN',        // 管理员
  MODERATOR = 'MODERATOR', // 版主
  MEMBER = 'MEMBER'       // 普通成员
}
```

### 4.3 协作文档实体（CollaborationDocument）

```typescript
interface CollaborationDocument {
  id: string;             // 文档唯一标识
  name: string;           // 文档名称
  type: DocumentType;     // 文档类型
  content?: string;       // 文档内容（初始版本）
  ownerId: string;        // 所有者ID
  projectId?: string;     // 所属项目ID
  folderId?: string;      // 所属文件夹ID
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  lastEditedById?: string; // 最后编辑者ID
  lastEditedAt?: Date;    // 最后编辑时间
  version: number;        // 当前版本号
  status: DocumentStatus; // 文档状态
  tags?: string[];        // 文档标签
  metadata?: Record<string, any>; // 元数据
  templateId?: string;    // 模板ID
}

enum DocumentType {
  RICH_TEXT = 'RICH_TEXT', // 富文本文档
  MARKDOWN = 'MARKDOWN',   // Markdown文档
  CODE = 'CODE',           // 代码文档
  PRESENTATION = 'PRESENTATION', // 演示文档
  SPREADSHEET = 'SPREADSHEET',   // 电子表格
  WHITEBOARD = 'WHITEBOARD'      // 白板文档
}

enum DocumentStatus {
  DRAFT = 'DRAFT',        // 草稿
  PUBLISHED = 'PUBLISHED', // 已发布
  ARCHIVED = 'ARCHIVED',  // 已归档
  DELETED = 'DELETED'     // 已删除
}
```

### 4.4 文档版本实体（DocumentVersion）

```typescript
interface DocumentVersion {
  id: string;             // 版本唯一标识
  documentId: string;     // 文档ID
  versionNumber: number;  // 版本号
  content?: string;       // 文档内容
  snapshot?: string;      // 文档快照（二进制或引用）
  editorId: string;       // 编辑者ID
  editedAt: Date;         // 编辑时间
  description?: string;   // 版本描述
  isMajor: boolean;       // 是否主版本
  changes?: string[];     // 变更摘要
  previousVersionId?: string; // 上一版本ID
}
```

### 4.5 协作会话实体（CollaborationSession）

```typescript
interface CollaborationSession {
  id: string;             // 会话唯一标识
  documentId: string;     // 文档ID
  participants: CollaborationSessionParticipant[]; // 会话参与者
  startedAt: Date;        // 开始时间
  endedAt?: Date;         // 结束时间
  status: SessionStatus;  // 会话状态
  lockStatus: LockStatus; // 锁定状态
  operations?: Operation[]; // 操作列表（实时编辑时使用）
}

enum SessionStatus {
  ACTIVE = 'ACTIVE',      // 活跃
  INACTIVE = 'INACTIVE',  // 不活跃
  CLOSED = 'CLOSED'       // 已关闭
}

enum LockStatus {
  UNLOCKED = 'UNLOCKED',  // 未锁定
  LOCKED = 'LOCKED',      // 已锁定
  PARTIALLY_LOCKED = 'PARTIALLY_LOCKED' // 部分锁定
}

interface CollaborationSessionParticipant {
  userId: string;         // 用户ID
  joinedAt: Date;         // 加入时间
  leftAt?: Date;          // 离开时间
  currentPosition?: string; // 当前位置（光标位置等）
  status: ParticipantStatus; // 参与者状态
  color?: string;         // 参与者颜色标识
}

enum ParticipantStatus {
  ONLINE = 'ONLINE',      // 在线
  AWAY = 'AWAY',          // 离开
  OFFLINE = 'OFFLINE'     // 离线
}

interface Operation {
  id: string;             // 操作唯一标识
  userId: string;         // 用户ID
  type: OperationType;    // 操作类型
  position?: string;      // 操作位置
  data?: any;             // 操作数据
  timestamp: Date;        // 操作时间
  version?: number;       // 版本号
}

enum OperationType {
  INSERT = 'INSERT',      // 插入
  DELETE = 'DELETE',      // 删除
  UPDATE = 'UPDATE',      // 更新
  MOVE = 'MOVE'           // 移动
}
```

### 4.6 团队实体（Team）

```typescript
interface Team {
  id: string;             // 团队唯一标识
  name: string;           // 团队名称
  description?: string;   // 团队描述
  ownerId: string;        // 所有者ID
  members: TeamMember[];  // 团队成员
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  avatar?: string;        // 团队头像
  settings?: TeamSettings; // 团队设置
  tags?: string[];        // 团队标签
}

interface TeamMember {
  userId: string;         // 用户ID
  role: TeamRole;         // 团队角色
  joinedAt: Date;         // 加入时间
  lastActiveAt?: Date;    // 最后活跃时间
  status: MemberStatus;   // 成员状态
}

enum TeamRole {
  OWNER = 'OWNER',        // 所有者
  ADMIN = 'ADMIN',        // 管理员
  MODERATOR = 'MODERATOR', // 版主
  MEMBER = 'MEMBER',      // 普通成员
  GUEST = 'GUEST'         // 访客
}

enum MemberStatus {
  ACTIVE = 'ACTIVE',      // 活跃
  INACTIVE = 'INACTIVE',  // 不活跃
  PENDING = 'PENDING'     // 待处理
}

interface TeamSettings {
  joinApprovalRequired: boolean; // 加入是否需要审批
  publicVisibility: boolean;     // 是否公开可见
  defaultPermissions?: Record<string, any>; // 默认权限设置
  notificationSettings?: NotificationSettings; // 通知设置
}

interface NotificationSettings {
  emailEnabled: boolean;  // 是否启用邮件通知
  pushEnabled: boolean;   // 是否启用推送通知
  desktopEnabled: boolean; // 是否启用桌面通知
  mentionOnly: boolean;   // 是否仅在被提及时通知
}
```

## 5. API 接口设计

### 5.1 RESTful API

#### 消息通信

- **发送消息**
  - 路径：`POST /api/v1/collaboration/messages`
  - 请求体：`{ receiverId?, conversationId, type, content, attachments?, parentMessageId? }`
  - 响应：`201 Created` with message info
  - 权限：已登录用户

- **获取消息列表**
  - 路径：`GET /api/v1/collaboration/conversations/{conversationId}/messages`
  - 查询参数：`page`, `pageSize`, `beforeId?`, `afterId?`, `type?`
  - 响应：`200 OK` with messages list
  - 权限：会话参与者

- **获取消息详情**
  - 路径：`GET /api/v1/collaboration/messages/{messageId}`
  - 响应：`200 OK` with message details
  - 权限：消息相关方

- **更新消息状态**
  - 路径：`PUT /api/v1/collaboration/messages/{messageId}/status`
  - 请求体：`{ status }`
  - 响应：`200 OK` with updated message
  - 权限：消息接收者

- **创建对话**
  - 路径：`POST /api/v1/collaboration/conversations`
  - 请求体：`{ type, name?, participants, avatar? }`
  - 响应：`201 Created` with conversation info
  - 权限：已登录用户

- **获取对话列表**
  - 路径：`GET /api/v1/collaboration/conversations`
  - 查询参数：`page`, `pageSize`, `type?`, `archived?`, `muted?`
  - 响应：`200 OK` with conversations list
  - 权限：已登录用户

- **获取对话详情**
  - 路径：`GET /api/v1/collaboration/conversations/{conversationId}`
  - 响应：`200 OK` with conversation details
  - 权限：会话参与者

#### 协作文档

- **创建文档**
  - 路径：`POST /api/v1/collaboration/documents`
  - 请求体：`{ name, type, content?, projectId?, folderId?, templateId?, tags? }`
  - 响应：`201 Created` with document info
  - 权限：已登录用户

- **获取文档列表**
  - 路径：`GET /api/v1/collaboration/documents`
  - 查询参数：`page`, `pageSize`, `projectId?`, `type?`, `folderId?`, `search?`, `sort?`
  - 响应：`200 OK` with documents list
  - 权限：已登录用户（根据文档可见性过滤）

- **获取文档详情**
  - 路径：`GET /api/v1/collaboration/documents/{documentId}`
  - 响应：`200 OK` with document details
  - 权限：文档所有者、项目成员或具有访问权限的用户

- **更新文档**
  - 路径：`PUT /api/v1/collaboration/documents/{documentId}`
  - 请求体：`{ name?, content?, status?, tags? }`
  - 响应：`200 OK` with updated document
  - 权限：文档所有者或具有编辑权限的用户

- **删除文档**
  - 路径：`DELETE /api/v1/collaboration/documents/{documentId}`
  - 响应：`204 No Content`
  - 权限：文档所有者或项目管理员

- **获取文档版本**
  - 路径：`GET /api/v1/collaboration/documents/{documentId}/versions`
  - 查询参数：`page`, `pageSize`, `isMajor?`
  - 响应：`200 OK` with versions list
  - 权限：文档所有者或具有访问权限的用户

- **回滚文档版本**
  - 路径：`POST /api/v1/collaboration/documents/{documentId}/versions/{versionId}/rollback`
  - 响应：`200 OK` with updated document
  - 权限：文档所有者或具有编辑权限的用户

#### 团队协作

- **创建团队**
  - 路径：`POST /api/v1/collaboration/teams`
  - 请求体：`{ name, description?, avatar?, settings? }`
  - 响应：`201 Created` with team info
  - 权限：已登录用户

- **获取团队列表**
  - 路径：`GET /api/v1/collaboration/teams`
  - 查询参数：`page`, `pageSize`, `search?`
  - 响应：`200 OK` with teams list
  - 权限：已登录用户

- **获取团队详情**
  - 路径：`GET /api/v1/collaboration/teams/{teamId}`
  - 响应：`200 OK` with team details
  - 权限：团队成员

- **更新团队**
  - 路径：`PUT /api/v1/collaboration/teams/{teamId}`
  - 请求体：`{ name?, description?, avatar?, settings? }`
  - 响应：`200 OK` with updated team
  - 权限：团队所有者或管理员

- **管理团队成员**
  - 路径：`POST /api/v1/collaboration/teams/{teamId}/members`
  - 请求体：`{ userId, role }`
  - 响应：`201 Created` with member info
  - 权限：团队所有者或管理员

- **更新团队成员角色**
  - 路径：`PUT /api/v1/collaboration/teams/{teamId}/members/{userId}`
  - 请求体：`{ role }`
  - 响应：`200 OK` with updated member
  - 权限：团队所有者或管理员

- **移除团队成员**
  - 路径：`DELETE /api/v1/collaboration/teams/{teamId}/members/{userId}`
  - 响应：`204 No Content`
  - 权限：团队所有者或管理员

## 6. WebSocket API

### 6.1 连接与认证

```javascript
// 客户端连接示例
const socket = new WebSocket(`wss://api.openmindlab.com/ws/collaboration?token=${authToken}`);

// 连接建立事件
socket.onopen = (event) => {
  console.log('WebSocket connection established');
};

// 连接关闭事件
socket.onclose = (event) => {
  console.log('WebSocket connection closed');
};

// 错误事件
socket.onerror = (error) => {
  console.error('WebSocket error:', error);
};

// 接收消息事件
socket.onmessage = (event) => {
  const data = JSON.parse(event.data);
  handleWebSocketMessage(data);
};
```

### 6.2 消息通信

**发送消息**
```json
{
  "type": "SEND_MESSAGE",
  "payload": {
    "receiverId": "user123",
    "conversationId": "conv456",
    "type": "TEXT",
    "content": "Hello, world!",
    "attachments": []
  }
}
```

**接收消息**
```json
{
  "type": "MESSAGE_RECEIVED",
  "payload": {
    "id": "msg789",
    "senderId": "user123",
    "conversationId": "conv456",
    "type": "TEXT",
    "content": "Hello, world!",
    "status": "SENT",
    "createdAt": "2023-05-20T12:00:00Z",
    "updatedAt": "2023-05-20T12:00:00Z"
  }
}
```

### 6.3 实时协作编辑

**加入协作会话**
```json
{
  "type": "JOIN_SESSION",
  "payload": {
    "documentId": "doc123"
  }
}
```

**离开协作会话**
```json
{
  "type": "LEAVE_SESSION",
  "payload": {
    "documentId": "doc123"
  }
}
```

**发送编辑操作**
```json
{
  "type": "SEND_OPERATION",
  "payload": {
    "documentId": "doc123",
    "operation": {
      "type": "INSERT",
      "position": "10:0",
      "data": "Hello",
      "timestamp": "2023-05-20T12:00:00Z"
    }
  }
}
```

**接收编辑操作**
```json
{
  "type": "OPERATION_RECEIVED",
  "payload": {
    "documentId": "doc123",
    "userId": "user123",
    "operation": {
      "type": "INSERT",
      "position": "10:0",
      "data": "Hello",
      "timestamp": "2023-05-20T12:00:00Z"
    }
  }
}
```

**更新光标位置**
```json
{
  "type": "UPDATE_CURSOR",
  "payload": {
    "documentId": "doc123",
    "position": "10:5"
  }
}
```

**接收光标位置更新**
```json
{
  "type": "CURSOR_UPDATED",
  "payload": {
    "documentId": "doc123",
    "userId": "user123",
    "position": "10:5"
  }
}
```

## 7. 服务实现

### 7.1 服务层实现

```typescript
// 协作服务实现示例
class CollaborationService {
  private messageRepository: MessageRepository;
  private conversationRepository: ConversationRepository;
  private documentRepository: DocumentRepository;
  private documentVersionRepository: DocumentVersionRepository;
  private sessionRepository: CollaborationSessionRepository;
  private teamRepository: TeamRepository;
  private userService: UserService;
  private notificationService: NotificationService;
  private webSocketService: WebSocketService;

  constructor(
    messageRepository: MessageRepository,
    conversationRepository: ConversationRepository,
    documentRepository: DocumentRepository,
    documentVersionRepository: DocumentVersionRepository,
    sessionRepository: CollaborationSessionRepository,
    teamRepository: TeamRepository,
    userService: UserService,
    notificationService: NotificationService,
    webSocketService: WebSocketService
  ) {
    this.messageRepository = messageRepository;
    this.conversationRepository = conversationRepository;
    this.documentRepository = documentRepository;
    this.documentVersionRepository = documentVersionRepository;
    this.sessionRepository = sessionRepository;
    this.teamRepository = teamRepository;
    this.userService = userService;
    this.notificationService = notificationService;
    this.webSocketService = webSocketService;
  }

  // 发送消息
  async sendMessage(sendMessageDto: SendMessageDto, userId: string): Promise<CollaborationMessage> {
    // 验证对话存在
    const conversation = await this.conversationRepository.findById(sendMessageDto.conversationId);
    if (!conversation) {
      throw new NotFoundException('Conversation not found');
    }

    // 验证用户是否为对话参与者
    const isParticipant = conversation.participants.some(p => p.userId === userId);
    if (!isParticipant) {
      throw new ForbiddenException('You are not a participant of this conversation');
    }

    // 创建消息
    const message: CollaborationMessage = {
      id: uuidv4(),
      senderId: userId,
      receiverId: sendMessageDto.receiverId,
      conversationId: sendMessageDto.conversationId,
      type: sendMessageDto.type || MessageType.TEXT,
      content: sendMessageDto.content,
      status: MessageStatus.SENT,
      createdAt: new Date(),
      updatedAt: new Date(),
      attachments: sendMessageDto.attachments || [],
      metadata: sendMessageDto.metadata || {},
      parentMessageId: sendMessageDto.parentMessageId
    };

    // 保存消息
    const savedMessage = await this.messageRepository.save(message);

    // 更新对话的最后消息
    conversation.lastMessageId = savedMessage.id;
    conversation.lastMessageAt = new Date();
    await this.conversationRepository.save(conversation);

    // 发送WebSocket通知给其他参与者
    const otherParticipants = conversation.participants
      .filter(p => p.userId !== userId)
      .map(p => p.userId);

    if (otherParticipants.length > 0) {
      await this.webSocketService.sendToUsers(otherParticipants, {
        type: 'MESSAGE_RECEIVED',
        payload: savedMessage
      });

      // 发送离线通知
      await this.notificationService.sendNotification(
        otherParticipants,
        'New Message',
        `You have a new message from ${userId}`,
        {
          conversationId: savedMessage.conversationId,
          messageId: savedMessage.id
        }
      );
    }

    return savedMessage;
  }

  // 创建文档
  async createDocument(createDocumentDto: CreateDocumentDto, userId: string): Promise<CollaborationDocument> {
    // 验证用户是否有权限在项目中创建文档（如果指定了项目）
    if (createDocumentDto.projectId) {
      // 假设存在一个projectService来检查项目权限
      // await projectService.checkProjectPermission(createDocumentDto.projectId, userId, 'CREATE_DOCUMENT');
    }

    // 创建文档
    const document: CollaborationDocument = {
      id: uuidv4(),
      name: createDocumentDto.name,
      type: createDocumentDto.type || DocumentType.RICH_TEXT,
      content: createDocumentDto.content || '',
      ownerId: userId,
      projectId: createDocumentDto.projectId,
      folderId: createDocumentDto.folderId,
      createdAt: new Date(),
      updatedAt: new Date(),
      lastEditedById: userId,
      lastEditedAt: new Date(),
      version: 1,
      status: DocumentStatus.DRAFT,
      tags: createDocumentDto.tags || [],
      metadata: createDocumentDto.metadata || {},
      templateId: createDocumentDto.templateId
    };

    // 保存文档
    const savedDocument = await this.documentRepository.save(document);

    // 创建初始版本
    await this.createDocumentVersion(savedDocument.id, userId, 'Initial version', true);

    // 通知相关用户
    if (savedDocument.projectId) {
      // 发送项目文档创建通知
      // await this.notificationService.sendProjectNotification(savedDocument.projectId, userId, 'DOCUMENT_CREATED', savedDocument.id);
    }

    return savedDocument;
  }

  // 更新文档
  async updateDocument(documentId: string, updateDocumentDto: UpdateDocumentDto, userId: string): Promise<CollaborationDocument> {
    // 验证文档存在
    const document = await this.documentRepository.findById(documentId);
    if (!document) {
      throw new NotFoundException('Document not found');
    }

    // 验证用户是否有权限编辑文档
    if (!await this.hasDocumentEditPermission(document, userId)) {
      throw new ForbiddenException('You do not have permission to edit this document');
    }

    // 更新文档信息
    if (updateDocumentDto.name !== undefined) {
      document.name = updateDocumentDto.name;
    }
    if (updateDocumentDto.content !== undefined) {
      document.content = updateDocumentDto.content;
      document.version += 1;
      document.lastEditedById = userId;
      document.lastEditedAt = new Date();
      
      // 创建新版本
      await this.createDocumentVersion(document.id, userId, updateDocumentDto.versionDescription);
    }
    if (updateDocumentDto.status !== undefined) {
      document.status = updateDocumentDto.status;
    }
    if (updateDocumentDto.tags !== undefined) {
      document.tags = updateDocumentDto.tags;
    }
    if (updateDocumentDto.metadata !== undefined) {
      document.metadata = updateDocumentDto.metadata;
    }
    document.updatedAt = new Date();

    // 保存文档
    const updatedDocument = await this.documentRepository.save(document);

    // 通知其他会话参与者有更新
    const activeSessions = await this.sessionRepository.findByDocumentId(documentId);
    for (const session of activeSessions) {
      const otherParticipants = session.participants
        .filter(p => p.userId !== userId)
        .map(p => p.userId);
      
      if (otherParticipants.length > 0) {
        await this.webSocketService.sendToUsers(otherParticipants, {
          type: 'DOCUMENT_UPDATED',
          payload: {
            documentId: documentId,
            version: document.version,
            lastEditedById: userId
          }
        });
      }
    }

    return updatedDocument;
  }

  // 创建文档版本
  private async createDocumentVersion(
    documentId: string, 
    userId: string, 
    description?: string, 
    isMajor: boolean = false
  ): Promise<DocumentVersion> {
    const document = await this.documentRepository.findById(documentId);
    if (!document) {
      throw new NotFoundException('Document not found');
    }

    const version: DocumentVersion = {
      id: uuidv4(),
      documentId: documentId,
      versionNumber: document.version,
      content: document.content,
      editorId: userId,
      editedAt: new Date(),
      description: description || `Version ${document.version}`,
      isMajor: isMajor
    };

    return await this.documentVersionRepository.save(version);
  }

  // 加入协作会话
  async joinCollaborationSession(documentId: string, userId: string): Promise<CollaborationSession> {
    // 验证文档存在
    const document = await this.documentRepository.findById(documentId);
    if (!document) {
      throw new NotFoundException('Document not found');
    }

    // 验证用户是否有权限访问文档
    if (!await this.hasDocumentAccessPermission(document, userId)) {
      throw new ForbiddenException('You do not have permission to access this document');
    }

    // 查找或创建会话
    let session = await this.sessionRepository.findActiveByDocumentId(documentId);
    if (!session) {
      session = {
        id: uuidv4(),
        documentId: documentId,
        participants: [],
        startedAt: new Date(),
        status: SessionStatus.ACTIVE,
        lockStatus: LockStatus.UNLOCKED
      };
      session = await this.sessionRepository.save(session);
    }

    // 检查用户是否已经在会话中
    const existingParticipant = session.participants.find(p => p.userId === userId);
    if (existingParticipant) {
      // 更新参与者状态
      existingParticipant.status = ParticipantStatus.ONLINE;
    } else {
      // 添加新参与者
      session.participants.push({
        userId: userId,
        joinedAt: new Date(),
        status: ParticipantStatus.ONLINE
      });
    }

    // 保存会话
    const updatedSession = await this.sessionRepository.save(session);

    // 通知其他参与者
    const otherParticipants = session.participants
      .filter(p => p.userId !== userId)
      .map(p => p.userId);

    if (otherParticipants.length > 0) {
      await this.webSocketService.sendToUsers(otherParticipants, {
        type: 'USER_JOINED_SESSION',
        payload: {
          documentId: documentId,
          userId: userId,
          joinedAt: new Date()
        }
      });
    }

    return updatedSession;
  }

  // 处理编辑操作
  async processOperation(documentId: string, userId: string, operation: Operation): Promise<void> {
    // 验证文档存在
    const document = await this.documentRepository.findById(documentId);
    if (!document) {
      throw new NotFoundException('Document not found');
    }

    // 验证用户是否有权限编辑文档
    if (!await this.hasDocumentEditPermission(document, userId)) {
      throw new ForbiddenException('You do not have permission to edit this document');
    }

    // 查找会话
    const session = await this.sessionRepository.findActiveByDocumentId(documentId);
    if (!session) {
      throw new NotFoundException('No active collaboration session found');
    }

    // 验证用户是否在会话中
    const isParticipant = session.participants.some(p => p.userId === userId);
    if (!isParticipant) {
      throw new ForbiddenException('You are not part of this collaboration session');
    }

    // 应用操作（实际应用根据具体的协作算法实现）
    // 这里只是简化的示例
    
    // 保存操作到会话
    if (!session.operations) {
      session.operations = [];
    }
    session.operations.push({
      id: uuidv4(),
      userId: userId,
      type: operation.type,
      position: operation.position,
      data: operation.data,
      timestamp: new Date(),
      version: document.version
    });

    await this.sessionRepository.save(session);

    // 广播操作给其他参与者
    const otherParticipants = session.participants
      .filter(p => p.userId !== userId)
      .map(p => p.userId);

    if (otherParticipants.length > 0) {
      await this.webSocketService.sendToUsers(otherParticipants, {
        type: 'OPERATION_RECEIVED',
        payload: {
          documentId: documentId,
          userId: userId,
          operation: operation
        }
      });
    }
  }

  // 创建团队
  async createTeam(createTeamDto: CreateTeamDto, userId: string): Promise<Team> {
    // 创建团队
    const team: Team = {
      id: uuidv4(),
      name: createTeamDto.name,
      description: createTeamDto.description,
      ownerId: userId,
      members: [{
        userId: userId,
        role: TeamRole.OWNER,
        joinedAt: new Date(),
        status: MemberStatus.ACTIVE
      }],
      createdAt: new Date(),
      updatedAt: new Date(),
      avatar: createTeamDto.avatar,
      settings: createTeamDto.settings || {
        joinApprovalRequired: true,
        publicVisibility: false
      },
      tags: createTeamDto.tags || []
    };

    // 保存团队
    const savedTeam = await this.teamRepository.save(team);

    return savedTeam;
  }

  // 管理团队成员
  async addTeamMember(teamId: string, addMemberDto: AddTeamMemberDto, userId: string): Promise<TeamMember> {
    // 验证团队存在
    const team = await this.teamRepository.findById(teamId);
    if (!team) {
      throw new NotFoundException('Team not found');
    }

    // 验证当前用户是否有权限添加成员
    const currentMember = team.members.find(m => m.userId === userId);
    if (!currentMember || 
        (currentMember.role !== TeamRole.OWNER && currentMember.role !== TeamRole.ADMIN)) {
      throw new ForbiddenException('You do not have permission to add members to this team');
    }

    // 验证用户是否存在
    const targetUser = await this.userService.getUserById(addMemberDto.userId);
    if (!targetUser) {
      throw new NotFoundException('Target user not found');
    }

    // 验证用户是否已经是团队成员
    const existingMember = team.members.find(m => m.userId === addMemberDto.userId);
    if (existingMember) {
      throw new BadRequestException('User is already a member of this team');
    }

    // 添加新成员
    const newMember: TeamMember = {
      userId: addMemberDto.userId,
      role: addMemberDto.role || TeamRole.MEMBER,
      joinedAt: new Date(),
      status: team.settings?.joinApprovalRequired ? MemberStatus.PENDING : MemberStatus.ACTIVE
    };

    team.members.push(newMember);
    team.updatedAt = new Date();

    // 保存团队
    await this.teamRepository.save(team);

    // 发送通知
    await this.notificationService.sendNotification(
      [addMemberDto.userId],
      'Team Invitation',
      `You have been invited to join team "${team.name}"`,
      {
        teamId: teamId,
        action: 'JOIN_TEAM'
      }
    );

    return newMember;
  }

  // 检查用户是否有文档访问权限
  private async hasDocumentAccessPermission(document: CollaborationDocument, userId: string): Promise<boolean> {
    // 文档所有者有访问权限
    if (document.ownerId === userId) {
      return true;
    }

    // 如果文档属于项目，检查用户是否为项目成员
    if (document.projectId) {
      // 假设存在一个projectService来检查项目成员关系
      // return await projectService.isProjectMember(document.projectId, userId);
    }

    // 检查文档共享权限（如果实现了更细粒度的权限控制）
    // ...

    return false;
  }

  // 检查用户是否有文档编辑权限
  private async hasDocumentEditPermission(document: CollaborationDocument, userId: string): Promise<boolean> {
    // 文档所有者有编辑权限
    if (document.ownerId === userId) {
      return true;
    }

    // 如果文档属于项目，检查用户是否有项目编辑权限
    if (document.projectId) {
      // 假设存在一个projectService来检查项目编辑权限
      // return await projectService.hasProjectPermission(document.projectId, userId, 'EDIT_DOCUMENT');
    }

    // 检查文档共享权限（如果实现了更细粒度的权限控制）
    // ...

    return false;
  }

  // 其他协作服务方法...
}
```

### 7.2 控制器实现

```typescript
// 协作控制器实现示例
class CollaborationController {
  private collaborationService: CollaborationService;
  
  constructor(collaborationService: CollaborationService) {
    this.collaborationService = collaborationService;
  }

  // 发送消息
  @Post('/messages')
  @AuthGuard()
  async sendMessage(@Body() sendMessageDto: SendMessageDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const message = await this.collaborationService.sendMessage(sendMessageDto, userId);
      res.status(HttpStatus.CREATED).json(message);
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

  // 获取消息列表
  @Get('/conversations/:conversationId/messages')
  @AuthGuard()
  async getMessages(@Param('conversationId') conversationId: string, @Query() query: MessageQueryDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      // 验证用户是否为会话参与者
      // 这里应该有一个验证逻辑，简化示例
      const messages = await this.collaborationService.getMessages(conversationId, query);
      res.status(HttpStatus.OK).json(messages);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 创建文档
  @Post('/documents')
  @AuthGuard()
  async createDocument(@Body() createDocumentDto: CreateDocumentDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const document = await this.collaborationService.createDocument(createDocumentDto, userId);
      res.status(HttpStatus.CREATED).json(document);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 更新文档
  @Put('/documents/:documentId')
  @AuthGuard()
  async updateDocument(@Param('documentId') documentId: string, @Body() updateDocumentDto: UpdateDocumentDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const document = await this.collaborationService.updateDocument(documentId, updateDocumentDto, userId);
      res.status(HttpStatus.OK).json(document);
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

  // 创建团队
  @Post('/teams')
  @AuthGuard()
  async createTeam(@Body() createTeamDto: CreateTeamDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const team = await this.collaborationService.createTeam(createTeamDto, userId);
      res.status(HttpStatus.CREATED).json(team);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 添加团队成员
  @Post('/teams/:teamId/members')
  @AuthGuard()
  async addTeamMember(@Param('teamId') teamId: string, @Body() addMemberDto: AddTeamMemberDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const member = await this.collaborationService.addTeamMember(teamId, addMemberDto, userId);
      res.status(HttpStatus.CREATED).json(member);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else if (error instanceof BadRequestException) {
        res.status(HttpStatus.BAD_REQUEST).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 其他控制器方法...
}
```

## 8. 安全考虑

### 8.1 身份认证与授权

- 严格的用户身份验证：所有协作操作必须经过身份验证
- 细粒度的权限控制：基于角色和资源所有权的权限模型
- 实时访问控制：在协作会话中动态验证用户权限
- API安全：所有API接口必须进行认证和授权检查
- 会话安全：实施安全的会话管理，防止会话劫持

### 8.2 数据保护

- 传输加密：所有数据传输必须使用HTTPS/WSS加密
- 存储加密：敏感的协作内容在存储时进行加密
- 数据隔离：不同用户和团队的协作数据相互隔离
- 审计日志：记录所有协作操作和访问日志，便于追踪
- 数据备份：定期备份协作数据，确保数据安全

### 8.3 防止滥用

- 速率限制：对API调用实施速率限制，防止滥用
- 内容审核：对用户生成的协作内容进行审核
- 敏感信息检测：检测和阻止敏感信息的共享
- 访问限制：对异常访问模式实施限制和告警
- 行为分析：分析用户行为模式，识别潜在的滥用行为

### 8.4 隐私保护

- 用户隐私：保护用户的个人信息和通信内容
- 隐私合规：遵循相关数据隐私法规（如GDPR、CCPA等）
- 匿名化处理：对需要分析但包含个人信息的数据进行匿名化处理
- 用户授权：在收集和使用用户数据前获得明确授权

## 9. 性能优化

### 9.1 实时通信优化

- WebSocket连接池：维护WebSocket连接池，提高连接复用率
- 消息队列：使用消息队列处理大量消息，提高系统吞吐量
- 消息批处理：对小型消息进行批处理，减少网络开销
- 消息压缩：压缩传输的消息数据，减少带宽占用
- 边缘节点：利用边缘节点减少延迟，提高消息传输速度

### 9.2 协作编辑性能

- 操作转换算法：优化操作转换算法，提高冲突解决效率
- 增量更新：只传输变更部分，减少数据传输量
- 本地缓存：在客户端维护文档的本地缓存，减少服务端请求
- 预加载策略：预加载可能需要的协作资源
- 延迟加载：对大型文档采用延迟加载策略

### 9.3 资源管理

- 会话管理：定期清理不活跃的协作会话，释放资源
- 连接管理：优化WebSocket连接的生命周期管理
- 负载均衡：实现WebSocket连接的负载均衡
- 资源限制：对单个会话的资源使用进行限制
- 自动扩展：根据负载情况自动扩展资源

### 9.4 并发处理

- 分布式锁：使用分布式锁协调多用户并发操作
- 异步处理：非关键操作使用异步处理，提高系统响应速度
- 并行处理：对独立的协作任务进行并行处理
- 资源隔离：不同协作任务的资源相互隔离，防止资源竞争

## 10. 总结

协作服务是OpenMind Lab系统的核心服务之一，负责提供实时交流、项目协作、文档协作和团队协作等功能。本文档详细描述了协作服务的设计方案，包括功能架构、数据模型、API接口、服务实现、安全考虑和性能优化等方面。通过合理的设计和实现，协作服务能够为科研人员提供高效、安全、可靠的协作平台，支持多人实时协作和知识共享。未来，协作服务可以根据业务需求进行扩展，如支持更多类型的协作文档、增强实时协作功能、集成更多第三方协作工具等，进一步提升用户体验和系统价值。
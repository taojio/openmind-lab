# OpenMind Lab 社区服务设计文档

## 1. 模块概述

社区服务（Community Service）是 OpenMind Lab 的重要组成部分，旨在建立和维护研究人员之间的互动社区，促进知识共享、协作交流和科研合作。本文档详细描述社区服务的设计方案、API 接口、数据模型和实现细节。

## 2. 功能架构

```
┌────────────────────────────────────────────────────────────────────┐
│                          社区服务架构                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  用户社区   │  │  论坛讨论   │  │  活动组织   │  │  社区管理   │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘ │
│         │                │                │                │        │
│  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐ │
│  │  用户档案   │  │  话题管理   │  │  通知系统   │  │  积分系统   │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

## 3. 核心功能模块

### 3.1 用户社区

**主要功能：**
- 用户档案管理：用户个人资料、研究方向、联系方式等信息管理
- 研究兴趣标签：用户可设置研究兴趣标签，便于发现志同道合的研究者
- 关注功能：用户可以关注其他用户，获取其最新动态
- 消息系统：用户间的私信和通知功能
- 研究成果展示：用户可以展示自己的研究成果
- 协作邀请：用户可以向其他用户发送协作邀请
- 社区活跃度统计：统计用户的社区活跃度

**用户档案信息：**
- 基本信息：姓名、头像、简介、所在地
- 研究信息：研究方向、所属机构、研究成果
- 教育背景：学历、毕业院校、专业
- 工作经历：工作单位、职位、职责
- 社交链接：学术社交平台、个人网站等链接
- 关注/粉丝：关注的用户和关注自己的用户
- 贡献统计：社区贡献统计

### 3.2 论坛讨论

**主要功能：**
- 话题创建：用户可以创建讨论话题
- 帖子发布：用户可以在话题下发布帖子
- 回复互动：用户可以回复帖子，形成讨论链
- 点赞/收藏：用户可以点赞和收藏感兴趣的帖子
- 标签分类：为话题和帖子添加标签，便于分类和检索
- 热门推荐：推荐热门话题和热门帖子
- 搜索功能：支持按关键词搜索话题和帖子
- 精华帖管理：标记高质量的精华帖

**论坛结构：**
- 话题（Topic）：讨论的主题，包含多个帖子
- 帖子（Post）：具体的讨论内容，属于某个话题
- 回复（Reply）：对帖子的回复
- 标签（Tag）：用于分类和检索
- 版块（Forum Section）：对话题进行分类的一级分类

### 3.3 活动组织

**主要功能：**
- 活动创建：创建线上/线下科研活动、研讨会、讲座等
- 活动管理：编辑、发布、取消活动
- 活动报名：用户可以报名参加活动
- 活动提醒：活动开始前发送提醒
- 活动反馈：收集用户对活动的反馈和评价
- 活动推荐：根据用户兴趣推荐相关活动
- 活动统计：统计活动参与人数、满意度等数据
- 活动成果分享：分享活动的演讲材料、视频等

**活动类型：**
- 研讨会：学术研讨会、专题讨论会
- 讲座：学术讲座、技术分享
- 培训：技能培训、工具使用培训
- 竞赛：科研竞赛、算法挑战
- 会议：学术会议、行业大会
- 工作坊：实践工作坊、实验教学
- 线上沙龙：线上交流活动

### 3.4 社区管理

**主要功能：**
- 管理员功能：社区管理员的管理界面和操作
- 用户管理：用户账号管理、权限控制、违规处理
- 内容审核：帖子、评论、活动等内容的审核
- 举报处理：处理用户举报的违规内容
- 公告管理：发布和管理社区公告
- 社区规则：制定和发布社区规则
- 数据分析：社区活跃度、用户行为等数据分析
- 反馈处理：收集和处理用户对社区的反馈

**管理角色：**
- 超级管理员：拥有最高权限，管理所有社区功能
- 社区管理员：负责特定社区版块的管理
- 内容审核员：负责内容的审核和管理
- 活动管理员：负责活动的组织和管理
- 版主：负责特定论坛版块的管理

### 3.5 用户档案

**主要功能：**
- 个人资料编辑：用户可以编辑和更新个人资料
- 隐私设置：用户可以设置个人资料的隐私级别
- 研究方向管理：添加和更新研究方向
- 教育经历管理：添加和更新教育经历
- 工作经历管理：添加和更新工作经历
- 社交链接管理：添加和更新社交平台链接
- 研究成果管理：添加和展示研究成果
- 个人主页定制：定制个人主页的展示内容和样式

**隐私设置：**
- 公开：所有人可见
- 仅关注者可见：仅关注自己的用户可见
- 仅自己可见：仅用户自己可见
- 自定义：自定义哪些用户或用户组可以查看

### 3.6 话题管理

**主要功能：**
- 话题创建：创建新的讨论话题
- 话题分类：为话题分配分类和标签
- 话题描述：设置话题的描述信息
- 话题权限：设置话题的访问和参与权限
- 话题状态：设置话题的状态（活跃、关闭、归档等）
- 话题统计：统计话题的参与人数、帖子数量等
- 话题推荐：推荐相关话题给用户
- 话题搜索：支持按关键词搜索话题

**话题类型：**
- 公开话题：所有人可以查看和参与
- 私有话题：仅特定用户或用户组可以查看和参与
- 项目话题：与特定项目相关的话题
- 活动话题：与特定活动相关的话题
- 问答话题：以问答形式组织的话题

### 3.7 通知系统

**主要功能：**
- 系统通知：系统级别的通知，如服务更新、维护通知等
- 活动通知：活动相关的通知，如活动开始提醒、报名成功等
- 互动通知：用户间互动的通知，如关注、点赞、评论等
- 消息通知：私信和群消息的通知
- 通知设置：用户可以自定义通知偏好
- 通知方式：支持站内信、邮件、推送等多种通知方式
- 通知历史：查看和管理历史通知
- 未读消息管理：管理未读消息状态

**通知类型：**
- 系统通知：来自系统的通知
- 活动通知：活动相关的通知
- 互动通知：用户互动产生的通知
- 消息通知：私信和群消息通知
- 提醒通知：各种提醒信息

### 3.8 积分系统

**主要功能：**
- 积分获取：用户通过参与社区活动获取积分
- 积分消费：用户可以使用积分兑换服务或权益
- 积分等级：根据积分数量设置用户等级
- 等级特权：不同等级的用户享有不同的特权
- 积分记录：记录用户的积分获取和消费历史
- 积分规则：设置积分的获取和消费规则
- 排行榜：展示用户的积分排名
- 徽章系统：根据用户贡献发放徽章

**积分来源：**
- 发布内容：发布帖子、回复、活动等
- 内容互动：点赞、收藏、评论等
- 社区贡献：回答问题、分享资源等
- 活动参与：参加社区活动
- 邀请用户：邀请新用户加入
- 举报违规：举报违规内容并被确认

## 4. 数据模型设计

### 4.1 社区实体（Community）

```typescript
interface Community {
  id: string;             // 社区唯一标识
  name: string;           // 社区名称
  description: string;    // 社区描述
  logoUrl?: string;       // 社区Logo URL
  coverUrl?: string;      // 社区封面图URL
  creatorId: string;      // 创建者ID
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  memberCount: number;    // 成员数量
  postCount: number;      // 帖子数量
  topicCount: number;     // 话题数量
  rules?: string;         // 社区规则
  status: CommunityStatus; // 社区状态
  visibility: Visibility; // 可见性
  settings?: CommunitySettings; // 社区设置
}

enum CommunityStatus {
  ACTIVE = 'ACTIVE',      // 活跃
  INACTIVE = 'INACTIVE',  // 不活跃
  PENDING = 'PENDING',    // 待审核
  BLOCKED = 'BLOCKED'     // 已封禁
}

enum Visibility {
  PUBLIC = 'PUBLIC',      // 公开
  PRIVATE = 'PRIVATE'     // 私有
}

interface CommunitySettings {
  enablePostApproval: boolean;       // 是否需要帖子审核
  enableCommentApproval: boolean;    // 是否需要评论审核
  allowAnonymousPost: boolean;       // 是否允许匿名发帖
  maxPostsPerDay?: number;           // 每日最大发帖数
  joinPolicy: JoinPolicy;            // 加入策略
}

enum JoinPolicy {
  OPEN = 'OPEN',          // 开放加入
  APPROVAL = 'APPROVAL',  // 需要审批
  INVITE = 'INVITE'       // 邀请制
}
```

### 4.2 社区成员实体（CommunityMember）

```typescript
interface CommunityMember {
  id: string;             // 成员关系唯一标识
  communityId: string;    // 社区ID
  userId: string;         // 用户ID
  role: CommunityRole;    // 角色
  joinDate: Date;         // 加入时间
  lastActiveAt: Date;     // 最后活跃时间
  status: MemberStatus;   // 成员状态
  settings?: MemberSettings; // 成员设置
}

enum CommunityRole {
  ADMIN = 'ADMIN',        // 管理员
  MODERATOR = 'MODERATOR', // 版主
  MEMBER = 'MEMBER',      // 普通成员
  VISITOR = 'VISITOR'     // 访客
}

enum MemberStatus {
  ACTIVE = 'ACTIVE',      // 活跃
  INACTIVE = 'INACTIVE',  // 不活跃
  PENDING = 'PENDING',    // 待审核
  BLOCKED = 'BLOCKED'     // 已封禁
}

interface MemberSettings {
  receiveNotifications: boolean;     // 是否接收通知
  emailDigest: DigestFrequency;      // 邮件摘要频率
}

enum DigestFrequency {
  DAILY = 'DAILY',        // 每日
  WEEKLY = 'WEEKLY',      // 每周
  MONTHLY = 'MONTHLY',    // 每月
  NEVER = 'NEVER'         // 从不
}
```

### 4.3 话题实体（Topic）

```typescript
interface Topic {
  id: string;             // 话题唯一标识
  title: string;          // 话题标题
  description?: string;   // 话题描述
  communityId: string;    // 所属社区ID
  creatorId: string;      // 创建者ID
  sectionId?: string;     // 所属版块ID
  tags: string[];         // 标签列表
  status: TopicStatus;    // 话题状态
  visibility: Visibility; // 可见性
  postCount: number;      // 帖子数量
  viewCount: number;      // 查看次数
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  lastPostAt: Date;       // 最后回复时间
  pinned: boolean;        // 是否置顶
  featured: boolean;      // 是否精选
}

enum TopicStatus {
  OPEN = 'OPEN',          // 开放
  CLOSED = 'CLOSED',      // 关闭
  ARCHIVED = 'ARCHIVED'   // 归档
}
```

### 4.4 帖子实体（Post）

```typescript
interface Post {
  id: string;             // 帖子唯一标识
  topicId: string;        // 所属话题ID
  authorId: string;       // 作者ID
  content: string;        // 帖子内容
  mediaUrls?: string[];   // 媒体文件URL列表
  status: PostStatus;     // 帖子状态
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  viewCount: number;      // 查看次数
  likeCount: number;      // 点赞数
  commentCount: number;   // 评论数
  anonymous: boolean;     // 是否匿名
  pinned: boolean;        // 是否置顶
  featured: boolean;      // 是否精选
  parentPostId?: string;  // 父帖子ID（用于回复）
}

enum PostStatus {
  PENDING = 'PENDING',    // 待审核
  APPROVED = 'APPROVED',  // 已批准
  REJECTED = 'REJECTED',  // 已拒绝
  DELETED = 'DELETED'     // 已删除
}
```

### 4.5 活动实体（Activity）

```typescript
interface Activity {
  id: string;             // 活动唯一标识
  title: string;          // 活动标题
  description: string;    // 活动描述
  type: ActivityType;     // 活动类型
  organizerId: string;    // 组织者ID
  communityId?: string;   // 所属社区ID
  location?: string;      // 活动地点
  onlineLink?: string;    // 线上活动链接
  startTime: Date;        // 开始时间
  endTime: Date;          // 结束时间
  registrationOpenTime: Date; // 报名开始时间
  registrationCloseTime: Date; // 报名截止时间
  maxParticipants?: number; // 最大参与人数
  participantCount: number; // 实际参与人数
  status: ActivityStatus; // 活动状态
  visibility: Visibility; // 可见性
  tags: string[];         // 标签列表
  coverImageUrl?: string; // 封面图片URL
  attachments?: Attachment[]; // 附件列表
  createdAt: Date;        // 创建时间
  updatedAt: Date;        // 更新时间
  requiresApproval: boolean; // 是否需要审核报名
}

enum ActivityType {
  WORKSHOP = 'WORKSHOP',  // 工作坊
  SEMINAR = 'SEMINAR',    // 研讨会
  LECTURE = 'LECTURE',    // 讲座
  CONFERENCE = 'CONFERENCE', // 会议
  TRAINING = 'TRAINING',  // 培训
  COMPETITION = 'COMPETITION', // 竞赛
  MEETUP = 'MEETUP'       // 聚会
}

enum ActivityStatus {
  DRAFT = 'DRAFT',        // 草稿
  PUBLISHED = 'PUBLISHED', // 已发布
  CANCELLED = 'CANCELLED', // 已取消
  COMPLETED = 'COMPLETED' // 已完成
}

interface Attachment {
  id: string;             // 附件唯一标识
  name: string;           // 附件名称
  type: string;           // 附件类型
  size: number;           // 附件大小（字节）
  url: string;            // 附件URL
}
```

### 4.6 活动参与者实体（ActivityParticipant）

```typescript
interface ActivityParticipant {
  id: string;             // 参与者关系唯一标识
  activityId: string;     // 活动ID
  userId: string;         // 用户ID
  registrationTime: Date; // 报名时间
  status: ParticipantStatus; // 参与者状态
  role: ParticipantRole;  // 参与者角色
  feedback?: string;      // 活动反馈
  rating?: number;        // 活动评分（1-5星）
}

enum ParticipantStatus {
  PENDING = 'PENDING',    // 待审核
  APPROVED = 'APPROVED',  // 已批准
  REJECTED = 'REJECTED',  // 已拒绝
  ATTENDED = 'ATTENDED',  // 已出席
  NO_SHOW = 'NO_SHOW',    // 未出席
  CANCELLED = 'CANCELLED' // 已取消
}

enum ParticipantRole {
  ORGANIZER = 'ORGANIZER', // 组织者
  SPEAKER = 'SPEAKER',    // 演讲者
  ATTENDEE = 'ATTENDEE',  // 参与者
  VOLUNTEER = 'VOLUNTEER' // 志愿者
}
```

### 4.7 通知实体（Notification）

```typescript
interface Notification {
  id: string;             // 通知唯一标识
  userId: string;         // 接收用户ID
  type: NotificationType; // 通知类型
  title: string;          // 通知标题
  content: string;        // 通知内容
  isRead: boolean;        // 是否已读
  sourceId?: string;      // 来源ID（如帖子ID、活动ID等）
  sourceType?: string;    // 来源类型
  actionUrl?: string;     // 操作URL
  createdAt: Date;        // 创建时间
  expiresAt?: Date;       // 过期时间
}

enum NotificationType {
  SYSTEM = 'SYSTEM',      // 系统通知
  ACTIVITY = 'ACTIVITY',  // 活动通知
  INTERACTION = 'INTERACTION', // 互动通知
  MESSAGE = 'MESSAGE',    // 消息通知
  REMINDER = 'REMINDER'   // 提醒通知
}
```

### 4.8 用户关注实体（UserFollow）

```typescript
interface UserFollow {
  id: string;             // 关注关系唯一标识
  followerId: string;     // 关注者ID
  followingId: string;    // 被关注者ID
  followDate: Date;       // 关注时间
  status: FollowStatus;   // 关注状态
}

enum FollowStatus {
  ACTIVE = 'ACTIVE',      // 活跃
  PENDING = 'PENDING',    // 待审核
  BLOCKED = 'BLOCKED'     // 已阻止
}
```

### 4.9 用户积分实体（UserPoints）

```typescript
interface UserPoints {
  id: string;             // 积分记录唯一标识
  userId: string;         // 用户ID
  points: number;         // 积分数量（正数表示增加，负数表示减少）
  type: PointsType;       // 积分类型
  sourceId?: string;      // 积分来源ID
  sourceType?: string;    // 积分来源类型
  description: string;    // 积分变动描述
  createdAt: Date;        // 变动时间
}

enum PointsType {
  POST_CREATED = 'POST_CREATED', // 发布帖子
  COMMENT_CREATED = 'COMMENT_CREATED', // 发布评论
  POST_LIKED = 'POST_LIKED',     // 帖子被点赞
  JOIN_ACTIVITY = 'JOIN_ACTIVITY', // 参加活动
  INVITE_USER = 'INVITE_USER',   // 邀请用户
  REPORT_APPROVED = 'REPORT_APPROVED', // 举报被批准
  POINTS_EXPENSE = 'POINTS_EXPENSE' // 积分消费
}
```

### 4.10 用户等级实体（UserLevel）

```typescript
interface UserLevel {
  id: string;             // 等级唯一标识
  name: string;           // 等级名称
  minPoints: number;      // 最低积分
  maxPoints: number;      // 最高积分
  privileges: string[];   // 等级特权描述
  iconUrl?: string;       // 等级图标URL
  color?: string;         // 等级颜色
}
```

## 5. API 接口设计

### 5.1 RESTful API

#### 社区管理

- **创建社区**
  - 路径：`POST /api/v1/communities`
  - 请求体：`{ name, description, logoUrl?, coverUrl?, rules?, settings? }`
  - 响应：`201 Created` with community info
  - 权限：已登录用户

- **获取社区列表**
  - 路径：`GET /api/v1/communities`
  - 查询参数：`page`, `pageSize`, `search?`, `sort?`, `visibility?`, `status?`
  - 响应：`200 OK` with communities list
  - 权限：已登录用户

- **获取社区详情**
  - 路径：`GET /api/v1/communities/{communityId}`
  - 响应：`200 OK` with community details
  - 权限：已登录用户

- **更新社区**
  - 路径：`PUT /api/v1/communities/{communityId}`
  - 请求体：`{ name?, description?, logoUrl?, coverUrl?, rules?, settings? }`
  - 响应：`200 OK` with updated community
  - 权限：社区管理员或超级管理员

- **删除社区**
  - 路径：`DELETE /api/v1/communities/{communityId}`
  - 响应：`204 No Content`
  - 权限：社区管理员或超级管理员

- **加入社区**
  - 路径：`POST /api/v1/communities/{communityId}/join`
  - 请求体：`{ message? }` // 加入申请消息（如果需要审批）
  - 响应：`200 OK` with member info
  - 权限：已登录用户

- **退出社区**
  - 路径：`POST /api/v1/communities/{communityId}/leave`
  - 响应：`204 No Content`
  - 权限：已登录用户（社区成员）

#### 话题与帖子

- **创建话题**
  - 路径：`POST /api/v1/communities/{communityId}/topics`
  - 请求体：`{ title, description?, sectionId?, tags? }`
  - 响应：`201 Created` with topic info
  - 权限：社区成员

- **获取话题列表**
  - 路径：`GET /api/v1/communities/{communityId}/topics`
  - 查询参数：`page`, `pageSize`, `search?`, `sort?`, `status?`, `sectionId?`, `tags?`
  - 响应：`200 OK` with topics list
  - 权限：已登录用户

- **获取话题详情**
  - 路径：`GET /api/v1/topics/{topicId}`
  - 响应：`200 OK` with topic details
  - 权限：已登录用户

- **创建帖子**
  - 路径：`POST /api/v1/topics/{topicId}/posts`
  - 请求体：`{ content, mediaUrls?, anonymous? }`
  - 响应：`201 Created` with post info
  - 权限：社区成员

- **获取帖子列表**
  - 路径：`GET /api/v1/topics/{topicId}/posts`
  - 查询参数：`page`, `pageSize`, `sort?`, `status?`
  - 响应：`200 OK` with posts list
  - 权限：已登录用户

- **获取帖子详情**
  - 路径：`GET /api/v1/posts/{postId}`
  - 响应：`200 OK` with post details
  - 权限：已登录用户

- **更新帖子**
  - 路径：`PUT /api/v1/posts/{postId}`
  - 请求体：`{ content?, mediaUrls?, status? }`
  - 响应：`200 OK` with updated post
  - 权限：帖子作者或社区管理员

- **删除帖子**
  - 路径：`DELETE /api/v1/posts/{postId}`
  - 响应：`204 No Content`
  - 权限：帖子作者或社区管理员

- **评论帖子**
  - 路径：`POST /api/v1/posts/{postId}/comments`
  - 请求体：`{ content, parentCommentId? }`
  - 响应：`201 Created` with comment info
  - 权限：社区成员

#### 活动管理

- **创建活动**
  - 路径：`POST /api/v1/activities`
  - 请求体：`{ title, description, type, organizerId, communityId?, location?, onlineLink?, startTime, endTime, registrationOpenTime, registrationCloseTime, maxParticipants?, tags?, coverImageUrl?, attachments?, requiresApproval? }`
  - 响应：`201 Created` with activity info
  - 权限：已登录用户

- **获取活动列表**
  - 路径：`GET /api/v1/activities`
  - 查询参数：`page`, `pageSize`, `search?`, `sort?`, `type?`, `status?`, `communityId?`, `startDate?`, `endDate?`
  - 响应：`200 OK` with activities list
  - 权限：已登录用户

- **获取活动详情**
  - 路径：`GET /api/v1/activities/{activityId}`
  - 响应：`200 OK` with activity details
  - 权限：已登录用户

- **更新活动**
  - 路径：`PUT /api/v1/activities/{activityId}`
  - 请求体：`{ title?, description?, type?, location?, onlineLink?, startTime?, endTime?, registrationOpenTime?, registrationCloseTime?, maxParticipants?, tags?, coverImageUrl?, attachments?, status?, requiresApproval? }`
  - 响应：`200 OK` with updated activity
  - 权限：活动组织者或社区管理员

- **删除活动**
  - 路径：`DELETE /api/v1/activities/{activityId}`
  - 响应：`204 No Content`
  - 权限：活动组织者或社区管理员

- **报名活动**
  - 路径：`POST /api/v1/activities/{activityId}/register`
  - 请求体：`{ message? }` // 报名消息（如果需要审批）
  - 响应：`200 OK` with participant info
  - 权限：已登录用户

- **取消报名**
  - 路径：`POST /api/v1/activities/{activityId}/unregister`
  - 响应：`204 No Content`
  - 权限：已登录用户（活动参与者）

#### 用户互动

- **关注用户**
  - 路径：`POST /api/v1/users/{userId}/follow`
  - 响应：`200 OK` with follow info
  - 权限：已登录用户

- **取消关注**
  - 路径：`DELETE /api/v1/users/{userId}/follow`
  - 响应：`204 No Content`
  - 权限：已登录用户

- **获取关注列表**
  - 路径：`GET /api/v1/users/{userId}/following`
  - 查询参数：`page`, `pageSize`
  - 响应：`200 OK` with following list
  - 权限：已登录用户

- **获取粉丝列表**
  - 路径：`GET /api/v1/users/{userId}/followers`
  - 查询参数：`page`, `pageSize`
  - 响应：`200 OK` with followers list
  - 权限：已登录用户

- **点赞帖子**
  - 路径：`POST /api/v1/posts/{postId}/like`
  - 响应：`200 OK` with like status
  - 权限：已登录用户

- **取消点赞**
  - 路径：`DELETE /api/v1/posts/{postId}/like`
  - 响应：`204 No Content`
  - 权限：已登录用户

#### 通知管理

- **获取通知列表**
  - 路径：`GET /api/v1/notifications`
  - 查询参数：`page`, `pageSize`, `type?`, `isRead?`
  - 响应：`200 OK` with notifications list
  - 权限：已登录用户

- **标记通知为已读**
  - 路径：`PUT /api/v1/notifications/{notificationId}/read`
  - 响应：`204 No Content`
  - 权限：已登录用户（通知接收者）

- **标记所有通知为已读**
  - 路径：`PUT /api/v1/notifications/read-all`
  - 响应：`204 No Content`
  - 权限：已登录用户

- **删除通知**
  - 路径：`DELETE /api/v1/notifications/{notificationId}`
  - 响应：`204 No Content`
  - 权限：已登录用户（通知接收者）

- **设置通知偏好**
  - 路径：`PUT /api/v1/users/me/notification-settings`
  - 请求体：通知偏好设置
  - 响应：`200 OK` with settings
  - 权限：已登录用户

## 6. 服务实现

### 6.1 服务层实现

```typescript
// 社区服务实现示例
class CommunityService {
  private communityRepository: CommunityRepository;
  private communityMemberRepository: CommunityMemberRepository;
  private topicRepository: TopicRepository;
  private postRepository: PostRepository;
  private activityRepository: ActivityRepository;
  private activityParticipantRepository: ActivityParticipantRepository;
  private notificationRepository: NotificationRepository;
  private userFollowRepository: UserFollowRepository;
  private userPointsRepository: UserPointsRepository;
  private userLevelRepository: UserLevelRepository;
  private userService: UserService;
  private permissionService: PermissionService;
  private fileService: FileService;

  constructor(
    communityRepository: CommunityRepository,
    communityMemberRepository: CommunityMemberRepository,
    topicRepository: TopicRepository,
    postRepository: PostRepository,
    activityRepository: ActivityRepository,
    activityParticipantRepository: ActivityParticipantRepository,
    notificationRepository: NotificationRepository,
    userFollowRepository: UserFollowRepository,
    userPointsRepository: UserPointsRepository,
    userLevelRepository: UserLevelRepository,
    userService: UserService,
    permissionService: PermissionService,
    fileService: FileService
  ) {
    this.communityRepository = communityRepository;
    this.communityMemberRepository = communityMemberRepository;
    this.topicRepository = topicRepository;
    this.postRepository = postRepository;
    this.activityRepository = activityRepository;
    this.activityParticipantRepository = activityParticipantRepository;
    this.notificationRepository = notificationRepository;
    this.userFollowRepository = userFollowRepository;
    this.userPointsRepository = userPointsRepository;
    this.userLevelRepository = userLevelRepository;
    this.userService = userService;
    this.permissionService = permissionService;
    this.fileService = fileService;
  }

  // 创建社区
  async createCommunity(createCommunityDto: CreateCommunityDto, userId: string): Promise<Community> {
    // 验证用户是否可以创建社区
    // await this.permissionService.checkPermission(userId, 'CREATE_COMMUNITY');

    // 创建社区
    const community: Community = {
      id: uuidv4(),
      name: createCommunityDto.name,
      description: createCommunityDto.description,
      logoUrl: createCommunityDto.logoUrl,
      coverUrl: createCommunityDto.coverUrl,
      creatorId: userId,
      createdAt: new Date(),
      updatedAt: new Date(),
      memberCount: 1, // 创建者自动加入
      postCount: 0,
      topicCount: 0,
      rules: createCommunityDto.rules,
      status: CommunityStatus.ACTIVE,
      visibility: createCommunityDto.visibility || Visibility.PUBLIC,
      settings: createCommunityDto.settings || {
        enablePostApproval: false,
        enableCommentApproval: false,
        allowAnonymousPost: true,
        joinPolicy: JoinPolicy.OPEN
      }
    };

    // 保存社区
    const savedCommunity = await this.communityRepository.save(community);

    // 添加创建者为管理员
    await this.communityMemberRepository.save({
      id: uuidv4(),
      communityId: savedCommunity.id,
      userId: userId,
      role: CommunityRole.ADMIN,
      joinDate: new Date(),
      lastActiveAt: new Date(),
      status: MemberStatus.ACTIVE,
      settings: {
        receiveNotifications: true,
        emailDigest: DigestFrequency.WEEKLY
      }
    });

    // 发送通知
    // await this.notificationService.sendNotification(userId, 'CommunityCreated', 'Your community has been created successfully');

    return savedCommunity;
  }

  // 加入社区
  async joinCommunity(communityId: string, userId: string, joinMessage?: string): Promise<CommunityMember> {
    // 验证社区存在
    const community = await this.communityRepository.findById(communityId);
    if (!community) {
      throw new NotFoundException('Community not found');
    }

    // 检查用户是否已经是社区成员
    const existingMember = await this.communityMemberRepository.findByCommunityIdAndUserId(communityId, userId);
    if (existingMember) {
      throw new ConflictException('User is already a member of this community');
    }

    // 检查加入策略
    const status = community.settings?.joinPolicy === JoinPolicy.APPROVAL ? 
      MemberStatus.PENDING : MemberStatus.ACTIVE;

    // 创建社区成员关系
    const member: CommunityMember = {
      id: uuidv4(),
      communityId: communityId,
      userId: userId,
      role: CommunityRole.MEMBER,
      joinDate: new Date(),
      lastActiveAt: new Date(),
      status: status,
      settings: {
        receiveNotifications: true,
        emailDigest: DigestFrequency.WEEKLY
      }
    };

    // 保存成员关系
    const savedMember = await this.communityMemberRepository.save(member);

    // 更新社区成员数量
    community.memberCount += 1;
    await this.communityRepository.save(community);

    // 如果需要审批，通知管理员
    if (status === MemberStatus.PENDING) {
      // 查找社区管理员
      const admins = await this.communityMemberRepository.findByCommunityIdAndRole(communityId, CommunityRole.ADMIN);
      
      // 发送审批通知
      for (const admin of admins) {
        await this.notificationRepository.save({
          id: uuidv4(),
          userId: admin.userId,
          type: NotificationType.SYSTEM,
          title: 'New Membership Request',
          content: `User ${userId} wants to join your community`,
          isRead: false,
          sourceId: communityId,
          sourceType: 'COMMUNITY',
          createdAt: new Date()
        });
      }
    }

    return savedMember;
  }

  // 创建话题
  async createTopic(createTopicDto: CreateTopicDto, userId: string): Promise<Topic> {
    // 验证社区存在
    const community = await this.communityRepository.findById(createTopicDto.communityId);
    if (!community) {
      throw new NotFoundException('Community not found');
    }

    // 验证用户是否是社区成员
    const member = await this.communityMemberRepository.findByCommunityIdAndUserId(
      createTopicDto.communityId, 
      userId
    );
    if (!member || member.status !== MemberStatus.ACTIVE) {
      throw new ForbiddenException('You must be an active member of the community to create a topic');
    }

    // 创建话题
    const topic: Topic = {
      id: uuidv4(),
      title: createTopicDto.title,
      description: createTopicDto.description,
      communityId: createTopicDto.communityId,
      creatorId: userId,
      sectionId: createTopicDto.sectionId,
      tags: createTopicDto.tags || [],
      status: TopicStatus.OPEN,
      visibility: Visibility.PUBLIC, // 简化示例，实际可能需要设置
      postCount: 0,
      viewCount: 0,
      createdAt: new Date(),
      updatedAt: new Date(),
      lastPostAt: new Date(),
      pinned: false,
      featured: false
    };

    // 保存话题
    const savedTopic = await this.topicRepository.save(topic);

    // 更新社区话题数量
    community.topicCount += 1;
    await this.communityRepository.save(community);

    // 增加用户积分
    await this.addUserPoints(userId, PointsType.TOPIC_CREATED, 10, 'Created a new topic');

    return savedTopic;
  }

  // 创建帖子
  async createPost(createPostDto: CreatePostDto, userId: string): Promise<Post> {
    // 验证话题存在
    const topic = await this.topicRepository.findById(createPostDto.topicId);
    if (!topic) {
      throw new NotFoundException('Topic not found');
    }

    // 验证社区成员身份
    const member = await this.communityMemberRepository.findByCommunityIdAndUserId(
      topic.communityId, 
      userId
    );
    if (!member || member.status !== MemberStatus.ACTIVE) {
      throw new ForbiddenException('You must be an active member of the community to create a post');
    }

    // 验证话题状态
    if (topic.status !== TopicStatus.OPEN) {
      throw new ForbiddenException('Cannot create posts in closed or archived topics');
    }

    // 处理媒体文件（简化示例）
    let mediaUrls: string[] = [];
    if (createPostDto.mediaFiles && createPostDto.mediaFiles.length > 0) {
      // 实际应用中，这里应该处理文件上传
      // 简化示例
      mediaUrls = createPostDto.mediaFiles.map(file => `https://files.openmindlab.com/${uuidv4()}/${file.name}`);
    }

    // 检查社区设置是否需要审核
    const community = await this.communityRepository.findById(topic.communityId);
    const status = community?.settings?.enablePostApproval ? 
      PostStatus.PENDING : PostStatus.APPROVED;

    // 创建帖子
    const post: Post = {
      id: uuidv4(),
      topicId: createPostDto.topicId,
      authorId: userId,
      content: createPostDto.content,
      mediaUrls: mediaUrls,
      status: status,
      createdAt: new Date(),
      updatedAt: new Date(),
      viewCount: 0,
      likeCount: 0,
      commentCount: 0,
      anonymous: createPostDto.anonymous || false,
      pinned: false,
      featured: false,
      parentPostId: createPostDto.parentPostId
    };

    // 保存帖子
    const savedPost = await this.postRepository.save(post);

    // 更新话题统计信息
    topic.postCount += 1;
    topic.lastPostAt = new Date();
    await this.topicRepository.save(topic);

    // 更新社区帖子数量
    if (community) {
      community.postCount += 1;
      await this.communityRepository.save(community);
    }

    // 如果是回复帖子，更新原帖子评论数
    if (createPostDto.parentPostId) {
      const parentPost = await this.postRepository.findById(createPostDto.parentPostId);
      if (parentPost) {
        parentPost.commentCount += 1;
        await this.postRepository.save(parentPost);

        // 发送通知给原帖子作者
        await this.notificationRepository.save({
          id: uuidv4(),
          userId: parentPost.authorId,
          type: NotificationType.INTERACTION,
          title: 'New Reply',
          content: `Your post in topic "${topic.title}" has a new reply`,
          isRead: false,
          sourceId: savedPost.id,
          sourceType: 'POST',
          createdAt: new Date()
        });
      }
    }

    // 如果需要审批，通知管理员
    if (status === PostStatus.PENDING) {
      // 查找社区管理员和版主
      const moderators = await this.communityMemberRepository.findByCommunityIdAndRoles(
        topic.communityId, 
        [CommunityRole.ADMIN, CommunityRole.MODERATOR]
      );
      
      // 发送审批通知
      for (const moderator of moderators) {
        await this.notificationRepository.save({
          id: uuidv4(),
          userId: moderator.userId,
          type: NotificationType.SYSTEM,
          title: 'New Post Pending Approval',
          content: `A new post in topic "${topic.title}" requires your approval`,
          isRead: false,
          sourceId: savedPost.id,
          sourceType: 'POST',
          createdAt: new Date()
        });
      }
    }

    // 增加用户积分
    await this.addUserPoints(userId, PointsType.POST_CREATED, 5, 'Created a new post');

    return savedPost;
  }

  // 创建活动
  async createActivity(createActivityDto: CreateActivityDto, userId: string): Promise<Activity> {
    // 验证用户是否可以创建活动
    // await this.permissionService.checkPermission(userId, 'CREATE_ACTIVITY');

    // 验证社区存在（如果指定了社区）
    if (createActivityDto.communityId) {
      const community = await this.communityRepository.findById(createActivityDto.communityId);
      if (!community) {
        throw new NotFoundException('Community not found');
      }

      // 验证用户是否是社区成员
      const member = await this.communityMemberRepository.findByCommunityIdAndUserId(
        createActivityDto.communityId, 
        userId
      );
      if (!member || member.status !== MemberStatus.ACTIVE) {
        throw new ForbiddenException('You must be an active member of the community to create an activity');
      }
    }

    // 处理附件（简化示例）
    let attachments: Attachment[] = [];
    if (createActivityDto.attachments && createActivityDto.attachments.length > 0) {
      // 实际应用中，这里应该处理文件上传
      // 简化示例
      attachments = createActivityDto.attachments.map((att, index) => ({
        id: uuidv4(),
        name: att.name,
        type: att.type,
        size: att.size,
        url: `https://files.openmindlab.com/${uuidv4()}/${att.name}`
      }));
    }

    // 创建活动
    const activity: Activity = {
      id: uuidv4(),
      title: createActivityDto.title,
      description: createActivityDto.description,
      type: createActivityDto.type,
      organizerId: userId,
      communityId: createActivityDto.communityId,
      location: createActivityDto.location,
      onlineLink: createActivityDto.onlineLink,
      startTime: new Date(createActivityDto.startTime),
      endTime: new Date(createActivityDto.endTime),
      registrationOpenTime: new Date(createActivityDto.registrationOpenTime),
      registrationCloseTime: new Date(createActivityDto.registrationCloseTime),
      maxParticipants: createActivityDto.maxParticipants,
      participantCount: 0,
      status: createActivityDto.status || ActivityStatus.PUBLISHED,
      visibility: createActivityDto.visibility || Visibility.PUBLIC,
      tags: createActivityDto.tags || [],
      coverImageUrl: createActivityDto.coverImageUrl,
      attachments: attachments,
      createdAt: new Date(),
      updatedAt: new Date(),
      requiresApproval: createActivityDto.requiresApproval || false
    };

    // 保存活动
    const savedActivity = await this.activityRepository.save(activity);

    // 如果活动属于社区，通知社区成员
    if (savedActivity.communityId) {
      const members = await this.communityMemberRepository.findByCommunityId(savedActivity.communityId);
      
      // 发送活动通知
      for (const member of members) {
        if (member.userId !== userId && member.settings?.receiveNotifications) {
          await this.notificationRepository.save({
            id: uuidv4(),
            userId: member.userId,
            type: NotificationType.ACTIVITY,
            title: 'New Activity',
            content: `A new activity "${savedActivity.title}" has been created in your community`,
            isRead: false,
            sourceId: savedActivity.id,
            sourceType: 'ACTIVITY',
            createdAt: new Date()
          });
        }
      }
    }

    // 增加用户积分
    await this.addUserPoints(userId, PointsType.ACTIVITY_CREATED, 20, 'Created a new activity');

    return savedActivity;
  }

  // 报名活动
  async registerForActivity(activityId: string, userId: string, message?: string): Promise<ActivityParticipant> {
    // 验证活动存在
    const activity = await this.activityRepository.findById(activityId);
    if (!activity) {
      throw new NotFoundException('Activity not found');
    }

    // 验证活动状态
    if (activity.status !== ActivityStatus.PUBLISHED) {
      throw new ForbiddenException('Cannot register for this activity');
    }

    // 验证报名时间
    const now = new Date();
    if (now < activity.registrationOpenTime || now > activity.registrationCloseTime) {
      throw new ForbiddenException('Registration is not open for this activity');
    }

    // 检查用户是否已经报名
    const existingParticipant = await this.activityParticipantRepository.findByActivityIdAndUserId(activityId, userId);
    if (existingParticipant) {
      throw new ConflictException('You have already registered for this activity');
    }

    // 检查是否达到最大参与人数
    if (activity.maxParticipants && activity.participantCount >= activity.maxParticipants) {
      throw new ForbiddenException('This activity has reached maximum capacity');
    }

    // 检查是否需要审核
    const status = activity.requiresApproval ? ParticipantStatus.PENDING : ParticipantStatus.APPROVED;

    // 创建活动参与者
    const participant: ActivityParticipant = {
      id: uuidv4(),
      activityId: activityId,
      userId: userId,
      registrationTime: new Date(),
      status: status,
      role: ParticipantRole.ATTENDEE
    };

    // 保存参与者
    const savedParticipant = await this.activityParticipantRepository.save(participant);

    // 更新活动参与人数
    activity.participantCount += 1;
    await this.activityRepository.save(activity);

    // 如果需要审批，通知组织者
    if (status === ParticipantStatus.PENDING) {
      await this.notificationRepository.save({
        id: uuidv4(),
        userId: activity.organizerId,
        type: NotificationType.ACTIVITY,
        title: 'New Registration',
        content: `User ${userId} wants to register for your activity "${activity.title}"`,
        isRead: false,
        sourceId: activityId,
        sourceType: 'ACTIVITY',
        createdAt: new Date()
      });
    } else {
      // 发送报名成功通知
      await this.notificationRepository.save({
        id: uuidv4(),
        userId: userId,
        type: NotificationType.ACTIVITY,
        title: 'Registration Successful',
        content: `You have successfully registered for activity "${activity.title}"`,
        isRead: false,
        sourceId: activityId,
        sourceType: 'ACTIVITY',
        createdAt: new Date()
      });
    }

    // 增加用户积分
    await this.addUserPoints(userId, PointsType.JOIN_ACTIVITY, 10, 'Joined an activity');

    return savedParticipant;
  }

  // 关注用户
  async followUser(targetUserId: string, followerUserId: string): Promise<UserFollow> {
    // 验证目标用户存在
    const targetUser = await this.userService.findById(targetUserId);
    if (!targetUser) {
      throw new NotFoundException('Target user not found');
    }

    // 检查不能关注自己
    if (targetUserId === followerUserId) {
      throw new BadRequestException('You cannot follow yourself');
    }

    // 检查是否已经关注
    const existingFollow = await this.userFollowRepository.findByFollowerIdAndFollowingId(followerUserId, targetUserId);
    if (existingFollow) {
      throw new ConflictException('You are already following this user');
    }

    // 创建关注关系
    const follow: UserFollow = {
      id: uuidv4(),
      followerId: followerUserId,
      followingId: targetUserId,
      followDate: new Date(),
      status: FollowStatus.ACTIVE
    };

    // 保存关注关系
    const savedFollow = await this.userFollowRepository.save(follow);

    // 发送通知给被关注用户
    await this.notificationRepository.save({
      id: uuidv4(),
      userId: targetUserId,
      type: NotificationType.INTERACTION,
      title: 'New Follower',
      content: `User ${followerUserId} is now following you`,
      isRead: false,
      sourceId: followerUserId,
      sourceType: 'USER',
      createdAt: new Date()
    });

    return savedFollow;
  }

  // 添加用户积分
  private async addUserPoints(userId: string, type: PointsType, points: number, description: string): Promise<void> {
    // 创建积分记录
    const pointsRecord: UserPoints = {
      id: uuidv4(),
      userId: userId,
      points: points,
      type: type,
      description: description,
      createdAt: new Date()
    };

    await this.userPointsRepository.save(pointsRecord);

    // 更新用户等级
    await this.updateUserLevel(userId);
  }

  // 更新用户等级
  private async updateUserLevel(userId: string): Promise<void> {
    // 获取用户总积分
    const pointsRecords = await this.userPointsRepository.findByUserId(userId);
    const totalPoints = pointsRecords.reduce((sum, record) => sum + record.points, 0);

    // 查找用户当前等级
    const currentLevel = await this.userLevelRepository.findByUserId(userId);

    // 查找符合当前积分的等级
    const levels = await this.userLevelRepository.findAll();
    const newLevel = levels.find(level => 
      totalPoints >= level.minPoints && totalPoints < level.maxPoints
    );

    // 如果等级发生变化，更新用户等级
    if (newLevel && (!currentLevel || currentLevel.id !== newLevel.id)) {
      // 更新用户等级（实际应用中应该有用户等级关联表）
      // await this.userLevelRepository.updateUserLevel(userId, newLevel.id);

      // 发送等级提升通知
      await this.notificationRepository.save({
        id: uuidv4(),
        userId: userId,
        type: NotificationType.SYSTEM,
        title: 'Level Up!',
        content: `Congratulations! You have reached level "${newLevel.name}"`,
        isRead: false,
        createdAt: new Date()
      });
    }
  }

  // 其他社区服务方法...
}
```

### 6.2 控制器实现

```typescript
// 社区控制器实现示例
class CommunityController {
  private communityService: CommunityService;
  
  constructor(communityService: CommunityService) {
    this.communityService = communityService;
  }

  // 创建社区
  @Post('/communities')
  @AuthGuard()
  async createCommunity(@Body() createCommunityDto: CreateCommunityDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const community = await this.communityService.createCommunity(createCommunityDto, userId);
      res.status(HttpStatus.CREATED).json(community);
    } catch (error) {
      if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 获取社区列表
  @Get('/communities')
  @AuthGuard()
  async getCommunityList(@Query() query: CommunityListQuery, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const pagination = {
        page: query.page || 1,
        pageSize: query.pageSize || 10
      };
      
      const filter = {
        search: query.search,
        sort: query.sort,
        visibility: query.visibility,
        status: query.status
      };
      
      const communityList = await this.communityService.getCommunityList(filter, pagination);
      res.status(HttpStatus.OK).json(communityList);
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
    }
  }

  // 获取社区详情
  @Get('/communities/:communityId')
  @AuthGuard()
  async getCommunityById(@Param('communityId') communityId: string, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const community = await this.communityService.getCommunityById(communityId, userId);
      res.status(HttpStatus.OK).json(community);
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

  // 加入社区
  @Post('/communities/:communityId/join')
  @AuthGuard()
  async joinCommunity(@Param('communityId') communityId: string, @Body() joinDto: JoinCommunityDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const member = await this.communityService.joinCommunity(communityId, userId, joinDto.message);
      res.status(HttpStatus.OK).json(member);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 创建话题
  @Post('/communities/:communityId/topics')
  @AuthGuard()
  async createTopic(@Param('communityId') communityId: string, @Body() createTopicDto: CreateTopicDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      // 设置communityId
      createTopicDto.communityId = communityId;
      const topic = await this.communityService.createTopic(createTopicDto, userId);
      res.status(HttpStatus.CREATED).json(topic);
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

  // 创建帖子
  @Post('/topics/:topicId/posts')
  @AuthGuard()
  async createPost(@Param('topicId') topicId: string, @Body() createPostDto: CreatePostDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      // 设置topicId
      createPostDto.topicId = topicId;
      const post = await this.communityService.createPost(createPostDto, userId);
      res.status(HttpStatus.CREATED).json(post);
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

  // 创建活动
  @Post('/activities')
  @AuthGuard()
  async createActivity(@Body() createActivityDto: CreateActivityDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const activity = await this.communityService.createActivity(createActivityDto, userId);
      res.status(HttpStatus.CREATED).json(activity);
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

  // 报名活动
  @Post('/activities/:activityId/register')
  @AuthGuard()
  async registerForActivity(@Param('activityId') activityId: string, @Body() registerDto: RegisterActivityDto, @Request() req, @Response() res) {
    try {
      const userId = req.user.id;
      const participant = await this.communityService.registerForActivity(activityId, userId, registerDto.message);
      res.status(HttpStatus.OK).json(participant);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
      } else if (error instanceof ForbiddenException) {
        res.status(HttpStatus.FORBIDDEN).json({ message: error.message });
      } else {
        res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Internal server error' });
      }
    }
  }

  // 关注用户
  @Post('/users/:userId/follow')
  @AuthGuard()
  async followUser(@Param('userId') userId: string, @Request() req, @Response() res) {
    try {
      const followerUserId = req.user.id;
      const follow = await this.communityService.followUser(userId, followerUserId);
      res.status(HttpStatus.OK).json(follow);
    } catch (error) {
      if (error instanceof NotFoundException) {
        res.status(HttpStatus.NOT_FOUND).json({ message: error.message });
      } else if (error instanceof ConflictException) {
        res.status(HttpStatus.CONFLICT).json({ message: error.message });
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

## 7. 安全考虑

### 7.1 身份认证与授权

- 所有社区操作都需要用户登录和适当的权限
- 基于角色的访问控制（RBAC），不同角色有不同的权限
- 细粒度的权限检查，确保用户只能访问和操作其有权限的资源
- API接口需要进行身份验证和授权检查
- 敏感操作需要二次确认或额外的安全验证

### 7.2 内容安全

- 所有用户生成的内容（帖子、评论、活动描述等）需要进行内容审核
- 实现敏感词过滤机制，过滤不当内容
- 提供举报功能，允许用户举报不当内容
- 对被举报内容进行快速处理和响应
- 对发布不当内容的用户进行警告和处罚

### 7.3 隐私保护

- 用户可以控制其个人信息的可见性
- 敏感信息（如联系方式）需要进行保护，不轻易公开
- 遵循相关数据隐私法规（如GDPR、CCPA等）
- 对用户数据进行匿名化处理，保护用户隐私
- 用户有权访问、修改和删除自己的数据

### 7.4 反垃圾机制

- 实现反垃圾机器人机制，防止垃圾内容发布
- 对频繁发布相似内容的用户进行限制
- 对短时间内大量注册的账号进行审核
- 使用验证码等技术防止自动化攻击
- 监控异常行为模式，及时发现和处理垃圾账号

### 7.5 数据安全

- 所有数据传输使用HTTPS加密
- 数据存储加密，保护敏感信息
- 定期数据备份，确保数据安全和可恢复性
- 实施访问控制策略，限制对数据库的访问
- 记录关键操作日志，便于审计和追踪

## 8. 性能优化

### 8.1 数据查询优化

- 使用适当的索引，提高查询性能
- 优化数据库查询语句，减少查询时间
- 实现数据分页，避免一次性加载大量数据
- 使用缓存机制，缓存频繁访问的数据
- 对复杂查询进行性能分析和优化

### 8.2 并发处理

- 使用异步处理处理耗时操作，提高系统响应速度
- 实现请求队列，避免系统过载
- 对热点数据实施并发控制，防止数据竞争
- 使用分布式锁或乐观锁解决并发问题
- 负载均衡，分散系统压力

### 8.3 消息通知优化

- 使用消息队列处理大量通知，提高系统吞吐量
- 实现批量通知处理，减少数据库操作
- 支持通知优先级，确保重要通知优先发送
- 提供多渠道通知（站内信、邮件、推送等），但避免过度通知
- 实现通知的智能聚合，减少通知数量

### 8.4 缓存策略

- 对热点数据（如热门话题、最新活动等）进行缓存
- 使用多级缓存（内存缓存、分布式缓存等）提高缓存效率
- 实现缓存失效策略，确保数据一致性
- 对缓存数据进行监控和统计，优化缓存命中率
- 使用缓存预热，提前加载热点数据

### 8.5 系统扩展性

- 采用微服务架构，各服务独立部署和扩展
- 数据库分库分表，支持大规模数据存储
- 使用消息队列实现服务间的异步通信
- 设计可扩展的API，支持未来功能扩展
- 监控系统性能，及时发现和解决性能瓶颈

## 9. 总结

社区服务是OpenMind Lab系统的重要组成部分，旨在促进研究人员之间的交流与协作。本文档详细描述了社区服务的设计方案，包括功能架构、数据模型、API接口、服务实现、安全考虑和性能优化等方面。通过合理的设计和实现，社区服务能够为科研人员提供一个高效、安全、活跃的科研社区平台，支持用户社区、论坛讨论、活动组织等核心功能。未来，社区服务可以根据用户需求和技术发展进行扩展和优化，如增强社交功能、改进推荐算法、支持更多类型的活动和互动方式等，进一步提升用户体验和社区价值。
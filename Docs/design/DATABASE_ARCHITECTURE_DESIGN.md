# OpenMind Lab 数据库架构设计文档

## 1. 数据库架构概述

OpenMind Lab 采用多数据库策略，根据不同类型的数据特点和访问需求，选择合适的数据库类型进行存储。本文档详细描述了 OpenMind Lab 的数据库架构、数据模型设计和存储方案。

## 2. 数据库分层架构

OpenMind Lab 的数据库架构分为以下几层：

### 2.1 关系型数据库层

用于存储结构化数据，如用户信息、项目信息等，支持事务性操作和复杂查询。

### 2.2 NoSQL数据库层

用于存储非结构化或半结构化数据，如知识图谱、日志等，支持高并发读写和灵活的数据模型。

### 2.3 文件存储层

用于存储各种类型的文件，如数据集、文档、模型等，支持大容量存储和高效访问。

### 2.4 缓存层

用于缓存热点数据，提高数据访问性能，减轻数据库负载。

### 2.5 数据集成层

用于实现不同数据库之间的数据同步和集成，确保数据的一致性和完整性。

## 3. 数据库技术选型

### 3.1 关系型数据库

- **PostgreSQL**：作为主要的关系型数据库，用于存储用户、项目、计算任务等核心业务数据
- **MySQL**：作为辅助关系型数据库，用于存储日志、统计数据等

### 3.2 NoSQL数据库

- **MongoDB**：用于存储非结构化或半结构化数据，如用户偏好、系统配置等
- **Elasticsearch**：用于全文检索和日志分析
- **Neo4j**：用于存储和查询知识图谱数据
- **Redis**：用于缓存、消息队列和分布式锁

### 3.3 文件存储

- **MinIO**：自建对象存储服务，用于存储各类文件
- **Amazon S3**：可选的云存储服务，用于数据备份和容灾

### 3.4 时序数据库

- **InfluxDB**：用于存储监控数据和时序数据
- **TimescaleDB**：基于PostgreSQL的时序数据库扩展

## 4. 核心数据模型设计

### 4.1 用户数据模型

```sql
-- PostgreSQL 用户表
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    display_name VARCHAR(100),
    avatar_url VARCHAR(255),
    role VARCHAR(20) NOT NULL DEFAULT 'user',
    status VARCHAR(20) NOT NULL DEFAULT 'active',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_login_at TIMESTAMP,
    -- 索引
    CONSTRAINT users_username_unique UNIQUE (username),
    CONSTRAINT users_email_unique UNIQUE (email)
);

-- 用户索引
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_users_status ON users(status);
```

### 4.2 项目数据模型

```sql
-- PostgreSQL 项目表
CREATE TABLE projects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    owner_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    status VARCHAR(20) NOT NULL DEFAULT 'active',
    visibility VARCHAR(20) NOT NULL DEFAULT 'private',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_accessed_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_projects_owner_id FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 项目索引
CREATE INDEX idx_projects_owner_id ON projects(owner_id);
CREATE INDEX idx_projects_status ON projects(status);
CREATE INDEX idx_projects_visibility ON projects(visibility);

-- 项目成员关联表
CREATE TABLE project_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role VARCHAR(20) NOT NULL DEFAULT 'member',
    joined_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 唯一约束
    CONSTRAINT project_members_unique UNIQUE (project_id, user_id),
    -- 索引
    CONSTRAINT fk_project_members_project_id FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE CASCADE,
    CONSTRAINT fk_project_members_user_id FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 项目成员索引
CREATE INDEX idx_project_members_project_id ON project_members(project_id);
CREATE INDEX idx_project_members_user_id ON project_members(user_id);
```

### 4.3 计算任务数据模型

```sql
-- PostgreSQL 计算任务表
CREATE TABLE computing_tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type VARCHAR(50) NOT NULL DEFAULT 'default',
    parameters JSONB NOT NULL DEFAULT '{}',
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    priority INTEGER NOT NULL DEFAULT 1,
    progress INTEGER NOT NULL DEFAULT 0,
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    result_url VARCHAR(255),
    error_message TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_computing_tasks_project_id FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE CASCADE,
    CONSTRAINT fk_computing_tasks_user_id FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 计算任务索引
CREATE INDEX idx_computing_tasks_project_id ON computing_tasks(project_id);
CREATE INDEX idx_computing_tasks_user_id ON computing_tasks(user_id);
CREATE INDEX idx_computing_tasks_status ON computing_tasks(status);
CREATE INDEX idx_computing_tasks_type ON computing_tasks(type);
CREATE INDEX idx_computing_tasks_created_at ON computing_tasks(created_at);
```

### 4.4 数据存储数据模型

```sql
-- PostgreSQL 数据存储表
CREATE TABLE data_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    project_id UUID NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
    owner_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type VARCHAR(50) NOT NULL DEFAULT 'file',
    format VARCHAR(50),
    size BIGINT NOT NULL DEFAULT 0,
    storage_path VARCHAR(255) NOT NULL,
    checksum VARCHAR(100) NOT NULL,
    visibility VARCHAR(20) NOT NULL DEFAULT 'private',
    metadata JSONB NOT NULL DEFAULT '{}',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    last_accessed_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_data_items_project_id FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE CASCADE,
    CONSTRAINT fk_data_items_owner_id FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 数据存储索引
CREATE INDEX idx_data_items_project_id ON data_items(project_id);
CREATE INDEX idx_data_items_owner_id ON data_items(owner_id);
CREATE INDEX idx_data_items_type ON data_items(type);
CREATE INDEX idx_data_items_visibility ON data_items(visibility);

-- 数据共享关联表
CREATE TABLE data_shares (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    data_id UUID NOT NULL REFERENCES data_items(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    permission VARCHAR(20) NOT NULL DEFAULT 'view',
    shared_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 唯一约束
    CONSTRAINT data_shares_unique UNIQUE (data_id, user_id),
    -- 索引
    CONSTRAINT fk_data_shares_data_id FOREIGN KEY (data_id) REFERENCES data_items(id) ON DELETE CASCADE,
    CONSTRAINT fk_data_shares_user_id FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 数据共享索引
CREATE INDEX idx_data_shares_data_id ON data_shares(data_id);
CREATE INDEX idx_data_shares_user_id ON data_shares(user_id);
```

### 4.5 协作消息数据模型

```sql
-- PostgreSQL 协作消息表
CREATE TABLE collaboration_messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    sender_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    receiver_id UUID REFERENCES users(id) ON DELETE CASCADE,
    project_id UUID REFERENCES projects(id) ON DELETE CASCADE,
    type VARCHAR(20) NOT NULL DEFAULT 'text',
    content TEXT NOT NULL,
    is_read BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_collaboration_messages_sender_id FOREIGN KEY (sender_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT fk_collaboration_messages_receiver_id FOREIGN KEY (receiver_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT fk_collaboration_messages_project_id FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE CASCADE
);

-- 协作消息索引
CREATE INDEX idx_collaboration_messages_sender_id ON collaboration_messages(sender_id);
CREATE INDEX idx_collaboration_messages_receiver_id ON collaboration_messages(receiver_id);
CREATE INDEX idx_collaboration_messages_project_id ON collaboration_messages(project_id);
CREATE INDEX idx_collaboration_messages_type ON collaboration_messages(type);
CREATE INDEX idx_collaboration_messages_is_read ON collaboration_messages(is_read);
CREATE INDEX idx_collaboration_messages_created_at ON collaboration_messages(created_at);
```

### 4.6 知识图谱数据模型

```cypher
-- Neo4j 知识图谱节点和关系模型

-- 实体节点标签
:Entity
:Concept
:Person
:Organization
:Publication
:Project
:Dataset
:Model
:Tool

-- 关系类型
:RELATED_TO
:HAS_PART
:IS_A
:AUTHOR_OF
:PARTICIPATED_IN
:CITES
:USES
:CONTAINS
:GENERATED_BY
```

### 4.7 知识商城数据模型

```sql
-- PostgreSQL 知识商品表
CREATE TABLE knowledge_products (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    seller_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    description TEXT NOT NULL,
    category VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
    currency VARCHAR(10) NOT NULL DEFAULT 'USD',
    status VARCHAR(20) NOT NULL DEFAULT 'draft',
    download_url VARCHAR(255),
    preview_url VARCHAR(255),
    rating DECIMAL(3, 2) DEFAULT 0.00,
    sales_count INTEGER DEFAULT 0,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_knowledge_products_seller_id FOREIGN KEY (seller_id) REFERENCES users(id) ON DELETE CASCADE
);

-- 知识商品索引
CREATE INDEX idx_knowledge_products_seller_id ON knowledge_products(seller_id);
CREATE INDEX idx_knowledge_products_category ON knowledge_products(category);
CREATE INDEX idx_knowledge_products_status ON knowledge_products(status);
CREATE INDEX idx_knowledge_products_rating ON knowledge_products(rating);

-- 订单表
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    buyer_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    product_id UUID NOT NULL REFERENCES knowledge_products(id) ON DELETE CASCADE,
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(10) NOT NULL DEFAULT 'USD',
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    payment_method VARCHAR(50),
    payment_transaction_id VARCHAR(100),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    -- 索引
    CONSTRAINT fk_orders_buyer_id FOREIGN KEY (buyer_id) REFERENCES users(id) ON DELETE CASCADE,
    CONSTRAINT fk_orders_product_id FOREIGN KEY (product_id) REFERENCES knowledge_products(id) ON DELETE CASCADE
);

-- 订单索引
CREATE INDEX idx_orders_buyer_id ON orders(buyer_id);
CREATE INDEX idx_orders_product_id ON orders(product_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at);
```

## 5. 数据分片与分区策略

为了提高系统的可扩展性和性能，OpenMind Lab 采用了数据分片和分区策略：

### 5.1 水平分片

- **用户分片**：根据用户ID范围将用户数据分布到不同的数据库实例
- **项目分片**：根据项目ID范围将项目数据分布到不同的数据库实例
- **地理位置分片**：根据用户和项目的地理位置信息进行分片

### 5.2 垂直分区

- **按功能分区**：将不同功能模块的数据存储在不同的数据库中
- **按访问频率分区**：将频繁访问的数据和不频繁访问的数据分开存储

### 5.3 时间分区

- **日志数据**：按时间范围分区，便于数据归档和查询
- **监控数据**：按时间范围分区，支持冷热数据分离

## 6. 数据备份与恢复策略

OpenMind Lab 采用多层次的数据备份和恢复策略，确保数据的安全性和可靠性：

### 6.1 备份策略

- **全量备份**：定期对所有数据库进行全量备份
- **增量备份**：每天对数据库进行增量备份
- **日志备份**：实时备份数据库事务日志
- **异地备份**：将备份数据存储在异地，提高容灾能力

### 6.2 恢复策略

- **快速恢复**：关键数据支持快速恢复，减少业务中断时间
- **点恢复**：支持将数据库恢复到指定的时间点
- **测试恢复**：定期进行恢复测试，验证备份数据的可用性

## 7. 数据安全与隐私保护

OpenMind Lab 重视数据安全和隐私保护，采取了多种安全措施：

### 7.1 数据加密

- **传输加密**：使用HTTPS、TLS等协议加密数据传输
- **存储加密**：敏感数据在存储时进行加密
- **密钥管理**：采用安全的密钥管理系统，定期轮换密钥

### 7.2 访问控制

- **基于角色的访问控制**：细粒度控制用户对数据的访问权限
- **访问审计**：记录所有数据访问操作，便于审计和问题排查
- **数据脱敏**：在非生产环境中使用脱敏数据

### 7.3 隐私保护

- **合规性**：遵守相关的数据隐私法规，如GDPR、CCPA等
- **用户授权**：收集和使用用户数据前获得用户授权
- **数据最小化**：仅收集和使用必要的数据

## 8. 数据一致性保证

OpenMind Lab 采用多种技术保证数据的一致性：

### 8.1 事务管理

- **ACID事务**：关键业务操作使用ACID事务保证数据一致性
- **分布式事务**：跨服务的操作使用分布式事务协调机制

### 8.2 数据同步

- **实时同步**：重要数据变更实时同步到相关系统
- **定期同步**：非实时数据定期进行同步
- **冲突解决**：建立数据冲突检测和解决机制

## 9. 总结

OpenMind Lab 的数据库架构采用了多数据库策略，根据不同类型的数据特点和访问需求选择合适的数据库类型。通过数据分片、备份恢复、安全保护和一致性保证等策略，确保了系统数据的可靠性、安全性和高性能。数据库设计充分考虑了系统的可扩展性和未来的业务增长需求，能够支持全球科研人员的协作和创新需求。
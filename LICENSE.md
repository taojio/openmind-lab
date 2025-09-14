# OpenMind Lab 开源许可证

<div align="center">
  <h1>OpenMind Lab</h1>
  <img src="./logo.png" alt="OpenMind Lab Logo" width="150"/> 
  <p>OpenMind Lab 项目采用双许可证模式：GPL-3.0 和 AGPL-3.0，根据不同组件的使用场景选择适当的许可证。</p>
</div>

## 📝 目录

- [许可证概览](#-许可证概览)
- [GPL-3.0 License](#-gpl-30-license)
  - [适用范围](#适用范围)
  - [主要条款](#主要条款)
  - [完整许可证文本](#完整许可证文本)
- [AGPL-3.0 License](#-agpl-30-license)
  - [适用范围](#适用范围-1)
  - [主要条款](#主要条款-1)
  - [完整许可证文本](#完整许可证文本-1)
- [组件许可证映射](#-组件许可证映射)
- [贡献者协议](#-贡献者协议)
- [第三方库许可](#-第三方库许可)
- [许可证选择说明](#-许可证选择说明)
- [常见问题](#-常见问题)

## 🔍 许可证概览

OpenMind Lab 项目采用双许可证模式（GPL-3.0 和 AGPL-3.0），主要基于以下考虑：

1. **保护开源精神**：确保项目的核心代码和衍生作品保持开源性质
2. **用户自由保障**：保障用户运行、研究、修改和分发软件的自由
3. **网络服务覆盖**：针对通过网络提供服务的场景，使用AGPL-3.0确保开源义务延伸到网络服务
4. **社区协作促进**：鼓励社区参与和协作，同时确保贡献的代码也受到开源保护

项目主要使用的许可证及其适用范围如下：

| 许可证类型 | 适用范围 | 主要特点 |
|------------|----------|----------|
| GPL-3.0 | 桌面应用、独立工具、离线组件 | 强开源保护、copyleft、衍生作品必须开源 |
| AGPL-3.0 | 网络服务、SaaS应用、云平台组件 | GPL-3.0的网络服务增强版、通过网络提供服务也需开源修改 |

## 📜 GPL-3.0 License

### 适用范围

GPL-3.0 适用于 OpenMind Lab 的以下组件：
- 桌面客户端应用
- 独立命令行工具
- 离线数据处理组件
- 无需网络连接即可运行的单机应用

### 主要条款

1. **copyleft 保护**：修改或基于GPL许可代码开发的衍生作品必须使用相同的GPL许可证
2. **用户四自由**：保障用户运行、研究、修改和分发软件的自由
3. **源码提供**：分发二进制版本时必须同时提供完整源代码或获取源代码的方法
4. **专利许可**：贡献者授予用户使用其专利的权利，防止专利诉讼
5. **兼容性**：与大多数copyleft许可证兼容，但与专有软件不兼容

### 完整许可证文本

GNU GENERAL PUBLIC LICENSE
Version 3, 29 June 2007

Copyright (C) 2007 Free Software Foundation, Inc.
<https://fsf.org/>
Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.

## Preamble

The GNU General Public License is a free, copyleft license for
software and other kinds of works.

The licenses for most software and other practical works are designed
to take away your freedom to share and change the works. By contrast,
the GNU General Public License is intended to guarantee your freedom to
share and change all versions of a program--to make sure it remains free
software for all its users.

We, the Free Software Foundation, use the GNU General Public License for
most of our software; it applies also to any other work released this
way by its authors. You can apply it to your programs, too.

When we speak of free software, we are referring to freedom, not price.
Our General Public Licenses are designed to make sure that you have
the freedom to distribute copies of free software (and charge for them if
you wish), that you receive source code or can get it if you want it,
that you can change the software or use pieces of it in new free programs,
and that you know you can do these things.

To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights. Therefore, you have
certain responsibilities if you distribute copies of the software, or if
you modify it: responsibilities to respect the freedom of others.

For example, if you distribute copies of such a program, whether gratis
or for a fee, you must pass on to the recipients the same freedoms that
you received. You must make sure that they, too, receive or can get the
source code. And you must show them these terms so they know their rights.

Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

For the developers' and authors' protection, the GPL clearly explains
that there is no warranty for this free software. For both users' and
authors' sake, the GPL requires that modified versions be marked as
changed, so that their problems will not be attributed erroneously to
authors of previous versions.

Some devices are designed to deny users access to install or run
modified versions of the software inside them, although the manufacturer
can do so. This is fundamentally incompatible with the aim of protecting
users' freedom to change the software. The systematic pattern of such
abuse occurs in the area of products for individuals to use, which is
precisely where it is most unacceptable. Therefore, we have designed this
version of the GPL to prohibit the practice for those products. If such
problems arise substantially in other domains, we stand ready to extend
this provision to those domains in future versions of the GPL, as needed.

Finally, every program is threatened constantly by software patents.
States should not allow patents to restrict development and use of
software on general-purpose computers, but in those that do, we wish
to avoid the special danger that patents applied to a free program
could make it effectively proprietary. To prevent this, the GPL assures
that patents cannot be used to render the program non-free.

The precise terms and conditions for copying, distribution and modification
follow.

## AGPL-3.0 License

### 适用范围

AGPL-3.0 适用于 OpenMind Lab 的以下组件：
- Web服务和API
- 云平台核心组件
- 在线协作系统
- 通过网络提供服务的SaaS应用
- 所有需要通过网络访问的服务器端组件

### 主要条款

AGPL-3.0 包含 GPL-3.0 的所有条款，并增加了针对网络服务的特殊条款：

1. **网络服务条款**：如果您将修改后的AGPL许可软件作为网络服务提供，必须向所有使用该服务的用户提供修改后的源代码
2. **copyleft 延伸**：确保通过网络提供的服务也受到copyleft保护
3. **服务器端源码**：要求公开服务器端运行的软件源代码
4. **用户自由保障**：保障通过网络使用软件的用户也能获得源代码

### 完整许可证文本

GNU AFFERO GENERAL PUBLIC LICENSE
Version 3, 19 November 2007

Copyright (C) 2007 Free Software Foundation, Inc.
<https://fsf.org/>
Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.

## Preamble

The GNU Affero General Public License is a free, copyleft license for
software and other kinds of works, specifically designed to ensure
cooperation with the community in the case of network server software.

The licenses for most software and other practical works are designed
to take away your freedom to share and change the works. By contrast,
the GNU General Public License, or GPL, is intended to guarantee your
freedom to share and change all versions of a program--to make sure it
remains free software for all its users.

When we speak of free software, we are referring to freedom, not price.
Our General Public Licenses are designed to make sure that you have
the freedom to distribute copies of free software (and charge for them if
you wish), that you receive source code or can get it if you want it,
that you can change the software or use pieces of it in new free programs,
and that you know you can do these things.

To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights. Therefore, you have
certain responsibilities if you distribute copies of the software, or if
you modify it: responsibilities to respect the freedom of others.

For example, if you distribute copies of such a program, whether gratis
or for a fee, you must pass on to the recipients the same freedoms that
you received. You must make sure that they, too, receive or can get the
source code. And you must show them these terms so they know their rights.

Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

A secondary benefit of defending all users' freedom is that improvements
to the software will propagate rapidly to all users. 

This is especially true when users can easily obtain and modify the source
code for a network server that they use. By contrast, the user of a
non-free network application typically has no such freedom. The developer
or other owner of that application can change the application's
functionality, look, and behavior at any time. Even if the application's
functionality seems desirable today, it might be seriously deficient
or even oppressive tomorrow.

For network server software, this problem is particularly acute: the users
are not individuals who have a copy of the software, but instead are
clients who send requests to a server that runs the software. In such a
case, even if the server software was released under the GPL, the users
cannot get the source code of the server they use (if it has been modified),
because the GPL's source code distribution requirement is triggered only
when the software is distributed.

The GNU Affero General Public License is designed specifically to ensure
that, in such cases, the modified source code becomes available to the
community. It requires the operator of a network server to provide the
source code of the modified version running there to the users of that
server. Therefore, public use of a modified version, on a publicly accessible
server, gives the public access to the source code of the modified version.

The precise terms and conditions for copying, distribution and modification
follow.

## 🔗 组件许可证映射

为了方便用户了解OpenMind Lab各组件的许可证情况，我们提供了以下组件许可证映射表：

| 组件名称 | 组件路径 | 许可证类型 | 适用条款 |
|----------|----------|------------|----------|
| API网关 | api-gateway/ | AGPL-3.0 | 网络服务组件 |
| 身份管理系统 | core-services/identity/ | AGPL-3.0 | 网络服务组件 |
| 项目协作平台 | core-services/collaboration/ | AGPL-3.0 | 网络服务组件 |
| 分布式计算框架 | science-engines/computation/ | AGPL-3.0 | 云平台核心组件 |
| 数据存储系统 | data-infrastructure/storage/ | AGPL-3.0 | 云平台核心组件 |
| 安全框架 | security-framework/ | AGPL-3.0 | 云平台核心组件 |
| Web客户端 | client-applications/web/ | AGPL-3.0 | Web应用组件 |
| 桌面客户端 | client-applications/desktop/ | GPL-3.0 | 桌面应用 |
| 移动客户端 | client-applications/mobile/ | GPL-3.0 | 离线应用 |
| 工具库 | client-applications/tools/ | GPL-3.0 | 独立工具 |
| 示例代码 | examples/ | GPL-3.0 | 示例代码 |
| 项目文档 | docs/ | GPL-3.0 | 文档 |
| 科学数据集 | data-sets/ | GPL-3.0 | 数据集 |
| 教程材料 | tutorials/ | GPL-3.0 | 教程 |
| 专业科学模型 | science-engines/models/professional/ | AGPL-3.0 | 云服务模型 |
| 专有研究案例 | research-cases/proprietary/ | AGPL-3.0 | 云服务案例 |

## 📋 贡献者协议

为了明确OpenMind Lab项目中贡献者的权利和义务，所有贡献者在提交代码、文档、数据或其他内容前，需要了解并同意以下贡献者协议：

1. **贡献者权利**：贡献者保留其贡献内容的版权，但授予项目永久、不可撤销、全球范围的许可使用权
2. **许可授予**：贡献者同意根据项目指定的许可证（GPL-3.0或AGPL-3.0）许可其贡献内容
3. **原创保证**：贡献者保证其贡献内容是原创的，或已获得适当的许可，不侵犯任何第三方的知识产权
4. **责任豁免**：贡献者同意免除项目及其维护者因使用其贡献内容而产生的任何法律责任
5. **协议变更**：项目可能会不时更新贡献者协议，但会在变更前通知所有贡献者

详细的贡献者协议请参考 [CONTRIBUTING.md](CONTRIBUTING.md) 文档。

## 📦 第三方库许可

OpenMind Lab项目依赖于多个第三方开源库和工具，这些库和工具各自有其自己的许可条款。我们尊重并遵守所有第三方库的许可要求，具体信息如下：

### 前端依赖

| 库名称 | 版本 | 许可证类型 | 来源 |
|--------|------|------------|------|
| React | 18.x | MIT License | [https://github.com/facebook/react](https://github.com/facebook/react) |
| TypeScript | 5.x | Apache License 2.0 | [https://github.com/microsoft/TypeScript](https://github.com/microsoft/TypeScript) |
| Redux | 4.x | MIT License | [https://github.com/reduxjs/redux](https://github.com/reduxjs/redux) |
| Material-UI | 5.x | MIT License | [https://github.com/mui/material-ui](https://github.com/mui/material-ui) |
| Axios | 1.x | MIT License | [https://github.com/axios/axios](https://github.com/axios/axios) |

### 后端依赖

| 库名称 | 版本 | 许可证类型 | 来源 |
|--------|------|------------|------|
| Node.js | 18.x | MIT License | [https://github.com/nodejs/node](https://github.com/nodejs/node) |
| Express | 4.x | MIT License | [https://github.com/expressjs/express](https://github.com/expressjs/express) |
| NestJS | 10.x | MIT License | [https://github.com/nestjs/nest](https://github.com/nestjs/nest) |
| GraphQL | 16.x | MIT License | [https://github.com/graphql/graphql-js](https://github.com/graphql/graphql-js) |
| MongoDB | 6.x | Server Side Public License (SSPL) | [https://www.mongodb.com/](https://www.mongodb.com/) |
| PostgreSQL | 15.x | PostgreSQL License | [https://www.postgresql.org/](https://www.postgresql.org/) |

### 科学计算依赖

| 库名称 | 版本 | 许可证类型 | 来源 |
|--------|------|------------|------|
| TensorFlow | 2.x | Apache License 2.0 | [https://github.com/tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) |
| PyTorch | 2.x | BSD 3-Clause License | [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch) |
| Apache Spark | 3.x | Apache License 2.0 | [https://github.com/apache/spark](https://github.com/apache/spark) |
| NumPy | 1.x | BSD 3-Clause License | [https://github.com/numpy/numpy](https://github.com/numpy/numpy) |
| SciPy | 1.x | BSD 3-Clause License | [https://github.com/scipy/scipy](https://github.com/scipy/scipy) |

### DevOps依赖

| 库名称 | 版本 | 许可证类型 | 来源 |
|--------|------|------------|------|
| Docker | 24.x | Apache License 2.0 | [https://github.com/docker/docker](https://github.com/docker/docker) |
| Kubernetes | 1.27.x | Apache License 2.0 | [https://github.com/kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) |
| Jenkins | 2.401.x | MIT License | [https://github.com/jenkinsci/jenkins](https://github.com/jenkinsci/jenkins) |

> 注意：以上列表仅包含主要依赖，完整的依赖列表请参考项目的 package.json 文件和其他依赖声明文件。

## 💡 许可证选择说明

OpenMind Lab项目选择双许可证模式（GPL-3.0/AGPL-3.0），主要基于以下考虑：

1. **保护开源精神**：选择强copyleft许可证，确保项目的核心代码和衍生作品保持开源性质，防止被闭源化或私有化。

2. **网络服务覆盖**：针对现代软件架构中网络服务日益重要的趋势，使用AGPL-3.0确保通过网络提供服务的组件也受到copyleft保护，防止"服务作为软件替代"（SaaS）模式规避开源义务。

3. **用户自由保障**：GPL系列许可证从根本上保障用户的四大自由：运行、研究、修改和分发软件的自由，这与OpenMind Lab项目的开放精神高度一致。

4. **社区协作促进**：强copyleft许可证鼓励真正的社区协作，确保所有贡献都能回馈社区，促进项目的可持续发展。

5. **兼容性考虑**：GPL-3.0和AGPL-3.0与许多其他开源许可证保持良好的兼容性，同时明确区分了开源与闭源的界限。

## ❓ 常见问题

### Q: 我可以将OpenMind Lab用于商业目的吗？

**A:** 可以。GPL-3.0和AGPL-3.0都允许商业使用，但有以下重要条件：
- 任何基于OpenMind Lab修改或衍生的作品也必须使用相同的许可证（GPL-3.0或AGPL-3.0）
- 如果您将修改后的AGPL-3.0组件作为网络服务提供，必须向所有用户提供修改后的源代码
- 必须保留原始版权声明和许可证文本

### Q: 我需要为基于OpenMind Lab开发的应用程序选择相同的许可证吗？

**A:** 是的。根据copyleft原则，如果您的应用程序包含或基于OpenMind Lab的代码，您需要为您的应用程序选择相同的许可证：
- 如果使用GPL-3.0许可的组件，您的应用程序必须使用GPL-3.0
- 如果使用AGPL-3.0许可的组件，您的应用程序必须使用AGPL-3.0
- 仅使用API或通过进程间通信调用不构成衍生作品的情况除外

详细信息请咨询专业的法律顾问。

### Q: 如何确认某个特定组件的许可证？

**A:** 您可以通过以下方式确认组件的许可证：
- 查看组件目录下的LICENSE文件
- 参考本文件中的[组件许可证映射](#-组件许可证映射)表
- 查阅项目的源代码注释和package.json文件
- 如有疑问，请联系项目维护团队 [licensing@openmind-lab.org](mailto:licensing@openmind-lab.org)

### Q: 我可以将OpenMind Lab的代码或内容用于我的研究论文或报告吗？

**A:** 可以。只要您按照相应许可证的要求进行适当的署名和引用，就可以在研究论文或报告中使用OpenMind Lab的代码或内容。具体要求请参考各许可证的详细条款。

### Q: 项目的许可证会变更吗？

**A:** 我们会尽量保持许可证的稳定性，但在必要时可能会对某些组件的许可证进行调整。任何重大的许可证变更都会提前通知社区，并给予足够的过渡期。我们承诺，任何许可证变更都将保持项目的开源性质和社区友好性。

---

<div align="center">
  <p>OpenMind Lab 团队致力于维护开源精神，保护贡献者权益，促进科学知识的开放共享。</p>
  <p>如有任何关于许可证的问题或建议，请联系我们：<a href="mailto:licensing@openmind-lab.org">licensing@openmind-lab.org</a></p>
</div>

> 最后更新时间：2024年4月
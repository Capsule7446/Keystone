# Keystone

Keystone 是一个 **Claude Code 插件市场(Marketplace)**。

它采用**引用(索引)模式**:本仓库只维护一份插件清单
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json),每个插件的
实际代码托管在各自独立的仓库中,通过 `source` 字段以引用方式指向。
**Keystone 本身不存放任何插件代码,只做索引。**

## 给使用者:添加并安装插件

在 Claude Code 中添加本市场:

```
/plugin marketplace add Capsule7446/Keystone
```

浏览与安装插件:

```
/plugin                                  # 打开插件管理界面
/plugin install <plugin-name>@keystone   # 安装指定插件
```

管理市场:

```
/plugin marketplace list             # 查看已添加的市场
/plugin marketplace update keystone  # 刷新本市场清单
/plugin marketplace remove keystone  # 移除本市场
```

## 给维护者:注册一个新插件

每个插件托管在它自己的仓库里。要发布到 Keystone,只需编辑
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json),
在 `plugins` 数组中追加一个条目。`name` 与 `source` 为必填项。

下面是几种「引用外部仓库」的 `source` 写法模板:

### GitHub 仓库

```json
{
  "name": "my-plugin",
  "description": "插件简介",
  "source": {
    "source": "github",
    "repo": "owner/repo"
  }
}
```

可选地锁定分支 / 标签 / 提交:

```json
{
  "name": "my-plugin",
  "source": {
    "source": "github",
    "repo": "owner/repo",
    "ref": "v1.0.0",
    "sha": "完整的 40 位 commit SHA(可选,优先级高于 ref)"
  }
}
```

### 任意 Git 仓库(GitLab / 自建等)

```json
{
  "name": "my-plugin",
  "source": {
    "source": "url",
    "url": "https://gitlab.com/team/plugin.git",
    "ref": "main"
  }
}
```

### Monorepo 子目录

```json
{
  "name": "my-plugin",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/owner/monorepo.git",
    "path": "tools/claude-plugin"
  }
}
```

### npm 包

```json
{
  "name": "my-plugin",
  "source": {
    "source": "npm",
    "package": "@org/claude-plugin",
    "version": "^1.0.0"
  }
}
```

每个条目还支持可选元数据:`displayName`、`version`、`author`、`homepage`、
`repository`、`license`、`keywords`、`category`、`tags`、`defaultEnabled`。

> **版本管理**:在插件条目里写 `version` 可固定版本,用户只有在你提升版本号时才会更新;
> 省略 `version` 且使用 git 源时,每次提交都会被视为新版本(按 commit SHA)。

## 验证

发布前,在仓库根目录运行:

```
claude plugin validate .
```

> `plugins` 为空时会出现 `Marketplace has no plugins defined` 警告,这是正常的(非阻塞),
> 添加首个插件后即消失。

## 仓库结构

```
Keystone/
├── .claude-plugin/
│   └── marketplace.json   # 市场清单(唯一的核心文件)
├── README.md
└── LICENSE
```

## 许可

[MIT](LICENSE)

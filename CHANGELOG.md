# 更新日志

## 未发布

### 修复

- README 补充 `funscript/develop/env.sh` 的正确使用方式：该脚本依赖仓库根目录下的
  `pyproject.toml` 执行 `uv sync`，不能脱离仓库目录通过 `curl | bash` 单独运行，需
  先安装 uv 并克隆仓库到本地根目录再执行（已由异步流水线落地）。
- 修正 README 退出码示例中的语法错误 `if [$? -eq 0 ]`，改为合法写法
  `if touch ... ; then ... fi`（已由异步流水线落地）。

### 变更

- 更新 GitHub 仓库 description，准确描述 `env.sh` 用 `uv sync` 同步可复现开发
  环境的用途，删除不准确的"一键 pip 安装""curl 管道直接执行"表述。

## 0.1.0

### 新增

- 增加 MIT 协议项目元数据和环境项目构建例外说明。

### 修复

- 提高组织内部依赖下限，避免解析到迁移前的旧版本。

### 变更

- 将开发环境依赖迁移到 `pyproject.toml` 和 `uv.lock`。
- 环境脚本改为由 `uv sync` 管理并在失败时立即退出。

### 废弃

- 无。

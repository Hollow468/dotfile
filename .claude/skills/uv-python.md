# uv-python Skill

当需要使用Python运行代码或管理依赖时，默认使用uv进行操作。

## 触发条件

- 用户要求运行Python代码
- 用户要求安装Python包
- 用户提到Python脚本、Python程序
- AI需要执行Python代码
- 需要创建Python虚拟环境

## 指令

### 核心原则

**始终使用uv替代pip、python、venv等命令。** uv是下一代Python包管理器，速度比pip快10-100倍。

### 命令映射

| 传统命令 | uv替代命令 |
|---------|-----------|
| `python script.py` | `uv run script.py` |
| `pip install package` | `uv pip install package` |
| `pip install -r requirements.txt` | `uv pip install -r requirements.txt` |
| `python -m venv .venv` | `uv venv` |
| `python -c "code"` | `uv run python -c "code"` |
| `pip install package==1.0` | `uv pip install package==1.0` |

### 使用模式

#### 1. 直接运行脚本（推荐）
```bash
uv run script.py
```
uv会自动：
- 检测并创建虚拟环境（如需要）
- 安装脚本依赖
- 运行脚本

#### 2. 带依赖运行
```bash
# 在脚本顶部添加依赖声明
# /// script
# requires-python = ">=3.10"
# dependencies = ["requests", "pandas"]
# ///

uv run script.py
```

#### 3. 临时运行带依赖的代码
```bash
uv run --with requests --with pandas python -c "
import requests
import pandas as pd
print('Done')
"
```

#### 4. 安装包到项目
```bash
# 添加到项目依赖
uv add requests pandas

# 仅安装到虚拟环境
uv pip install requests pandas
```

#### 5. 创建虚拟环境
```bash
uv venv
# 或指定Python版本
uv venv --python 3.12
```

#### 6. 运行Python命令
```bash
uv run python -c "print('Hello')"
```

### 项目初始化

创建新Python项目时：
```bash
uv init project-name
cd project-name
```

这会创建：
- `pyproject.toml` - 项目配置
- `.python-version` - Python版本
- `main.py` - 入口文件

### 依赖管理

```bash
# 添加依赖
uv add requests
uv add "requests>=2.28"
uv add --dev pytest  # 开发依赖

# 移除依赖
uv remove requests

# 锁定依赖
uv lock

# 同步依赖
uv sync
```

### 工具运行

运行Python工具（如ruff、pytest等）：
```bash
uvx ruff check .
uvx pytest
uvx black .
```

## 示例

### 用户说：运行这个Python脚本

```bash
uv run script.py
```

### 用户说：安装pandas并运行数据分析

```bash
uv run --with pandas python analyze.py
```

### 用户说：创建一个Python项目

```bash
uv init my-project
cd my-project
uv add requests
```

### 用户说：测试代码

```bash
uv run --with pytest pytest test_*.py
```

## 注意事项

1. **不需要手动创建venv** - uv会自动管理
2. **不需要pip** - 全部用uv替代
3. **不需要python命令** - 用`uv run python`替代
4. **依赖声明** - 优先使用`uv add`而非手动编辑pyproject.toml
5. **锁文件** - uv会自动生成`uv.lock`确保可重复性

## 检查uv是否安装

```bash
uv --version
```

如未安装：
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

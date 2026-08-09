---
title: "Python 最佳实践速查"
summary: "日常 Python 开发中最常用的代码规范、最佳实践和写法推荐"
document_created_at: "2026-08-09"
document_updated_at: "2026-08-09"
tags: ["python", "programming", "example"]
related:
  - "../tools/git-cheatsheet.md"
confidence: "high"
---

# Python 最佳实践速查

> 适用于 Python 3.10+，收集日常编码中反复用到的好写法。

---

## 📌 核心原则

1. **可读 > 简洁**：一行能写完但看不懂，不如拆成三行
2. **用标准库**：能用 `pathlib`、`dataclasses`、`typing` 就别自己造
3. **加类型注解**：Type Hints 不是装饰，是给下一个看代码的人（包括未来的你）的礼物

---

## 📖 详细内容

### 1. 类型注解 (Type Hints)

```python
from typing import Optional, Union, List, Dict
from pathlib import Path

# 基础
def greet(name: str) -> str:
    return f"Hello, {name}"

# 可选参数 + 默认值
def read_file(path: str | Path, encoding: str = "utf-8") -> str:
    return Path(path).read_text(encoding=encoding)

# 可能为 None
def find_user(user_id: int) -> Optional[dict]:
    return db.get(user_id)  # Python 3.10+ 也可以写 dict | None

# 复杂结构
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int

def process_users(users: list[User]) -> list[str]:
    return [u["name"] for u in users]
```

### 2. 路径处理：用 pathlib，别用 os.path

```python
from pathlib import Path

# ✅ 好
p = Path("/home/user") / "docs" / "file.txt"
print(p.exists())        # True/False
print(p.read_text())     # 读文本
print(p.suffix)          # .txt
print(p.stem)            # file
print(p.parent)          # /home/user/docs

# ❌ 老写法（不要再用）
import os
os.path.exists(os.path.join("/home/user", "docs", "file.txt"))
```

### 3. 数据结构：dataclasses > 手写 dict

```python
from dataclasses import dataclass, field
from datetime import datetime

@dataclass
class Article:
    title: str
    content: str
    tags: list[str] = field(default_factory=list)
    created_at: datetime = field(default_factory=datetime.now)

    @property
    def summary(self) -> str:
        return self.content[:50] + "..."

# 使用
a = Article(title="Hello", content="World " * 20)
print(a.title)           # Hello
print(a.summary)         # World World World...
print(a)                 # 自动有好看的 __repr__
```

### 4. 文件读取：永远用 with

```python
# ✅ 安全，自动关闭
with open("a.txt", encoding="utf-8") as f:
    data = f.read()

# 按行读，内存友好
with open("big_file.txt", encoding="utf-8") as f:
    for line in f:
        process(line.strip())

# Pathlib 更简洁
from pathlib import Path
data = Path("a.txt").read_text(encoding="utf-8")
lines = Path("a.txt").read_text(encoding="utf-8").splitlines()
```

### 5. 字符串处理：f-string 优先

```python
name = "Alice"
age = 30

# ✅ 最推荐
s = f"{name} is {age} years old"

# 格式化数字
pi = 3.1415926
print(f"{pi:.2f}")       # 3.14
print(f"{pi:>8.2f}")     # "    3.14" 右对齐
```

### 6. 错误处理

```python
# ✅ 捕获具体异常，不要裸 except:
try:
    user = users[user_id]
    value = int(user["age"])
except KeyError as e:
    logger.error(f"User not found: {e}")
    return None
except ValueError as e:
    logger.error(f"Invalid age: {e}")
    return 0

# 需要清理资源时用 finally，或直接用 with
```

### 7. 虚拟环境 & 依赖

```bash
# 创建环境
python -m venv .venv

# 激活
source .venv/bin/activate   # Linux/Mac
.venv\Scripts\activate      # Windows

# 安装依赖并冻结版本
pip install requests rich
pip freeze > requirements.txt

# 其他人安装
pip install -r requirements.txt
```

---

## ⚠️ 常见坑

- **可变默认参数**：`def f(x=[]):` 永远不要这么写，用 `None` + 内部初始化
  ```python
  # ✅ 正确
  def f(x: list | None = None):
      x = x or []
  ```
- **迭代时修改列表**：不要在 `for item in mylist:` 循环里 `remove` / `append`，应该新建列表或用列表推导式
- **`is` vs `==`**：`None`、`True`、`False` 用 `is`，值比较用 `==`
- **相对 vs 绝对导入**：项目内一律用绝对导入 `from mypkg.utils import xxx`

---

## ✅ 最佳实践

1. **代码格式化统一用 Ruff**：`ruff format .` + `ruff check . --fix`，替代 Black + isort + flake8
2. **小函数，单一职责**：一个函数超过 50 行就该考虑拆分
3. **写 docstring**：至少给公共 API 写，说明参数、返回值和可能的异常
4. **用 `__main__` 保护入口**：
   ```python
   if __name__ == "__main__":
       main()
   ```
5. **日志替代 print**：用 `logging` 模块，别在正式代码里到处 `print`

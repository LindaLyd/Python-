## 【os/sys/datetime/random】系统性知识点

---

# 一、os模块（操作系统接口）

## 1. 路径操作（os.path）

| 函数 | 作用 | 示例 |
|------|------|------|
| `os.getcwd()` | 获取当前工作目录 | `os.getcwd()` → `/home/user` |
| `os.path.join(a, b)` | 智能拼接路径 | `os.path.join('a','b')` → `a/b` 或 `a\b` |
| `os.path.abspath(path)` | 转绝对路径 | `os.path.abspath('file.txt')` → `/home/user/file.txt` |
| `os.path.dirname(path)` | 取目录部分 | `os.path.dirname('/a/b/c.txt')` → `/a/b` |
| `os.path.basename(path)` | 取文件名部分 | `os.path.basename('/a/b/c.txt')` → `c.txt` |
| `os.path.split(path)` | 分割目录和文件名 | `os.path.split('/a/b/c.txt')` → `('/a/b', 'c.txt')` |
| `os.path.exists(path)` | 判断文件/目录是否存在 | `os.path.exists('/tmp')` → `True` |
| `os.path.isfile(path)` | 判断是否是文件 | `os.path.isfile('file.txt')` → `True` |
| `os.path.isdir(path)` | 判断是否是目录 | `os.path.isdir('/tmp')` → `True` |
| `os.path.getsize(path)` | 获取文件大小（字节） | `os.path.getsize('file.txt')` → `1024` |
| `os.path.splitext(path)` | 分割文件名和扩展名 | `os.path.splitext('a.txt')` → `('a', '.txt')` |

```python
import os

# 当前工作目录
cwd = os.getcwd()
print(cwd)  # /home/user/project

# 构建路径（跨平台）
file_path = os.path.join('data', 'logs', 'app.log')
# Linux/Mac: data/logs/app.log
# Windows: data\logs\app.log

# 获取绝对路径
abs_path = os.path.abspath('app.py')
print(abs_path)  # /home/user/project/app.py

# 获取目录和文件名
path = '/home/user/project/app.py'
dirname = os.path.dirname(path)   # /home/user/project
basename = os.path.basename(path) # app.py

# 判断存在
if os.path.exists('config.ini'):
    print("配置文件存在")
    
# 获取文件大小
size = os.path.getsize('data.csv')  # 字节数
```

## 2. 目录操作

| 函数 | 作用 | 示例 |
|------|------|------|
| `os.listdir(path)` | 列出目录内容 | `os.listdir('.')` → `['a.py', 'data']` |
| `os.mkdir(path)` | 创建单级目录 | `os.mkdir('new_folder')` |
| `os.makedirs(path)` | 创建多级目录 | `os.makedirs('a/b/c')` |
| `os.rmdir(path)` | 删除空目录 | `os.rmdir('empty_folder')` |
| `os.removedirs(path)` | 递归删除空目录 | `os.removedirs('a/b/c')` |
| `os.remove(path)` | 删除文件 | `os.remove('file.txt')` |
| `os.rename(src, dst)` | 重命名 | `os.rename('old.txt', 'new.txt')` |

```python
import os

# 列出当前目录
files = os.listdir('.')
for f in files:
    print(f)

# 创建目录
os.makedirs('data/2024/logs', exist_ok=True)  # exist_ok=True 不报错

# 删除文件
if os.path.exists('temp.txt'):
    os.remove('temp.txt')
```

## 3. 环境变量

```python
import os

# 获取环境变量
path = os.environ.get('PATH')        # 获取PATH，不存在返回None
home = os.environ.get('HOME', '/')   # 指定默认值

# 设置环境变量
os.environ['MY_VAR'] = 'my_value'

# 获取所有环境变量
for key, value in os.environ.items():
    print(f"{key}={value}")
```

## 4. 系统信息

```python
import os

# 获取操作系统名称
os.name          # 'posix' (Linux/Mac) 或 'nt' (Windows)

# 获取进程ID
os.getpid()      # 当前进程ID

# 执行系统命令
os.system('ls -l')  # 执行命令，返回退出码
```

---

# 二、sys模块（Python解释器）

## 1. 命令行参数

| 属性 | 作用 | 示例 |
|------|------|------|
| `sys.argv` | 命令行参数列表 | `sys.argv[0]` 是脚本名 |
| `sys.argv[1:]` | 脚本参数 | 运行时传入的参数 |

```python
import sys

# 假设运行: python script.py arg1 arg2 arg3
print(sys.argv)      # ['script.py', 'arg1', 'arg2', 'arg3']
print(sys.argv[0])   # script.py
print(sys.argv[1])   # arg1

# 常见用法
if len(sys.argv) > 1:
    filename = sys.argv[1]
else:
    filename = 'default.txt'
```

## 2. 路径和模块

| 属性/函数 | 作用 | 示例 |
|-----------|------|------|
| `sys.path` | 模块搜索路径列表 | 可动态添加路径 |
| `sys.modules` | 已导入模块字典 | `sys.modules.keys()` |
| `sys.executable` | Python解释器路径 | `/usr/bin/python3` |

```python
import sys

# 查看模块搜索路径
for path in sys.path:
    print(path)

# 添加自定义路径
sys.path.append('/my/custom/path')

# Python版本信息
print(sys.version)        # 3.10.0 (main, ...)
print(sys.version_info)   # sys.version_info(major=3, minor=10, ...)
print(sys.platform)       # 'linux', 'win32', 'darwin'
```

## 3. 程序控制

| 函数 | 作用 | 示例 |
|------|------|------|
| `sys.exit(code)` | 退出程序 | `sys.exit(0)` 正常退出 |
| `sys.stdin` | 标准输入 | `sys.stdin.read()` |
| `sys.stdout` | 标准输出 | `sys.stdout.write('hello')` |
| `sys.stderr` | 标准错误 | `sys.stderr.write('error')` |

```python
import sys

# 退出程序
if error_occurred:
    sys.exit(1)  # 非0表示异常退出

# 重定向输出
sys.stdout = open('output.txt', 'w')
print("这会被写入文件")
```

---

# 三、datetime模块（日期时间）

## 1. 核心类

| 类 | 说明 | 示例 |
|----|------|------|
| `datetime` | 日期+时间 | `datetime(2024, 1, 1, 12, 30, 0)` |
| `date` | 日期（年-月-日） | `date(2024, 1, 1)` |
| `time` | 时间（时:分:秒.微秒） | `time(12, 30, 0)` |
| `timedelta` | 时间差 | `timedelta(days=1, hours=2)` |

## 2. 获取当前时间

```python
from datetime import datetime, date, time, timedelta

# 当前日期时间
now = datetime.now()          # 2024-01-15 14:30:45.123456
today = date.today()          # 2024-01-15
current_time = datetime.now().time()  # 14:30:45.123456

# 获取各个部分
now.year      # 2024
now.month     # 1
now.day       # 15
now.hour      # 14
now.minute    # 30
now.second    # 45
now.weekday() # 0=周一, 6=周日
```

## 3. 创建指定时间

```python
# 创建datetime
dt = datetime(2024, 12, 25, 10, 30, 0)  # 2024-12-25 10:30:00

# 创建date
d = date(2024, 5, 1)  # 2024-05-01

# 创建time
t = time(14, 30, 15)  # 14:30:15
```

## 4. 时间计算（timedelta）

```python
from datetime import datetime, timedelta

now = datetime.now()

# 时间加减
tomorrow = now + timedelta(days=1)
yesterday = now - timedelta(days=1)
next_week = now + timedelta(weeks=1)
next_hour = now + timedelta(hours=1)
after_30_min = now + timedelta(minutes=30)
after_10_sec = now + timedelta(seconds=10)

# 计算时间差
start = datetime(2024, 1, 1)
end = datetime(2024, 1, 15)
delta = end - start
print(delta.days)        # 14
print(delta.seconds)     # 0（不含天数部分）
print(delta.total_seconds())  # 1209600.0
```

## 5. 格式化和解析（必考）

| 方法 | 作用 | 示例 |
|------|------|------|
| `strftime(format)` | datetime → 字符串 | `dt.strftime("%Y-%m-%d")` |
| `strptime(string, format)` | 字符串 → datetime | `datetime.strptime("2024-01-01", "%Y-%m-%d")` |

### 常用格式码

| 格式码 | 含义 | 示例 |
|--------|------|------|
| `%Y` | 四位年份 | 2024 |
| `%y` | 两位年份 | 24 |
| `%m` | 月份（01-12） | 01 |
| `%d` | 日期（01-31） | 15 |
| `%H` | 小时（00-23） | 14 |
| `%I` | 小时（01-12） | 02 |
| `%M` | 分钟（00-59） | 30 |
| `%S` | 秒（00-59） | 45 |
| `%A` | 星期几全名 | Monday |
| `%a` | 星期几缩写 | Mon |
| `%B` | 月份全名 | January |
| `%b` | 月份缩写 | Jan |
| `%p` | AM/PM | AM, PM |

```python
from datetime import datetime

# datetime → 字符串
now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M:%S"))      # 2024-01-15 14:30:45
print(now.strftime("%Y年%m月%d日"))            # 2024年01月15日
print(now.strftime("%A"))                     # Monday
print(now.strftime("%Y-%m-%d %I:%M %p"))      # 2024-01-15 02:30 PM

# 字符串 → datetime
date_str = "2024-12-25 10:30:00"
dt = datetime.strptime(date_str, "%Y-%m-%d %H:%M:%S")
print(dt)  # 2024-12-25 10:30:00
```

---

# 四、random模块（随机数）

## 1. 基本随机函数

| 函数 | 作用 | 示例 |
|------|------|------|
| `random.random()` | [0.0, 1.0) 浮点数 | `0.374` |
| `random.uniform(a, b)` | [a, b] 浮点数 | `random.uniform(1, 10)` → `5.23` |
| `random.randint(a, b)` | [a, b] 整数 | `random.randint(1, 10)` → `7` |
| `random.randrange(start, stop, step)` | 按步长随机 | `random.randrange(0, 10, 2)` → 偶数 |
| `random.choice(seq)` | 从序列随机选一个 | `random.choice(['a','b','c'])` → `'b'` |
| `random.choices(seq, k=n)` | 随机选n个（可重复） | `random.choices(lst, k=3)` |
| `random.sample(seq, k=n)` | 随机选n个（不重复） | `random.sample(lst, 3)` |
| `random.shuffle(lst)` | 原地打乱顺序 | `random.shuffle(lst)` |

```python
import random

# 随机浮点数
print(random.random())        # 0.0 ~ 1.0
print(random.uniform(1, 10))  # 1.0 ~ 10.0

# 随机整数
print(random.randint(1, 10))      # 包含两端 1~10
print(random.randrange(0, 10, 2)) # 0,2,4,6,8 中随机

# 随机选择
colors = ['red', 'green', 'blue']
print(random.choice(colors))           # 选一个
print(random.choices(colors, k=3))     # 选3个（可重复）
print(random.sample(colors, k=2))      # 选2个（不重复）

# 打乱顺序
cards = list(range(1, 53))
random.shuffle(cards)  # 原地打乱
```

## 2. 设置随机种子

```python
import random

# 设置种子（确保结果可复现）
random.seed(42)
print(random.randint(1, 10))  # 每次运行都相同

random.seed(42)
print(random.randint(1, 10))  # 与上面相同
```

---

# 五、典型用法示例

## 1. 遍历目录下的所有文件

```python
import os

def list_all_files(directory):
    for root, dirs, files in os.walk(directory):
        for file in files:
            full_path = os.path.join(root, file)
            print(full_path)

list_all_files('/home/user/project')
```

## 2. 批量重命名文件

```python
import os

def rename_files(directory, prefix):
    for filename in os.listdir(directory):
        if os.path.isfile(os.path.join(directory, filename)):
            new_name = prefix + filename
            os.rename(
                os.path.join(directory, filename),
                os.path.join(directory, new_name)
            )
```

## 3. 获取脚本所在目录

```python
import os
import sys

# 方法1：使用 __file__
script_dir = os.path.dirname(os.path.abspath(__file__))

# 方法2：使用 sys.argv[0]
script_dir = os.path.dirname(os.path.abspath(sys.argv[0]))
```

## 4. 日期范围遍历

```python
from datetime import datetime, timedelta

start = datetime(2024, 1, 1)
end = datetime(2024, 1, 31)

current = start
while current <= end:
    print(current.strftime("%Y-%m-%d"))
    current += timedelta(days=1)
```

## 5. 随机密码生成器

```python
import random
import string

def generate_password(length=8):
    chars = string.ascii_letters + string.digits + "!@#$%"
    return ''.join(random.choice(chars) for _ in range(length))

print(generate_password(12))  # 随机密码
```

---

# 六、练习题

## 题1：路径操作
```python
import os
print(os.path.join('a', 'b', 'c'))      # 答案：a/b/c（或 a\b\c）
print(os.path.dirname('/a/b/c.txt'))    # 答案：/a/b
print(os.path.basename('/a/b/c.txt'))   # 答案：c.txt
```

## 题2：获取当前目录
```python
import os
# 答案：os.getcwd()
```

## 题3：时间计算
```python
from datetime import datetime, timedelta
now = datetime(2024, 1, 15)
print((now + timedelta(days=7)).day)    # 答案：22
```

## 题4：日期格式化
```python
from datetime import datetime
dt = datetime(2024, 12, 25, 10, 30)
print(dt.strftime("%Y/%m/%d"))          # 答案：2024/12/25
```

## 题5：随机选择
```python
import random
lst = [1, 2, 3, 4, 5]
print(random.choice(lst))   # 答案：1-5中随机一个
print(random.sample(lst, 3)) # 答案：3个不同元素
```

---

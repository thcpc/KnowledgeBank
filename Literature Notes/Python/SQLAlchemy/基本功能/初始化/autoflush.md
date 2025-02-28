控制是否在每次查询或操作前自动刷新（flush）会话中的更改
- 如果 `autoflush=True`（默认值），SQLAlchemy 会在每次查询或操作前自动将未提交的更改刷新到数据库。
- 如果 `autoflush=False`，SQLAlchemy 不会自动刷新更改，需要显式调用 `session.flush()`。

## 使用场景

- **`autoflush=True`**：
    - 适用于大多数场景，确保查询结果包含最新的更改，保证查询一致性
    - 例如，如果你插入了一条新记录并立即查询，`autoflush=True` 会确保查询结果包含这条新记录。
- **`autoflush=False`**：
    - 适用于需要手动控制刷新时机的场景。
    - 例如，如果你希望在批量插入数据后再一次性刷新，可以禁用 `autoflush`。
## 例子
### 查询一致性
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine("sqlite:///mydatabase.db")
Session = sessionmaker(bind=engine, autoflush=True)  # 默认值
session = Session()

# 插入数据
session.add(some_object)
# 此时数据并未提交
# 查询数据（autoflush=True 会确保查询结果包含新插入的数据）
result = session.query(MyModel).filter_by(...).first()

session.commit()
```
### 批量提交
```python
session = Session(autoflush=False)
for item in large_list:
    session.add(item)
session.flush()  # 手动刷新
session.commit()
```
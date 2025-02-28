lazy 参数控制关联对象的加载方式，决定何时以及如何从数据库中加载关联对象

- lazy = "select" 默认
当访问关系属性时，SQLAlchemy 会发出一个 `SELECT` 查询来加载关联对象。
```python
class Parent(Base):
	__tablename__ = 'parents'
	id = Column(Integer, primary_key=True)
    children = relationship('Child', lazy='select')

	parent = session.query(Parent).first() # 此时并未关联查询子对象
	print(parent.children)  # 触发 SELECT 查询，此时再查询子对象
```
适合场景: 当关联对象不总是需要加载时，可以使用 `lazy="select"` 来延迟加载

- lazy = "joined" 
在加载父对象时，SQLAlchemy 会使用 `JOIN` 一次性加载关联对象。
```python
class Parent(Base):
    __tablename__ = 'parents'
    id = Column(Integer, primary_key=True)
    children = relationship('Child', lazy='joined')

parent = session.query(Parent).first() # 此时已加载关联对象进内存
print(parent.children)  # 已通过 JOIN 加载，无需额外查询
```
适合场景: 当关联对象总是需要加载时，可以使用 `lazy="joined"` 来减少查询次数。
- lazy = "subquery" 
在加载父对象时，SQLAlchemy 会使用 `JOIN` 一次性加载关联对象。
```python
class Parent(Base):
    __tablename__ = 'parents'
    id = Column(Integer, primary_key=True)
    children = relationship('Child', lazy='joined')

parent = session.query(Parent).first() # 此时已加载关联对象进内存
print(parent.children)  # 已通过子查询加载，无需额外查询
```
适合场景: 和 joined类似，当关联对象较多时，使用 `lazy="subquery"` 可以避免 `JOIN` 的性能问题。
- lazy = "dynamic"
返回一个查询对象，而不是直接加载关联对象。允许进一步过滤或分页加载关联对象
```python
class Parent(Base):
    __tablename__ = 'parents'
    id = Column(Integer, primary_key=True)
    children = relationship('Child', lazy='dynamic')

parent = session.query(Parent).first()
children = parent.children.filter(Child.name == "Alice").all()  # 返回查询对象
print(children)
```
- lazy = "noload"
不加载关联对象，访问关系属性时返回 `None` 或空列表
```python
class Parent(Base):
    __tablename__ = 'parents'
    id = Column(Integer, primary_key=True)
    children = relationship('Child', lazy='noload')

parent = session.query(Parent).first()
print(parent.children)  # 返回空列表
```
当明确不需要加载关联对象时，可以使用 `lazy="noload"`
# 2023-7-3

## 介绍

SQLAlchemy库用于与各种数据库交互，你可以使用一种类似于python类和语句的方式创建数据模型和查询。

## SQLAlchemy ORM

它提供了一种讲数据库模式和操作绑定到应用程序中使用的相同的数据对象上非常有效的方法，还提供了一种快速构建应用程序并交付给客户的方法。

### 使用ORM类定义表

满足下面四个要求：

- 继承自`declarative_base`对象
- 包含`__tablename__`,这是数据库中使用的表名
- 包含一个或者多个属性，他们都是`Colum`对象
- 确保一个或多个属性组成主键

```python
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Table, Column, Integer, Numeric, String, Boolean

Base = declarative_base()

class Cookie(Base):
    __tablename__ = 'cookies'

    cookie_id = Column(Integer(), primary_key=True)
    cookie_name = Column(String(50), index=True)
    cookie_recipe_url = Column(String(255))
    cookie_sku = Column(String(55))
    quantity = Column(Integer())
    unit_cost = Column(Numeric(12, 2))
```

### 关系

```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship, backref

class Order(Base):
    __tablename__ = 'orders'
    order_id = Column(Integer(), primary_key=True)
    user_id = Column(Integer(), ForeignKey('users.user_id'))
    shipped = Column(Boolean(), default=False)
    user =  relationship("User", backref=backref('orders', order_by=id))


class LineItems(Base):
    __tablename__ = 'line_items'
    line_items_id = Column(Integer(), primary_key=True)
    order_id = Column(Integer(), ForeignKey('orders.order_id'))
    cookie_id = Column(Integer(), ForeignKey('cookies.cookie_id'))
    quantity = Column(Integer())
    extended_cost = Column(Numeric(12, 2))
    order = relationship("Order", backref=backref('line_items', order_by=line_items_id))
    cookie = relationship("Cookie", uselist=False)
```

使用了`relationship`和`backref`方法

### 模式持久化

```python
from sqlalchemy import create_engine
engine = create_engine('sqlite:///:memory:')

Base.metadata.create_all(engine)
```

### 会话

会话是它和数据库交互的方式。

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine('sqlite:///:memory:')

Session = sessionmaker(bind=engine)

session = Session()
```

### 插入数据

1. 创建一个类的实例

```python
cc_cookie = Cookie(cookie_name='chocolate chip',                 									   cookie_recipe_url='http://some.aweso.me/cookie/recipe.html', 
                   cookie_sku='CC01', 
                   quantity=12, 
                   unit_cost=0.50)
```



2. 将实例添加到会话

```python
session.add(cc_cookie)
```



3. 提交会话

```python
session.commit()
```

(一般提交会话在最后使用)

一般使用如下方法：

```ptrhon
session.flush()
```

它不会执行数据库的提交并且结束事务，所以，实例仍然和会话相连接。

多条插入可以使用`bulk_save_objects`方法

但是会牺牲一些特性

### 查询

使用`query()`方法

```python
# 获取所有记录
cookies = session.query(Cookie).all()
print(cookies)
```

```python
# 将查询作为可迭代对象来遍历查询结果
for cookie in session.query(Cookie):  
    print(cookie)
```

除了`all()`方法，还有一些方法可以访问数据

- `first()`
  - 若有，就返回第一个记录对象
- `one()`
  - 查询所有行，如果返回的不是单个结果，就会抛出异常
- `scalar()`
  - 返回第一个结果的第一个元素。如果没有结果，返回None；若结果返回多个，就会引发错误。

#### 控制查询中的列数

将要查询的列传递给`query()`方法

```python
print(session.query(Cookie.cookie_name, Cookie.quantity).first())
```

#### 排序

使用`order_by()`语句

```python
for cookie in session.query(Cookie).order_by(Cookie.quantity):
    print('{:3} - {}'.format(cookie.quantity, cookie.cookie_name))
```

#### 限制返回结果数据的条数

- 可以直接使用列表的切片（但是在大型结果集来讲，做法的效率会很低）。

```python
query = session.query(Cookie).order_by(Cookie.quantity)[:2]
print([result.cookie_name for result in query])
```

- 使用`limit()`语句

```python
query = session.query(Cookie).order_by(Cookie.quantity).limit(2)
print([result.cookie_name for result in query])
```

#### 内置SQL函数和标签

常用的数据库函数就是`SUM()`和`COUNT()`

他们都在sqlalchemy.func模块中。

#### 过滤

过滤查询是通过在查询后面添加`filter()`语句完成的。

#### 运算符

也可以使用运算符来过滤数据

#### 连接词

`and_()`、`or_()`和`not_()`

```python
from sqlalchemy import and_, or_, not_
query = session.query(Cookie).filter(
    or_(
        Cookie.quantity.between(10, 50),
        Cookie.cookie_name.contains('chip')
    )
)
for result in query:
    print(result.cookie_name)
```

### 更新

`update()`方法

- 通过对象更新数据

```python
query = session.query(Cookie)
cc_cookie = query.filter(Cookie.cookie_name == "chocolate chip").first()
cc_cookie.quantity = cc_cookie.quantity + 120
session.commit()
print(cc_cookie.quantity)
```

- 就地更新数据

```python
query = session.query(Cookie)
query = query.filter(Cookie.cookie_name == "chocolate chip")
query.update({Cookie.quantity: Cookie.quantity - 20})

cc_cookie = query.first()
print(cc_cookie.quantity)
```

### 删除

- 获取对象删除

```python
query = session.query(Cookie)
query = query.filter(Cookie.cookie_name == "dark chocolate chip")
dcc_cookie = query.one()
session.delete(dcc_cookie)
session.commit()
dcc_cookie = query.first()
print(dcc_cookie)
```

- 就地删除

```python
query = session.query(Cookie)
query = query.filter(Cookie.cookie_name == "molasses")
query.delete()
mol_cookie = query.first()
print(mol_cookie)
```

### 连接

使用`join()`和`outerjoin()`方法来查询相关数据

```python
query = session.query(Order.order_id, User.username, User.phone,
                      Cookie.cookie_name, LineItem.quantity,
                      LineItem.extended_cost)
query = query.join(User).join(LineItem).join(Cookie)
results = query.filter(User.username == 'cookiemon').all()
print(results)
```

### 分组

```python
query = session.query(User.username, func.count(Order.order_id))
query = query.outerjoin(Order).group_by(User.username)
for row in query:
    print(row)
```

### 会话

会话状态

- transient（瞬时状态）
  - 实例不在会话中，也不在数据库中
- pending（挂起状态）
  - 实例由add()方法添加到会话中，但是没有刷新或者提交
- persistent（持久化状态）
  - 会话中的对象在数据库中有对应的记录
- detached（脱管状态）
  - 实例不再与会话相连，但是在数据库中仍然有一条记录

对一个实例选用inspect()方法时，可以看到当前的状态

## 事务

事务是一组语句，这组语句被看成一个整体，它的执行要么全部成功，要么全部失败。


##Python    ORM框架  ———SQLAlchemy
#####SQLAlchemy是Python编程语言下的一款ORM框架，该框架建立在数据库API之上，
#####使用关系对象映射进行数据库操作，简言之便是：将对象转换成SQL，然后使用数据API执行SQL并获取执行结果。
#####Object-Relational Mapping，把关系数据库的表结构映射到对象上
#####在虚拟环境安装  
```py
# 创建虚拟环境
virtualenv env
cd  env
pip install SQLAlchemy
```
######
#####SQLAlchemy本身无法操作数据库，其必须以来pymsql等第三方插件，Dialect用于和数据API进行交流，
#####根据配置文件的不同调用不同的数据库API，从而实现对数据库的操作，如：

```py
MySQL-Python
    mysql+mysqldb://<user>:<password>@<host>[:<port>]/<dbname>
  
pymysql
    mysql+pymysql://<username>:<password>@<host>/<dbname>[?<options>]
  
MySQL-Connector
    mysql+mysqlconnector://<user>:<password>@<host>[:<port>]/<dbname>

```
#####
#####首先导入包和链接数据库

```py

from sqlalchemy import Column, String, create_engine, CHAR, Integer, ForeignKey, DateTime

from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

from time import strftime, gmtime

# declarative_base() 创建了一个 BaseModel 类，这个类的子类可以自动与一个表关联。
db = declarative_base()
# 链接数据库返回一个db对象
# '数据库类型+数据库驱动名称://用户名:口令@机器地址:端口号/数据库名'
enging = create_engine('mysql+pymysql://root:666666@172.17.78.236:3306/testss', echo=True)

# 生成数据库会话类
DBSession = sessionmaker(bind=enging)



```

##
#####创建数据库映射表

```py
class User(db):
    __tablename__ = "user"
    id = Column(Integer, primary_key=True)
    name = Column(String(200))
    email = Column(String(200), unique=True)
    password = Column(String(32), nullable=True)
    telephone = Column(Integer)
    info = Column(String(255))
    create_at = Column(DateTime, nullable=False, default=strftime("%Y-%m-%d %H:%M:%S", gmtime()))
```
#
#####实例化数据库
```py
session = DBSession()
obj=User(name="Judy",password="264184")



```


#
#####数据库的基本操作 ——增删查改
```py
#增加数据
    def add_user(obj):
        error = None
        try:
            session.add(obj)
            session.commit()
        except Exception as err:
            session.rollback()
            error = str(err)
        finally:
            return error



```
```py
#删除数据
    def delete_user(id):
        error = None
        try:
            user = self.query.get(id)
            if user:
                session.delete(user)
                session.commit()
            else:
                error = "用户不存在，请输入正确的用户"
        except Exception as err:
            session.rollback()
            error = str(err)
        finally:
            return error


```
```py
#更新数据
   def update_user(id, name):User
        error = None
        try:
            user = .query.get(id)
            if user:
                user.name = name
                session.add(user)
                session.commit()
            else:
                error = "用户不存在，请输入正确的用户"
        except Exception as err:
            session.rollback()
            error = str(err)
        finally:
            return error


```
```py
#查询数据
    @classmethod
    def get_user(id=None):
        error = None
        user_value = None
        try:
            if id is not None:
                user = User.query.filter(id=id).first()
            else:
                user = User.query.all()
            if user:
                user_value = user
            else:
                error = "用户不存在，请输入正确的用户"
        except Exception as err:
            db.DBSession.rollback()
            error = str(err)
        finally:
            return error, user_value



```


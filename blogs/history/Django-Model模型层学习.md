---
title: Django Model模型层学习
date: 2017-11-29 10:09:51
tags: [Python, Django]
categories: Python
---
当编写一个数据库驱动的Web应用时，第一步就是定义该应用的模型 —— 本质上，就是定义该模型所对应的数据库设计及其附带的元数据。模型是你的数据的唯一的、权威的信息源，它包含你所储存数据的必要字段和行为。通常，每个模型对应数据库中唯一的一张表。
<!--more-->
## Model层流程
### 创建项目
终端执行：
```shell
$ django-admin startproject myproject
```
生成的项目目录：
```
myproject/
    manage.py
    myproject/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
这些文件是：
- 外层的myproject/根目录仅仅是项目的一个容器。它的命名对Django无关紧要，你可以把它重新命名为任何你喜欢的名字
- manage.py：一个命令行工具，可以使你用多种方式对Django项目进行交互
- 内层的myproject/目录是你的项目的真正的Python包。它是你导入任何东西时将需要使用的Python包的名字（例如 myproject.urls）
- myprpject/\__init\__.py：一个空文件，它告诉Python这个目录应该被看做一个Python包
- myproject/settings.py：该Django 项目的配置
- myproject/urls.py：该Django项目的URL声明；你的Django站点的“目录”
- myproject/wsgi.py：用于你的项目的与WSGI兼容的Web服务器入口

### 建立数据库
编辑项目下的settings.py，在DATABASES 'default'条目中修改：
  - 使用配置文件，如果使用PostgreSQL或者MySQL，确保已经建立好一个数据库
```python
#settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {
            'read_default_file': 'conf/db.cnf'
        },
    },
}

#conf/db.cnf
[client]
host = localhost
database = nero
user = root
password = 123
default-character-set = utf8
```
  - 直接在settings.py里配置：除了加入'ENGINE'，还需加上NAME、USER、PASSWORD、HOST等额外设置。

### 创建模型
首先创建应用，终端执行：
```shell
python manage.py startapp myapp
```

模型一般放在app下的models.py文件：
```python
# myapp/models.py
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=20)
    age = models.IntegerField('年龄', default=0)
```
1.每个模型都用一个类表示，该类继承自django.db.models.Model。每个模型都有一些类变量，在模型中每个类变量都代表了数据库中的一个字段。

2.每个字段通过Field类的一个实例表示——例如字符字段CharField和日期字段DateTimeField。这种方法告诉Django，每个字段中保存着什么类型的数据。

3.每个Field 实例的名字（例如name 或 age）就是字段的名字，并且是机器可读的格式。你将在Python代码中使用到它的值，并且你的数据库将把它用作表的列名。

4.可以使用Field的第一个参数来指定一个人类可读的名字，这是可选的。它在Django的内省机制中有使用，而且可以兼作文档。 如果没有提供这个参数，Django将使用那个机器可读的名字（实例名）。

### 激活模型
1. 将应用安装到Django，编辑项目settings.py：
```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',
)
```

2. 运行命令：python manager.py makemigrations myapp
会在该myapp下建立 migrations目录，并记录下你所有的关于modes.py的改动，比如0001_initial.py， 但是这个改动还没有作用到数据库文件
3. 运行命令：python manage.py sqlmigrate myapp 0001
sqlmigrate命令接收迁移文件的名字并返回它们的SQL语句，相当于查看文件，可以用来检查接下来需要执行的SQL操作
```
BEGIN;
--
-- Create model User
--
CREATE TABLE "myapp_user" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
    "name" varchar(20) NOT NULL, 
    "age" integer NOT NULL
);
COMMIT;
```
  - 表名是自动生成的，由app的名字（polls）和模型名字的小写字母组合而成，如myapp_user（可以重写此行为）
  - 主键（id）是自动添加的（可以重写此行为)
  - 按照惯例，Django会在外键的字段名后面添加 "_id"（可以重写此行为）
4. 运行命令：python manage.py migrate
migrate命令会找出所有还没有被应用的迁移文件（Django使用数据库中一个叫做django_migrations的特殊表来追踪哪些迁移文件已经被应用过），并且在你的数据库上运行它们 —— 本质上来讲，就是使你的数据库模式和你改动后的模型进行同步

## Model字段
### 字段选项
以下参数都是全部类型可用，某些字段类型还会有特殊的参数
#### null
- 如果为True，Django将在数据库中将空值存储为NULL。默认值是 False。
- 字符串字段例如CharField 和TextField 要避免使用null，因为空字符串值将始终储存为空字符串而不是NULL。如果字符串字段的null=True，那意味着对于“无数据”有两个可能的值：NULL 和空字符串。在大多数情况下，对于“无数据”声明两个值是赘余的，Django 的惯例是使用空字符串而不是NULL。
- 如果你希望BooleanField 接受null 值，请用 NullBooleanField 代替。

#### blank
- 如果为True，则该字段允许为空白。默认值是 False。
- 注意它与null不同。null 纯粹是数据库范畴的概念，而blank 是数据验证范畴的。如果字段设置blank=True，表单验证时将允许输入空值。如果字段设置blank=False，则该字段为必填。

#### choices
- 它是一个可迭代的结构（比如，列表或是元组），由可迭代的二元组组成（比如[(A, B), (A, B) ...]），用来给这个字段提供选择项。
- 每个元组中的第一个元素，是存储在数据库中的值；第二个元素是该选项更易理解的描述。比如：
```python
PARAGRAPH_TYPE = (
    (0, u'文本'),
    (1, u'图片'),
)
```

#### default
- 该字段的默认值。它可以是一个值或者一个可调用对象。如果是一个可调用对象，那么在每一次创建新对象的时候，它将会调用一次。
- 这个默认值不可以是一个可变对象（如字典，列表，等等），因为对于所有模型的一个新的实例来说，它们指向同一个引用。如果需要使用可变对象（如字典），可以把它包装成一个可调用对象：
```python
def contact_default():
    return {"email": "to1@example.com"}

contact_info = JSONField("ContactInfo", default=contact_default)
```
- 注意lambdas 函数不可作为如 default 这类可选参数的值，因为它们无法被 migrations命令序列化。

#### primary_key
- 若为 True, 则该字段会成为模型的主键字段。
- 如果你没有在模型的任何字段上指定 primary\_key=True, Django会自动添加一个 AutoField 字段来充当主键。 所以除非你想要覆盖默认的主键行为，否则不需要在任何字段上设定primary_key=True。
- primary_key=True 暗含着null=False 和unique=True. 一个对象上只能拥有一个主键。
- 主键字段是只读的。如果你改变了一个已存在对象上的主键并且保存的话，会创建一个新的对象，而不是覆盖旧的。
- 每个模型只能有一个字段指定primary\_key=True（无论是显式声明还是自动添加）。

#### unique
- 如果为 True, 这个字段在表中必须有唯一值。
- 除了ManyToManyField、OneToOneField和FileField 以外的其他字段类型都可以使用这个设置。
- 注意当设置unique 为True 时，你不需要再指定 db_index，因为unique 本身就意味着一个索引的创建。

#### db_index
- 若值为 True, 则 django-admin sqlindexes 将会为此字段输出 CREATE INDEX（创建索引）语句。

### 字段类型
#### AutoField
- 一个根据实际ID自动增长的IntegerField
- 默认情况下，Django 会给每个模型添加下面这个字段：
```python
id = models.AutoField(primary_key=True)
```
- 如果你想指定一个自定义主键字段，只要在某个字段上指定 primary\_key=True 即可。如果 Django 看到你显式地设置了 Field.primary\_key，就不会自动添加 id 列。

#### BooleanField
- true/false 字段。
- 如果Field.default没有指定的话， BooleanField 的默认值是 None。

#### CharField
- 一个用来存储从小到很大各种长度的字符串的地方，如果是巨大的文本类型, 可以用 TextField。
- 须接收一个额外的参数：max\_length

#### DateTimeField
- 它是通过Python datetime.datetime实例表示的日期和时间。
- 参数auto\_now：每次保存对象时，自动设置该字段为当前时间。
- 参数auto\_now\_add：当对象第一次被创建时自动设置当前时间。

#### FloatField
- 用Python的一个float 实例来表示一个浮点数。

#### IntegerField
- 一个整数。在Django所支持的所有数据库中，从 -2147483648 到 2147483647 范围内的值是合法的。

### 关联字段
#### ForeignKey
- 多对一关系。需要一个位置参数：与该模型关联的类。
- ForeignKey 会自动创建数据库索引。你可以通过设置db_index 为False 来取消。
- 在幕后，Django 会在字段名上添加"_id" 来创建数据库中的列名。
- ForeignKey.related_name：这个名称用于让关联的对象反查到源对象。注意，当你为抽象模型定义关联关系的时，必须设置这个参数的值。
- ForeignKey.to_field：关联到的关联对象的字段名称。默认地，Django 使用关联对象的主键。
- ForeignKey.on_delete：外键删除约束。有：CASCADE（默认值）、PROTECT（抛出ProtectedError 以阻止被引用对象的删除）、SET\_NULL、SET\_DEFAULT（ForeignKey 值设置成它的默认值；此时必须设置ForeignKey 的default 参数）、DO\_NOTHING等。

#### ManyToManyField
- 一个多对多关联。要求一个关键字参数：与该模型关联的类。
- 关联的对象可以通过字段的RelatedManager 添加、删除和创建。
- ManyToManyField.related_name：这个名称用于让关联的对象反查到源对象。

#### OneToOneField
- 一对一关联关系。概念上讲，这个字段很像是ForeignKey 设置了unique=True，不同的是它会直接返回关系另一边的单个对象。
- 需要一个位置参数：与该模型关联的类。

### 元选项
#### abstract
- 如果 abstract = True， 就表示模型是 抽象基类。
当你想将一些共有信息放进其它一些model的时候，抽象化类是十分有用的。你编写完基类之后，在 Meta类中设置 abstract=True ，这个模型就不会被用来创建任何数据表。取而代之的是，当它被用来作为一个其他model的基类时，它的字段将被加入那些子类中。如果抽象基类和它的子类有相同的字段名，那么将会出现error（并且Django将抛出一个exception）。

#### app_label
- 如果一个模型位于标准的位置之外（应用的models.py 或models 包），该模型必须定义它属于哪个应用。

#### db_table
- 该模型所用的数据表的名称。

#### ordering
- 对象默认的顺序，获取一个对象的列表时使用。
- 它是一个字符串的列表或元组。每个字符串是一个字段名，前面带有可选的“-”前缀表示倒序。前面没有“-”的字段表示正序。使用"?"来表示随机排序。先按照pub_date的倒序排序，再按照 author 的正序排序，这样写：
```python
ordering = ['-pub_date', 'author']
```

### 管理器
每个非抽象的Model 类必须给自己添加一个Manager实例。Django 确保在你的模型类中至少有一个默认的Manager。如果你没有添加自己的Manager，Django 将添加一个属性objects，它包含默认的Manager 实例。如果你添加自己的Manager实例的属性，默认值则不会出现。

## CURD操作
一旦你建立好数据模型，Django 会自动为你生成一套数据库抽象的API，可以让你创建、检索、更新和删除对象。
```python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=50)
    email = models.EmailField()

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    n_comments = models.IntegerField()
    n_pingbacks = models.IntegerField()
    rating = models.IntegerField()

    def __str__(self):              # __unicode__ on Python 2
        return self.headline
```
### 创建对象
1. save()方法
```python
b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
b.save()	# 返回保存实例
```
2. create()方法：如果你的model中有一个你手动设置主键，并且这个值已经存在于数据库中，调用 create()将会失败并且触发 IntegrityError，因为主键必须是唯一的
```python
Blog.objects.create(name='Beatles Blog', tagline='All the latest Beatles news.')	# 返回保存实例
```

### 保存对象改动
- save()方法
```python
b.name = 'New name'
b.save()	# update
```

#### 保存ForeignKey和ManyToManyField字段
- 更新ForeignKey 字段的方式和保存普通字段相同 —— 只要把一个正确类型的对象赋值给该字段
```python
entry = Entry.objects.get(pk=1)
cheese_blog = Blog.objects.get(name="Cheddar Talk")
entry.blog = cheese_blog
entry.save()
```
- 更新ManyToManyField 的方式有一些不同 —— 需要使用字段的**add()**方法来增加关联关系的记录（可以同时增加多个）
```python
joe = Author.objects.create(name="Joe")
entry.authors.add(joe)
```

### 获取对象
通过模型中的管理器构造一个查询集，来从你的数据库中获取对象。
#### 获取所有对象
```python
all_entries = Entry.objects.all()
```
#### 使用过滤器获取对象
- filter(**kwargs)：返回一个新的查询集，它包含满足查询参数的对象
- exclude(**kwargs)：返回一个新的查询集，它包含不满足查询参数的对象
```python
Entry.objects.filter(pub_date__year=2006)
```

1. 链式过滤：查询集的筛选结果本身还是查询集，所以可以将筛选语句链接在一起。
2. 过滤后的查询集是独立的：每次你筛选一个查询集，得到的都是全新的另一个查询集，它和之前的查询集之间没有任何绑定关系。每次筛选都会创建一个独立的查询集，它可以被存储及反复使用。
3. 查询集是惰性执行的：查询集 是惰性执行的 —— 创建查询集不会带来任何数据库的访问。你可以将过滤器保持一整天，直到查询集需要求值时，Django才会真正运行这个查询。

查询的关键字参数的基本形式是**field__lookuptype=value**：
- exact：“精确”匹配
- iexact：大小写不敏感的匹配
- contains：大小写敏感的包含关系测试（LIKE）
- startswith, endswith：分别表示以XXX开头和以XXX结尾，大小写敏感

#### 使用get获取单个对象
返回查到的第一个满足条件的对象
```python
one_entry = Entry.objects.get(pk=1)
```
注意：
如果没有结果满足查询，get() 将引发一个DoesNotExist 异常，这个异常是正在查询的模型类的一个属性；如果有多条记录满足get() 的查询条件，Django 也将报错。这种情况将引发MultipleObjectsReturned，它同样是模型类自身的一个属性。

#### 限制查询集
可以使用Python 的切片语法来限制查询集记录的数目 。它等同于SQL 的LIMIT 和OFFSET 子句。不支持负的索引（例如Entry.objects.all()[-1]）。
**注意**：通常，查询集 的切片返回一个新的查询集 —— 它不会执行查询。有一个例外，是如果你使用Python 切片语法中"step"参数。例如，下面的语句将返回前10 个对象中每隔2个对象，它将真实执行查询

#### 跨关联关系查询
Django 提供一种强大而又直观的方式来“处理”查询中的关联关系，它在后台自动帮你处理JOIN。 若要跨越关联关系，只需使用关联的模型字段的名称，并使用双下划线分隔，直至你想要的字段（这种跨越可以是任意的深度）：
```python
Entry.objects.filter(blog__name='Beatles Blog')
```

跨越多值（filter举例，exclude略有不同）：
1. 选择所有包含**同时满足**两个条件的entry的blog，这两个条件是headline 包含Lennon 和发表时间是2008 （同一个entry 满足两个条件）
```python
Blog.objects.filter(entry__headline__contains='Lennon', entry__pub_date__year=2008)
```
2. 选择满足以下条件的blog实例：blog的enrty的headline属性值是“Lennon”，**或者**entry的发表时间是2008（两个条件至少满足一个，也可以同时满足）
```python
Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)
```

#### 查询快捷方式pk
为了方便，Django 提供一个查询快捷方式pk ，它表示“primary key” 的意思。
在Blog 模型示例中，主键是id 字段，所以下面三条语句是等同的：
```python
Blog.objects.get(id__exact=14) # Explicit form
Blog.objects.get(id=14) # __exact is implied
Blog.objects.get(pk=14) # pk implies id__exact

Blog.objects.filter(pk__gt=14)
Entry.objects.filter(blog__pk=3) 
```

### 删除对象
```python
Blog.objects.get(id=14).delete()
Entry.objects.filter(pub_date__year=2005).delete()
```

### 一次更新多个对象
```python
Entry.objects.filter(pub_date__year=2007).update(headline='Everything is the same')
```
update() 方法会立即执行并返回查询匹配的行数（如果有些行已经具有新的值，返回的行数可能和被更新的行数不相等）。

### 关联的对象
#### 多对一、多对多关系
1. 前向查询
如果一个模型具有ForeignKey，那么该模型的实例将可以通过属性访问关联的（外部）对象。
```python
e = Entry.objects.get(id=2)
e.blog # Returns the related Blog object.
```
2. 反向查询
如果模型I有一个ForeignKey，那么该ForeignKey 所指的模型II实例可以通过一个管理器返回前面有ForeignKey的模型I的所有实例。默认情况下，这个管理器的名字为foo_set（你可以在ForeignKey 定义时设置related\_name 参数来覆盖foo\_set 的名称），其中foo 是源模型的小写名称。该管理器返回的查询集可以用上一节提到的方式进行过滤和操作。
```python
b = Blog.objects.get(id=1)
b.entry_set.all() # Returns all Entry objects related to Blog.

# b.entry_set is a Manager that returns QuerySets.
b.entry_set.filter(headline__contains='Lennon')
b.entry_set.count()
```
3. 处理关联对象的其他方法
  - add(obj1, obj2, ...)
    添加一指定的模型对象到关联的对象集中
  - create(**kwargs)
    创建一个新的对象，将它保存并放在关联的对象集中。返回新创建的对象
  - remove(obj1, obj2, ...)
    从关联的对象集中删除指定的模型对象
  - clear()
    从关联的对象集中删除所有的对象

#### 通过关联对象进行查询
当你指定查询需要匹配的一个值时，你可以使用一个对象实例或者对象的主键的值。
例如，如果你有一个id=5 的Blog 对象b，下面的三个查询将是完全一样的：
```python
Entry.objects.filter(blog=b) # Query using object instance
Entry.objects.filter(blog=b.id) # Query using id from instance
Entry.objects.filter(blog=5) # Query using id directly
```

### 使用原始SQL查询
```python
# 1.Manager.raw()方法，返回模型实例
Person.objects.raw('SELECT id, first_name, last_name, birth_date FROM myapp_person')

# 2.获取原始数据
from django.db import connection
def my_custom_sql(self):
    cursor = connection.cursor()
    cursor.execute("UPDATE bar SET foo = 1 WHERE baz = %s", [self.baz])
    cursor.execute("SELECT foo FROM bar WHERE baz = %s", [self.baz])
    row = cursor.fetchone()
    return row
```

## QuerySet API
除了上面使用到的，还有以下常用的API：
1. order_by
默认情况下，QuerySet 根据模型Meta 类的ordering 选项排序。你可以使用order_by 方法给每个QuerySet 指定特定的排序。
2. reverse
reverse() 方法反向排序QuerySet 中返回的元素。第二次调用reverse() 将恢复到原有的排序。
3. distinct
返回一个在SQL 查询中使用SELECT DISTINCT 的新QuerySet。它将去除查询结果中重复的行。
4. values
返回一个ValuesQuerySet —— QuerySet 的一个子类，迭代时返回字典而不是模型实例对象。
```python
Blog.objects.filter(name__startswith='Beatles').values()
# [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]

Blog.objects.values('id', 'name')
# [{'id': 1, 'name': 'Beatles Blog'}]
```
5. values_list
与values() 类似，只是在迭代时返回的是元组而不是字典。
```python
Entry.objects.values_list('id', 'headline')
# [(1, 'First entry'), ...]
```
6. all
返回当前QuerySet（或QuerySet 子类） 的副本。
7. select_related
返回一个QuerySet，当执行它的查询时它沿着外键关系查询关联的对象的数据。它会生成一个复杂的查询并引起性能的损耗，但是在以后使用外键关系时将不需要数据库查询。
8. get\_or\_create
一个通过给出的kwargs 来查询对象的便捷方法（如果你的模型中的所有字段都有默认值，可以为空），需要的话创建一个对象。
9. update\_or\_create
10. count()
返回在数据库中对应的 QuerySet.对象的个数。 count() 永远不会引发异常。
10. latest
使用作为日期字段提供的field_name，按日期返回表中的最新对象。
```python
Entry.objects.latest('pub_date')
```
11. earliest
除非方向更改，否则像latest()。
12. first
first()返回结果集的第一个对象, 当没有找到时返回None.如果 QuerySet 没有设置排序,则将会自动按主键进行排序
13. last
last()工作方式类似first()，只是返回的是查询集中最后一个对象。
14. exists
如果QuerySet 包含任何结果，则返回True，否则返回False。





------------------
> [Django 文档 — Django 1.8.2 中文文档](http://python.usyiyi.cn/translate/django_182/index.html)

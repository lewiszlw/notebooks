---
title: Django-REST-framework Serializers学习
date: 2017-12-07 11:39:01
tags: [Django-REST-framework, Python, Django]
categories: Python
---
Serializers（序列化器）允许诸如查询集和模型实例等复杂的数据转换为Python内置数据类型，然后可以方便渲染成JSON，XML或其他内容类型。Serializers还提供反序列化，在首次验证传入数据之后，可以将解析的数据转换回复杂的类型。
<!--more-->
## 序列化器基础
### 声明
以Comment对象为例：
```python
from datetime import datetime

class Comment(object):
    def __init__(self, email, content, created=None):
        self.email = email
        self.content = content
        self.created = created or datetime.now()

comment = Comment(email='leila@example.com', content='foo bar')
```
继承Serializer来声明是一个序列化器：
```python
from rest_framework import serializers

class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```
CommentSerializer可以用来序列化和反序列话与Comment对象相关的数据。

### 序列化
序列化可以将Comment对象序列化为一个dict：
```python
serializer = CommentSerializer(comment)
serializer.data
# {'email': 'leila@example.com', 'content': 'foo bar', 'created': '2016-01-27T15:17:10.375877'}

json = JSONRenderer().render(serializer.data)
json
# b'{"email":"leila@example.com","content":"foo bar","created":"2016-01-27T15:17:10.375877"}'
```

### 反序列化
反序列化可以将dict内置数据类型转化为相应的object：
```python
serializer = CommentSerializer(data=data) # data是dict类型
serializer.is_valid()
# True
serializer.validated_data
# {'content': 'foo bar', 'email': 'leila@example.com', 'created': datetime.datetime(2012, 08, 22, 16, 20, 09, 822243)}
```

### 保存实例
如果想基于验证数据返回完整的对象实例，需要实现相应方法：`create()`和`update()`。
```python
class CommentSerializer(serializers.Serializer):
    email = serializers.EmailField()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()

    def create(self, validated_data):
        return Comment(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        return instance
```
如果对象实例是Django模型，并且你需要确保这些方法能将对象保存到数据库：
```python
    def create(self, validated_data):
        return Comment.objects.create(**validated_data)

    def update(self, instance, validated_data):
        instance.email = validated_data.get('email', instance.email)
        instance.content = validated_data.get('content', instance.content)
        instance.created = validated_data.get('created', instance.created)
        instance.save()
        return instance
```
当反序列化数据后，可以调用**save()**方法来返回一个基于验证数据的对象实例：
```python
comment = serializer.save()
```
调用**save()**方法会创建一个新实例，或者更新一个已有实例，取决于实例化序列化器时是否传递了一个已经存在的实例：
```python
# .save() will create a new instance.
serializer = CommentSerializer(data=data)

# .save() will update the existing `comment` instance.
serializer = CommentSerializer(comment, data=data)
```

### 验证
当反序列化数据时，在你打算获取验证数据或者保持对象实例前，你通常需要调用`is_valid()`。如果验证发送错误，`errors`属性将显示错误信息：
```python
serializer = CommentSerializer(data={'email': 'foobar', 'content': 'baz'})
serializer.is_valid()
# False
serializer.errors
# {'email': [u'Enter a valid e-mail address.'], 'created': [u'This field is required.']}
```
#### raise_exception
is\_valid()方法带有一个可选`raise_exception`标志，如果引发serializers.ValidationError默认会返回HTTP 400 Bad Request响应。
```python
serializer.is_valid(raise_exception=True)
```
#### 字段级验证
可以通过添加`validate_<field_name>()`方法自定义字段级验证，这些方法只有一个参数——需要验证的字段值，它返回字段值或者引发ValidationError异常：
```python
class BlogPostSerializer(serializers.Serializer):
    title = serializers.CharField(max_length=100)
    content = serializers.CharField()

    def validate_title(self, value):
        if 'django' not in value.lower():
            raise serializers.ValidationError("Blog post is not about Django")
        return value
```
#### 对象级验证
需要访问多个字段进行验证，可以添加`validate()`方法，它只有一个参数——字段值组成的dict，返回dict或者引发ValidationError异常：
```python
class EventSerializer(serializers.Serializer):
    description = serializers.CharField(max_length=100)
    start = serializers.DateTimeField()
    finish = serializers.DateTimeField()

    def validate(self, data):
        if data['start'] > data['finish']:
            raise serializers.ValidationError("finish must occur after start")
        return data
```

#### 验证器
序列化器的每个字段可以在声明时传入一个或多个验证器：
```python
def multiple_of_ten(value):
    if value % 10 != 0:
        raise serializers.ValidationError('Not a multiple of ten')

class GameRecord(serializers.Serializer):
    score = IntegerField(validators=[multiple_of_ten])
    ...
```

### 访问初始数据和实例
```python
    def __init__(self, instance=None, data=empty, **kwargs):
        self.instance = instance
        if data is not empty:
            self.initial_data = data
        self.partial = kwargs.pop('partial', False)
        self._context = kwargs.pop('context', {})
        kwargs.pop('many', None)
        super(BaseSerializer, self).__init__(**kwargs)
```
访问初始数据：将数据传递给序列化器时，为修改的数据将作为`.initial_data`。如果data关键字参数没有被传递，那么.initial_data属性将不存在。

访问实例：将初始对象或查询集传递给序列化程序实例时，该对象将变为可用`.instance`。如果没有初始对象被传递，那么.instance属性将为None。

### 处理嵌套对象
对象某些属性可能不是简单的数据类型，如字符串、日期等，可能是一个复杂的数据结构，例如：
```python
class UserSerializer(serializers.Serializer):
    email = serializers.EmailField()
    username = serializers.CharField(max_length=100)

class CommentSerializer(serializers.Serializer):
    user = UserSerializer()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```
如果嵌套表示可以选择性接受None值，则应将该required=False标志传递给嵌套序列化程序：
```python
user = UserSerializer(required=False)
```
如果嵌套表示应该是项目列表，则应将该many=True标志传递给嵌套序列化：
```python
users = UserSerializer(many=True)
```

### 处理嵌套数据
反序列化支持处理嵌套数据，如反序列化为对象并保存或更新：
```python
class UserSerializer(serializers.ModelSerializer):
    profile = ProfileSerializer()

    class Meta:
        model = User
        fields = ('username', 'email', 'profile')

    def create(self, validated_data):
        profile_data = validated_data.pop('profile')
        user = User.objects.create(**validated_data)
        Profile.objects.create(user=user, **profile_data)
        return user

    def update(self, instance, validated_data):
        profile_data = validated_data.pop('profile')
        profile = instance.profile

        instance.username = validated_data.get('username', instance.username)
        instance.email = validated_data.get('email', instance.email)
        instance.save()

        profile.is_premium_member = profile_data.get(
            'is_premium_member',
            profile.is_premium_member
        )
        profile.has_support_contract = profile_data.get(
            'has_support_contract',
            profile.has_support_contract
         )
        profile.save()

        return instance
```
更好的方法是在模型管理器中编写这些方法，然后在序列化器中调用这些方法即可：
```python
class UserManager(models.Manager):
    ...

    def create(self, username, email, is_premium_member=False, has_support_contract=False):
        user = User(username=username, email=email)
        user.save()
        profile = Profile(
            user=user,
            is_premium_member=is_premium_member,
            has_support_contract=has_support_contract
        )
        profile.save()
        return user
```

## ModelSerializer
ModelSerializer提高了一个快捷方式用于自动创建一个与模型字段对应的序列化器，它会：
1. 根据模型自动生成相应字段
2. 自动生成相关的验证器，如unique_together验证器
3. 默认简单实现create()和update()


### 检查各个字段
输入命令：python manage.py shell，打开Django shell：
```python
>>> from myapp.serializers import AccountSerializer
>>> serializer = AccountSerializer()
>>> print(repr(serializer))
AccountSerializer():
    id = IntegerField(label='ID', read_only=True)
    name = CharField(allow_blank=True, max_length=100, required=False)
    owner = PrimaryKeyRelatedField(queryset=User.objects.all())
```

### 指定字段
在Meta类中，可以field指定需要序列化的字段，也可以用exclude去除无需序列化的字段：
``` python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ('id', 'account_name', 'users', 'created')
        # exclude = ('users',) 去除‘user’字段
```
field = '\__all\__'表示使用模型中所有字段。

### 嵌套字段
如果一个model有外键的时候，默认显示的是外键的id，此时要显示外键的所有值，可以设置depth，会把外键的所有值显示出来
```python
class AccountSerializer(serializers.ModelSerializer):
    class Meta:
        model = Account
        fields = ('id', 'account_name', 'users', 'created')
        depth = 1
```
该depth选项应设置为一个整数值，指示在还原为平面表示之前应该遍历的关系的深度。

### 添加或重写字段
可以在继承ModelSerializer的类中声明字段来添加额外字段，或者重写默认字段：
```python
class AccountSerializer(serializers.ModelSerializer):
    account_id = serializers.CharField(source='id', read_only=True)
    groups = serializers.PrimaryKeyRelatedField(many=True)

    class Meta:
        model = Account
```
source关键字可以指定覆盖哪一个模型字段。

### 只读字段
1. 可以在定义字段时加上read\_only=True
2. 元选项read\_only\_fields = ('account_name', )
3. 设置了editable=False和AutoField字段默认只读

### 额外关键字参数
可以使用extra_kwargs元选项在字段上指定任意附加关键字参数，这样可以不用在序列化器上显示声明这个字段：
```python
class CreateUserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ('email', 'username', 'password')
        extra_kwargs = {'password': {'write_only': True}}
```

## 序列化器字段
### 常用参数
#### read_only
- API输出中包含只读字段，但在创建或更新操作期间不应将其包含在输入中。序列化程序输入中包含的任何“read_only”字段都将被忽略
- 默认False

#### write_only
- 将其设置True为确保在更新或创建实例时可以使用该字段，但在序列化表示时不包括该字段
- 默认False

#### required
- 如果在反序列化过程中没有提供字段，通常会出现错误。如果在反序列化过程中不需要此字段，则设置为false
- 默认True

#### allow_null
- 通常情况下，如果None传递到序列化程序字段会引发错误。将此关键字参数设置为True，None会被视为有效值。
- 默认False

#### default
- 设置了默认值，如果没有提供输入值，该值将用于字段。如果没有设置，默认行为是根本不填充属性。

#### source
将用于填充字段的属性的名称。可能只是一个只带self参数的方法URLField(source='get\_absolute\_url')，或者可能使用`.`符号来遍历属性，比如EmailField(source='user.email')

#### validators
验证函数列表会被应用到输入字段输入，并且提出一个验证错误或者简单地返回

### 常用字段
#### CharField
- 文本表示。可以选择验证文本是短于max\_length还是长于min\_length
- 对应于django.db.models.fields.CharField或django.db.models.fields.TextField

#### EmailField
- 文本表示，将文本验证为有效的电子邮件地址
- 对应于 django.db.models.fields.EmailField

#### IntegerField
- 整数表示
- 对应于django.db.models.fields.IntegerField，django.db.models.fields.SmallIntegerField，django.db.models.fields.PositiveIntegerField和django.db.models.fields.PositiveSmallIntegerField

#### FloatField
- 浮点表示
- 对应于django.db.models.fields.FloatField

#### DateTimeField
- 日期和时间表示
- 对应于django.db.models.fields.DateTimeField

#### 自定义字段
如果你想创建一个自定义字段，你需要子类Field，然后重写.to\_representation()和.to\_internal\_value()方法之一或两者。这两个方法用于在初始数据类型和原始序列化数据类型之间进行转换：
- to\_representation()：调用该方法将初始数据类型转换为基本的可序列化数据类型
- to\_internal\_value()：调用该方法将原始数据类型恢复为其内部的python表示形式











--------------
> [Home - Django REST framework](http://www.django-rest-framework.org/)

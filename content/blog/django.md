---
title: "Django"
date: 2018-06-28T20:41:47+09:00
draft: false
tags: ["django"]
categories: ["django"]
series: []
---

Djangoの重要コマンド、設計パターンのまとめ。


# Django クイックリファレンス

## コマンドライン操作
#### バージョン確認
```
$ python -m django --version
```

#### プロジェクト作成
```
$ django-admin startproject mysite
$ cd mysite
```

#### 開発用サーバー起動
```
$ python manage.py runserver
$ python manage.py runserver 8080  # ポート指定
```

#### アプリケーションを作る（Scaffoldのイメージ）
```
$ python manage.py startapp polls
```

#### マイグレーション
マイグレーションを実行する。
```
$ python manage.py migrate
$ python manage.py migrate --run-syncdb
```

#### [モデルを有効にする](https://docs.djangoproject.com/ja/2.0/intro/tutorial02/#activating-models)
マイグレーションのSQLコマンドを生成する
```
$ python manage.py makemigrations polls
```

```
$ python manage.py sqlmigrate polls 0001
```

#### 対話シェル
```
$ python manage.py shell
```


## Model

### モデル設定・設計
```
ForeignField
m.CASCADE
```

[Relationships](https://docs.djangoproject.com/en/2.0/topics/db/models/#relationships)
[Related object reference](https://docs.djangoproject.com/en/2.0/ref/models/relations/)
[Django: モデルフィールドリファレンスの一覧](https://qiita.com/nachashin/items/f768f0d437e0042dd4b3) <- 不完全
[モデルフィールドリファレンス](https://docs.djangoproject.com/ja/2.0/ref/models/fields/)

##### `on_delete`
[Django、`on_delete`を使う(django2.0から必須)](https://torina.top/detail/297/)

```
models.CASCADE
models.PROTECT
models.SET_NULL
models.SET_DEFAULT
models.SET()
models.DO_NOTHING
```

##### `AutoField`
idに使われる。数字で、自動で1ずつ増える（？）。（参照：https://stackoverflow.com/a/6062240/8776028)

##### `unique`
ユニークさを保証するが、shellで試したとき重複するとエラーになってしまうため、使いにくいかも。
idを変更する目的ならいじる必要ない。

#### `*`IDを文字列にする
```python
class FooModel(m.Model):
    id = m.CharField(max_length=100, unique=True, primary_key=True)
```
このようにidを書き換えればいい。

#### Overwrite save()

```python
def save(self, *args, **kwargs):
        # Before save
        # ...
        super(MyModel, self).save(*args, **kwargs)
        # After save
        # ...
```



### Relationship
- [2.1/ref/models/relations/](https://docs.djangoproject.com/en/2.1/ref/models/relations/)
- [2.1/topics/db/examples/](https://docs.djangoproject.com/en/2.1/topics/db/examples/)

#### [多対多関係(Many-to-many)](https://docs.djangoproject.com/ja/2.0/topics/db/examples/many_to_many/)
m.ManyToManyField(RelatedModel)

ManyToManyは.save()してから登録することに注意!

ManyToMany.throughを明示指定すれば任意のモデルをManyToManyのRelationとして使用できる（？）

##### [Extra fields on many-to-many relationships](https://docs.djangoproject.com/en/2.2/topics/db/models/#extra-fields-on-many-to-many-relationships)

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through='Membership')

    def __str__(self):
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```



through Modelの要素で`order_by`するベストプラクティス何だろう
QuerySetの使い方？
 [Metaで記述する例](https://stackoverflow.com/questions/3893955/django-manytomanyfield-ordering-using-through)

```python
class ProfileAccount(models.Model):
	pass
	class Meta:
		ordering = ('number',)
```



[複数属性の組でPrimary Keyとする](https://docs.djangoproject.com/en/2.2/ref/models/options/#unique-together)

現状の方法では`unique_together`を用いる。これは将来depricatedとなる可能性があるため、他の方法があれば随時更新する。

```python
class Hop(models.Model):
    migration = models.ForeignKey('Migration')
    host = models.ForeignKey(User, related_name='host_set')

    class Meta:
        unique_together = (("migration", "host"),)
```



多対一](https://docs.djangoproject.com/ja/2.0/topics/db/examples/many_to_one/)

#### [一対一(one-to-one)](https://docs.djangoproject.com/en/2.0/topics/db/examples/one_to_one/)
```python
m.OneToOneField()
```
参照される方はsave済みである必要がある。

#### その他Model.pyを書くときのTips
##### 定義前のモデルを参照
クラス名を文字列で書けば良い


### モデル操作
#### モデルオブジェクトを作成するときの作法
[Creating objects](https://docs.djangoproject.com/en/dev/topics/db/queries/#creating-objects)

```python
f = FooModel(name='fooo')
f.save()
```

```python
Player # <- Model
Player.objects.get(id=1)
Player.objects.filter(shard='ea')
```

#### クエリ
.filter()の中で使う、多分。

lte (less than or equal)など
[](https://docs.djangoproject.com/en/2.0/ref/contrib/gis/geoquerysets/)
`lte`
`gte`


[cf.](https://stackoverflow.com/a/20012419/8776028)
> Consider using the .exists method, for it issues a faster query to your database than if you try to retrieve all the user information with the .get method. And the code gets a little cleaner too

getよりfilter().exists()がいいらしい。


[Using Django querysets effectively](http://blog.etianen.com/blog/2013/06/08/django-querysets/)
all()やiterator()についても記述がある。

#### ループの前にRelatedをあらかじめ取ってくる
[`select_related()`, `prefetch_related("creative_set"):`](https://qiita.com/shunsukeaihara/items/eaaace97f6db75355f95)

### 統計分析
#### Aggregation
[](https://docs.djangoproject.com/ja/2.0/topics/db/aggregation/)

#### Django-Pandas
[](https://github.com/chrisdev/django-pandas)


## View

### ディレクトリ設計


#### 簡易
```views.py
def index(request, ):
    return HttpResponse("Hello world")
```

```urls.py
urlpatterns = [
    path('', views.index, name='index'),
]
```



##### Method view ?で部分的にアップデートする方法

https://stackoverflow.com/a/4674127/8776028

```python
def my_view(request, id): 
    instance = get_object_or_404(MyModel, id=id)
    form = MyForm(request.POST or None, instance=instance)
    if form.is_valid():
        form.save()
        return redirect('next_view')
    return render(request, 'my_template.html', {'form': form}) 
```





#### Class View

ListView, DetailView
[Generic detail views](https://docs.djangoproject.com/ja/2.0/ref/class-based-views/generic-display/)

CreateView, DeleteView, EditView, FormView
[クラスベースのビューでフォームを扱う](https://docs.djangoproject.com/ja/2.0/topics/class-based-views/generic-editing/)



`extra_context`は簡易に定義できるものの，urlパラメータを利用したオブジェクトを渡そうとするとselfが必要になる．そこで次のリンクを利用する．

https://docs.djangoproject.com/en/2.1/topics/class-based-views/generic-display/#adding-extra-context


## Template

### HTML
[](https://docs.djangoproject.com/ja/2.0/ref/templates/builtins/)
[](https://docs.djangoproject.com/ja/2.0/ref/templates/language/#template-inheritance)


## Form
Formクラスを使う。（使わなくてもいいかもしれないが）
タグの編集がめんどくさい。
```python
q = forms.CharField(label='search', 
                    widget=forms.TextInput(attrs={'placeholder': 'Search'}))
```
[cf.](https://stackoverflow.com/questions/4101258/how-do-i-add-a-placeholder-on-a-charfield-in-django)
[公式](https://docs.djangoproject.com/ja/2.0/ref/forms/widgets/)
[one more](https://docs.djangoproject.com/ja/2.0/topics/forms/#widgets<Paste>)


値を取り出して使う方法
仮に使わなかった場合、どうやって値を取り出すのか不明。
```python
if form.is_valid():
      subject = form.cleaned_data['subject']
```
[cf.](https://docs.djangoproject.com/ja/2.0/topics/forms/#field-data)

フォームのPOST先は自動で元のViewだが、actionで指定することもできるらしい？
[cf.](https://stackoverflow.com/questions/5467192/django-what-goes-into-the-form-action-parameter-when-view-requires-a-parameter)

Submitボタンはないので普通に書いていいっぽい。
[cf.](https://stackoverflow.com/questions/2080332/django-form-submit-button)


## URL

DetailsViewなどを使いたい時、URLではpkまたはslugしか受け付けてもらえない。

cf. [What is a slug in dnango](https://stackoverflow.com/questions/427102/what-is-a-slug-in-django)
モデルにslugを加える必要がありそう。


## デバッグ

データベースをデバッグするときの一連の操作
```
mv db.sqlite3 db.sqlite3.bak
python manage.py migrate --run-syncdb
python manage.py shell
```

```
from vainlab.models import Player
from vainlab.vain_api import VainAPI
v = VainAPI()
v.player_matches('ea', 'Qiuqiu')
```

SQL文を確認する！
[reference](https://www.laurencegellert.com/2016/09/django-enable-sql-debug-logging-in-shell-how-to/)
```
import logging
log = logging.getLogger('django.db.backends')
log.setLevel(logging.DEBUG)
log.addHandler(logging.StreamHandler())
```


## デプロイ
### WSGI
[](https://docs.djangoproject.com/ja/2.0/howto/deployment/wsgi/)
よくわからん。

### uWSGI
[uwsgi](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/uwsgi/)
これだっ。


### static
```terminal
$ python manage.py collectstatic
```

# dhireクトリ構成

```terminal
$ tree
.
├── db.sqlite3
├── gitshell
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   ├── settings.cpython-36.pyc
│   │   ├── urls.cpython-36.pyc
│   │   └── wsgi.cpython-36.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── listter
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
└── manage.py
```


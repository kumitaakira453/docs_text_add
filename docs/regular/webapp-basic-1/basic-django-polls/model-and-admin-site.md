---
layout: default
title: "モデル・管理者サイト(追記事項)"
---

## さらに便利なデータベースの操作

[教科書](https://be-engineer.tech/docs/regular/webapp-basic-1/basic-django-polls/model-and-admin-site.html#:~:text=%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B%E3%80%82-,%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%82%92%E6%93%8D%E4%BD%9C%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B,-%EF%83%81)で扱った対話モードでのデータベースの操作は、作成したデータベースの挙動をテストする際に非常に有用な方法です。特に今後複雑なデータベースを構築するようになると、データベースにアクセスするコードを書いていても、思い通りには動かないケースを多く出てくると思います。そういった時に、自分が考えたデータベースへのアクセスをテストしながら書いていくことは、開発していく上で大事なスキルになります。
そういった時に用いられるのが`django_shell`なのですが、使ってみてわかるように作成した DB を毎回 import する必要があり、さらに django で開発している時は当たり前に使っている、(vscode のエディターの)入力補完機能もついていません。
今回紹介するのは、そういった面倒なことを自動でやってくれる django のパッケージです。
`django-extension`の[`shell_plus`](https://django-extensions.readthedocs.io/en/latest/shell_plus.html)と呼ばれる機能を使います。djnago にもともと入っているものではないので、追加で install が必要ですが、一度入れてしまったら後々非常に便利なので、ぜひ入れておいてください。
導入方法を以下に示します。

### `shell_plus`の導入方法

1. ターミナル上で以下を実行し`django-extensions`をインストール
   ```bash
    $ pip install django-extensions
   ```
2. `mysite/settings.py`の`INSTALLED_APPS`を編集
   ```python
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django_extensions', ←ここに追記
    'polls',
    'forum',
   ]
   ```
3. 最後にターミナル上で shell を起動します
   ```bash
   $ python manage.py shell_plus
   ```

## 管理者画面のカスタマイズ

Django には自動で管理者画面が作成されるという非常に便利な機能がついていますが、さらにこの管理者画面は自分好みにカスタマイズすることができます。特にアプリを個人で作っているとあまり気にしないかもしれませんが、実際に業務として開発を行うようになるとアプリを使う管理者側の立場になると、特定のよく使うデータを見やすく配置することが必要になります。そういった時に向けて、いくつか紹介しておきます。

### データベースの表示名を日本語にする

現状では管理者画面で「Question」や「Choice」などの`models.py`で定義した名前になっていると思います。（管理者画面の DB 一覧表示ではデフォルトで複数形になっています)
ただ実際にサービスを利用する管理者にとっては日本語で統一しておいたほうがわかりやすいでしょう。そこで`models.py`の設定を変更して日本語表示にして見ましょう。
ついでに field も日本語表示するようにコードを変更しています。

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField("質問内容", max_length=200)
    pub_date = models.DateTimeField("公開日")

    class Meta:
        verbose_name = "質問"
        verbose_name_plural = "質問一覧"

    def __str__(self):
        return self.question_text


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField("選択肢", max_length=200)
    votes = models.IntegerField("投票数", default=0)

    class Meta:
        verbose_name = "選択肢"
        verbose_name_plural = "選択肢一覧"

    def __str__(self):
        return self.choice_text
```

#### コードの簡単な解説

1. DB クラス自体の表示名を変更する
   `class Meta`というクラスを作りその中に`verbose_name`,`verbose_name_plural`にそれぞれ値を設定します。それぞれ管理者画面で表示するときの単数形、複数形を指定することができます
1. DB クラス内のそれぞれの field の表示名を変更する
   それぞれの field を定義したコードの第一引数に表示名を追加することで変更できます。

#### 変更の反映

基本的には DB 自体の構造を変えたわけではないので、変更を保存し管理者画面をリロードすることで変化を確認できます。

### それぞれの DB の一覧表示で各 field も見れるようにする

Question や Choice の一覧を表示するページには、`__str__`で登録した形式で DB についての情報が書かれていますが、実際にそれぞれの field の情報を確認しようとすると、それぞれを選択して別の画面を開く必要があります。特定の field の値を全て確認したいというときに不便です。そこで一覧画面でも全ての field が閲覧できるように編集してみましょう。`admin.py`を以下のように変更してください。

```python
from django.contrib import admin
from .models import Question, Choice


#教科書に沿って書いた以下の二行はコメントアウトする
#admin.site.register(Question)
#admin.site.register(Choice)



@admin.register(Question)
class QuestionAdmin(admin.ModelAdmin):
    list_display = ('question_text', 'pub_date')


@admin.register(Choice)
class ChoiceAdmin(admin.ModelAdmin):
    list_display = ('question', 'choice_text', 'votes')
```

#### コードの簡単な説明

Django が提供してくれている管理者画面をそのまま使う場合は、教科書のように一行書くだけでいいのですが、設定の一部を変更する際には以下のようにそれぞれをクラスとして書く直す必要があります。それぞれのクラスに対して`list_display`という`タプル`を宣言し、一覧表示で見れるようにしたい field 名をその中に追加することで、一覧表示の設定を変更することができます。

### Question を作った時に Choice も一緒に作成できるようにする

現在の使用ではそれぞれの DB クラスごとに管理者画面の別々のページで作成する必要がありますが、Question を作ってから、それに紐ずく Choice を毎回別のページを開いて作成するのは面倒です。以下のよう`admin.py`を変更することで Question を作成する画面に Choice を作成する部分を追加することができます。

```python
from django.contrib import admin
from .models import Question, Choice

#教科書に沿って書いた以下の二行はコメントアウトする
#admin.site.register(Question)
#admin.site.register(Choice)

#他の DB 作成画面で表示する用の Choice 作成部分(インラインと呼ぶ)を作る
class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 1 # 空の追加フィールドを 1 つ表示

@admin.register(Question)
class QuestionAdmin(admin.ModelAdmin):
    list_display = ('question_text', 'pub_date') # インライン(一緒に表示する部分)として登録
    inlines = [ChoiceInline]

@admin.register(Choice)
    class ChoiceAdmin(admin.ModelAdmin):
    list_display = ('question', 'choice_text', 'votes') # 一覧で表示するフィールド

```

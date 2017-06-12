# Making Queries

>  2017.06.07 박현준

- 데이터 모델을 만들면 Django는 자동으로 객체를 생성, 검색, 업데이트 및 삭제할 수있는 데이터베이스 추상화 API를 제공한다

###Creating objects
- Django는 Python 객체에서 데이터베이스 테이블 데이터를 나타내기 위해 직관적인 시스템을 사용한다
- 모델 클래스는 데이터베이스 테이블을 나타내며 클래스의 인스턴스는 데이터베이스 테이블의 특정 레코드를 나타낸다

###Saving changes to objects
- 이미 데이터베이스에있는 객체의 변경 사항을 저장하려면 save()를 사용한다
- 이것은 뒤에서 UPDATE SQL 문을 수행합니다. Django는 명시 적으로 save()를 호출 할 때까지 데이터베이스에 접근하지 않는다

###Retrieving objects
- 데이터베이스에서 객체를 검색하려면 모델 클래스의 관리자를 통해 QuerySet을 생성한다
- 관리자는 모델에 대한 QuerySets의 주요 소스이다. Blog.objects.all()은 데이터베이스의 모든 Blog 객체를 포함하는 QuerySet을 반환한다

```
>>> Blog.objects
<django.db.models.manager.Manager object at ...>
>>> b = Blog(name='Foo', tagline='Bar')
>>> b.objects
Traceback:
    ...
AttributeError: "Manager isn't accessible via Blog instances."
```

###Saving ForeignKey and ManyToManyField fields
- ForeignKey 필드를 업데이트하는 것은 객체 업데이트 방법과 같다. save()
- ManyToMany 필드에서는 add()를 사용해야 하며, 중간 테이블에 레코드를 하나 생성한다

###Retrieving specific objects with filters
- filter(**kwargs) : 조건에 맞는 객체를 QuerySet으로 반환한다.
- exclude(**kwargs) : 조건에 부합하는 객체를 제외한 나머지 객체로 QuerySet 을 반환한다.
- 예를 들어, 2006 년부터 블로그 항목의 QuerySet을 얻으려면 filter ()를 다음과 같이 사용한다

```
Entry.objects.filter(pub_date__year=2006)
Entry.objects.all().filter(pub_date__year=2006)
```

###Retrieving a single object with get()

```
>>> one_entry = Entry.objects.get(pk=1)
```

###Limiting QuerySets
- Python의 배열 슬라이스 구문의 서브 세트를 사용하여 QuerySet을 특정 수의 결과로 제한해야한다 
- 이것은 SQL의 LIMIT 및 OFFSET 절과 동일하다

```
>>> Entry.objects.all()[:5]
>>> Entry.objects.all()[5:10]
>>> Entry.objects.all()[:10:2]
>>> Entry.objects.order_by('headline')[0]
>>> Entry.objects.order_by('headline')[0:1].get()
```
###The pk lookup shortcut
- 모델에서 기본 키는 id 필드이므로이 세 문은 동일합니다.

```
>>> Blog.objects.get(id__exact=14) # Explicit form
>>> Blog.objects.get(id=14) # __exact is implied
>>> Blog.objects.get(pk=14) # pk implies id__exact
```
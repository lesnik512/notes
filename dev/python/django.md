# ORM

Correspondence of some types to SQL-types:

- CharField -> VARCHAR(N)
- EmailField -> VARCHAR(N)
- TextField -> LONGTEXT
- BooleanField -> TINYINT(1)
- DateTimeField -> DATETIME
- DateField -> DATE
- TimeField -> TIME
- ItegerField - INT(11)
- PostitiveIntegerField -> INT(11)

## Examples of SQL and Django ORM queries

|SQL|Python|
|-|-|
|SELECT * FROM articles_article WHERE title='some title';|Article.objects.filter(title='some title')|
|SELECT * FROM articles_article WHERE NOT title='some title';|Article.objects.exclude(title='some title')|
|SELECT * FROM articles_article WHERE title LIKE '%non%';|Article.objects.filter(title__contains='non')|
|SELECT * FROM articles_article AS aa INNER JOIN articles_category AS ac ON aa.category_id = ac.id
WHERE ac.name = 'category name';|Article.objects.filter(category__name='category name')|
|SELECT * FROM articles_article ORDER BY created_at;|Article.objects.order_by('created_at')|
|SELECT * FROM articles_article ORDER BY created_at;|Article.objects.order_by('-created_at')|

## Field lookups
- field=value - field is equal to value
- field__contains=value - field contains value
- relation__field=value - value of a field "field" in related table contains value
- field__gt, field__lt, field__gte, field__lte - more, less, etc.
- field__in=[v1, v2, v3] - value is in some list

# Code Snippets

## Drop duplicate rows from database by Django ORM

```python
def drop_duplicates(queryset, unique_fields):
    duplicates = queryset.values(*unique_fields).annotate(
        max_id=models.Max('id'),
        count_id=models.Count('id'),
    ).order_by().filter(count_id__gt=1)

    for duplicate in duplicates:
        queryset.filter(**{x: duplicate[x] for x in unique_fields}).exclude(id=duplicate['max_id']).delete()
```

Usage example:

```python
drop_duplicates(
    SberbankCard.objects.all(),
    ('client_id', 'pan'),
)
```

Source: [StackOverflow](https://stackoverflow.com/a/13700642)

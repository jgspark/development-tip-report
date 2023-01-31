


* GCP 에서 GQL을 가지고 DataStore 에서 페이징 처리 및 Spring Java 에서 DataStore 페이징 처리 

### 구현 

* GQL 로 페이징 처리 하는 방법 

```
select *
from Table
where __key__ > Key(Table, 89010)
limit 10
order by __key__ asc
```


* application level 에서 구현 방법


```java
 public CursorQueryResult<Table> findAllByUserId(CursorPageable pageable, Long userId) {

        Query<Table> query = _ofy().load().type(Table.class);

        if (userId != 0) {
            query = query.filter("__key__ >", Key.create(Table.class, userId));
        }

        query = query
            .limit(pageable.getPageSize())
            .order("__key__");

        if (pageable.getCursor() != null) {
            query = query.startAt(Cursor.fromWebSafeString(pageable.getCursor()));
        }

        QueryResultIterator<Table> iterator = query.iterator();

        List<Table> inactiveUserAccounts = slice(iterator, pageable.getPageSize());

        return new CursorQueryResult<>(inactiveUserAccounts, iterator.getCursor(), iterator.hasNext());
    }
```

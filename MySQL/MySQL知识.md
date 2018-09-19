# MySQL知识

## 常用SQL语句

t - table, c - column, v - value, o - operation( = , < , > , <=, >=)

### select

```sql
select * from t;
select c from t;
select distinct c1 from t;
select c1, c2 from t where c1 = v;
select * from t where c1 = v1 and c2 < v2;
select * from t where c1 >= V1 or c2 <> v2;
select * from t where c1 = v1 and (c2 = v2 or c3 = v3);
select * from t order by c; -- (默认升序 ASC， 升序为DESC)
select * from t order by c desc;
select * from t order by c1, c2; -- (在c1的排序基础上，再次对c2排序)
select * from t limit num; -- (制定取出多少条数据)
select * from t limit 9,4; -- （表示从9行之后开始取，取4条数据）
```

### where中的运算符

| 运算符  | 描述                                         |
| ------- | -------------------------------------------- |
| =       | 等于                                         |
| <>      | 不等于。或（！=）                            |
| >       | 大于                                         |
| <       | 小于                                         |
| \>=的   | 大于等于                                     |
| \<=     | 小于等于                                     |
| between | 在某个范围内                                 |
| like    | 模糊查询,通配符 （%like, %like%, like%,...） |
| in      | 指定针对某个列的多个可能值                   |

### like 通配符

| 通配符                   | 描述                       |
| ------------------------ | -------------------------- |
| %                        | 替代0个或多个字符          |
| _                        | 替代一个字符               |
| [charlist]               | 字符列中的任何单一字符     |
| [^charlist]或[!charlist] | 不在字符列中的任何单一字符 |

```mysql
like 'Tc%' -- 将搜索以字母Tc开头的所有字符串（如 Tccisa）
like '%er' -- 将搜索以字符er结尾的所有字符串（如 teacher）
like '%know%' -- 将搜索在任何位置包含know的所有字符串（如 unknowledgeable）
like '_ove' -- 将搜索以字母ove结尾的所有四个字母的名称（如 love）
like '[CK]ars[eo]n' -- 将搜索下列字符串： Carsen、Karson、Carson和Karson
like '[A-Z]user' -- 将搜索以字符串user结尾、以从A-Z的任何单个字母开头的所有名称
like 'M[^c]%' -- 将搜索以字母M开头，并且第二个字母不是c的所有名称（如MiPro）
```

### insert into 插入

```sql
insert into t values (v1, v2, v3, v4, ...);
insert into t (c1, c2, c3, ...) values (v1, v2, v3, ...);
```

### update 更新数据

```sql
update t set c1=v1, c2=v2, ... where c = v;
```

### delete 删除数据

```sql
delete from t where c=v; --(注意，如果省略了where子句，所以的记录都会被删除！)
```

> 关于删除的三个语句： `DROP`, `TRUNCATE`, `DELETE`的区别
>
> **DROP**
>
> ```sql
> drop t;
> ```
>
> 删除表，并释放空间，将test删除的一干二净。
>
> **TRUNCATE**
>
> ```mysql
> truncate t;
> ```
>
> 删除表中的内容，并释放空间，但不删除表的定义，表的结构还在。
>
> **DELETE**
>
> ```mysql
> delete from t;
> ```
>
> 仅删除表test内的所有内容，保留表的定义，不释放空间。


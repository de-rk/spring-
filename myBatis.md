动态sql 里面 foreach 属性<code>conllection</code>对应的是集合

<code>list</code> <code>array</code> 

不过map类型的有一点点区别
对应的是map值里面的key 

```java
map1.put ("map1" ,new int[]{7788,7369});
```

<code>分页查询,不要再搞忘了</code>
```sql
-- 从大于(2-1)*3开始 ,到 <=(2*3)结束,2可以替换为第几页
select * from(select a.*,ROWNUM rn from(select * from emp) a where ROWNUM<=(2*3)) where rn>(2-1)*3
```

```java
// myBatis中转义<号
<![CDATA[<=]]>
```

## Mybatis中分页两种方法
* 1.普同sql
* 2.RowBounds插件分页

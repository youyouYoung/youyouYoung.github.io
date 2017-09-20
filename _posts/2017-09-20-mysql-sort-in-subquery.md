---
layout: post
title: MySQL的子查询中排序 
date:  2017-09-20 17:22:35
categories: MySQL
expert: 一次使用group by时需要对组内数据进行排序, 网上的方法一直无法实现, 最后终于找到原因, 在这里记录一下
---

* content
{:toc}

## 起因

---

有一张表保存着用户对于文件的阅读记录, 包括文件名, 读者, 阅读时间, 时长, 备注评论.

```
create table reading_record (
	id int primary key auto_increment,	-- 自增主键
	file varchar(255),	-- 阅读文件名
	user varchar(255),	-- 读者
	expend int,		-- 阅读时长
	time datetime,		-- 阅读时间
	note text		-- 备注评论
)
```

有个要求是:**想要统计每个人对于每个文件的阅读总时长, 阅读次数, 最后阅读时间, 最后的阅读评论**

一开始我写了下面的sql去实现这个功能:

```
select sum(expend) as 阅读总时长, count(id) as 阅读次数, time as 最后阅读时间, note as 最后的阅读评论 from reading_record group by user, file order by id desc;
```

得到的结果里面阅读总时长和阅读次数时符合要求的, 但是**最后阅读时间和最后的阅读评论出现了多条错误的信息**

为了证明这个sql写的有问题, 我使用下面的这个sql去进行校验.

```
select max(id) as 实际的每组最后一次记录id, id as 以为的每组最后一次记录id, sum(expend) as 阅读总时长, count(id) as 阅读次数, time as 最后阅读时间, note as 最后的阅读评论 from reading_record group by user, file order by id desc;
```

查询结果中出现多条`实际的每组最后一次记录id`!=`以为的每组最后一次记录id`.

---

## 解决方法

---

### 使用组内排序

网上搜索之后发现, 排序的`order by`是在分组`group by`之后. 所以首先要做组内排序, 然后再进行分组才能得到正确的结果.

基于上面的方法, 写了下面的sql.

```
select sum(expend) as 阅读总时长, count(id) as 阅读次数, time as 最后阅读时间, note as 最后的阅读评论 from (select * from reading_record order by id desc) as temptable group by user, file
```

**结果发现:子排序中不管是以desc方式排序还是以asc方式排序, 结果都是一样的**

为了排除错误, 我在查询结果字段前面都加上了temptable. 如`max(temptable.id)`, 发现并没有什么作用.

### mysql以及mariadb的特性

查了很长时间之后, 终于在一个年限比较新的文章里看到了: **新版本的MySQL或者mariadb会自动忽略subquery的排序**

具体的MySQL版本我记不清了.

为了验证这个说法, 我使用了以下的sql:

```
select * from (select * from reading_record order by id desc) as temptable;
```

经过验证发现结果仍然是id正序输出的记录, 所以我这个版本的MySQL会忽略subquery的排序.

### 解决subquery忽略排序

**为了实现subquery可以排序, 添加limit限制.**

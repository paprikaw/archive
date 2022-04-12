# Postgre从源码编译过程

Postgresql的服务器集中安装在一个目录底下

具体安装编译步骤可以参考[这里](../database/webResources/COMP9315%2022T1%20-%20Prac%20Exercise%2001.html)

# Catalog



# index

使用索引可以更加高效的找到对应的row

注意：创建index的时候可能会block写操作

```
By default, PostgreSQL allows reads (SELECT statements) to occur on the table in parallel with index creation, but writes (INSERT, UPDATE, DELETE) are blocked until the index build is finished. In production environments this is often unacceptable. 

PostgreSQL supports building indexes without locking out writes. This method is invoked by specifying the CONCURRENTLY option of CREATE INDEX. When this option is used, PostgreSQL must perform two scans of the table, and in addition it must wait for all existing transactions that could potentially modify or use the index to terminate. Thus this method requires more total work than a standard index build and takes significantly longer to complete. However, since it allows normal operations to continue while the index is built, this method is useful for adding new indexes in a production environment. Of course, the extra CPU and I/O load imposed by the index creation might slow other operations.

```


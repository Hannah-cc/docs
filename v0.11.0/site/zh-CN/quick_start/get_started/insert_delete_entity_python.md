---
id: insert_delete_entity_python.md
---


# 插入、删除向量

你可以在集合或集合的分区中进行 Entity 操作，本页提供以下内容：

- [在集合中插入 Entity](#insert-entity-to-collection)
- [在分区中插入 Entity](#insert-entity-to-partition)
- [通过 ID 删除 Entity](#delete-entity)


## 在集合中插入向量
<a name="insert-entity-to-collection"></a>

1. 随机生成 10000 个 Entity：

   ```python
   >>> import random
   # Generate 10000 entities.
   >>> list_of_int = [random.randint(0, 255) for _ in range(10000)]
   >>> vectors = [[random.random() for _ in range(128)] for _ in range(10000)]
   ```

2. 插入向量列表。在创建集合时指定 `auto_id` 为 True, Milvus 自动为 Entity 分配 ID。

   ```python
   # Insert vectors.
   >>> hybrid_entities = [
           {"name": "A", "values": list_of_int, "type": DataType.INT32},
           {"name": "B", "values": list_of_int, "type": DataType.INT32},
           {"name": "C", "values": list_of_int, "type": DataType.INT64},
           {"name": "Vec", "values": vectors, "type":DataType.FLOAT_VECTOR}
       ]
   >>> client.insert('test01', hybrid_entities)
   ```

    如果在创建集合时指定`auto_id` 为 False, 你也可以自己定义向量 ID：

   ```python
   >>> vector_ids = [id for id in range(10000)]
   >>> client.insert('test01', hybrid_entities, ids=vector_ids)
   ```

## 在分区中插入向量
<a name="insert-entity-to-partition"></a>

```python
>>> client.insert('test01', hybrid_entities, partition_tag="tag01")
```

## 通过 ID 删除向量
<a name="delete-entity"></a>

假设你的集合中存在以下向量 ID：

```python
>>> ids = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

你可以通过以下命令删除向量：

```python
>>> client.delete_entity_by_id('test01', ids)
```
<div class="alert note">
在调用 <code>delete</code> 接口后，用户可以选择再调用 <code>flush</code>，保证新增的数据可见，被删除的数据不会再被搜到。
</div>


## 常见问题

<details>
<summary><font color="#4fc4f9">Milvus 中自定义 ID 有没有长度限制？</font></summary>
ID 类型是非负的 64 位整型。
</details>
<details>
<summary><font color="#4fc4f9">Milvus 可以插入重复 ID 的向量吗？</font></summary>
可以，这样在 Milvus 中会存在相同 ID 的多条向量。
</details>
<details>
<summary><font color="#4fc4f9">Milvus 是否支持 “边插入边查询” ？</font></summary>
支持。
</details>
<details>
<summary><font color="#4fc4f9">Milvus 中单次插入数据有上限吗？</font></summary>
单次插入数据不能超过 256 MB。
</details>

# Quick Notes for Pyarrow


Collecting of personal notes while working with Pyarrow



## Convert a table to batches with designated size:


```python
table: pa.Table

batches = table.to_batches(max_chunksize=32000)
```







```python
pool = multiprocessing.Pool(processes=3)  
for database in ['eclinical_edc_dev_1090', 'eclinical_edc_dev_836']:  
    pool.apply_async(visit, args=(database,))  
    pool.apply_async(form, args=(database,))  
    pool.apply_async(ig, args=(database,))  
pool.join()
```
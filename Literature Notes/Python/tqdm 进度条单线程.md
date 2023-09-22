```python
pbar = tqdm(self.subject_form_ids())  
for subject_form_id in pbar:  
    if list_eql(self.by_id(subject_form_id=subject_form_id), self.by_uid(subject_form_id=subject_form_id)):  
        pass  
    else:  
        with open(self.database_info.database, 'a') as f:  
            f.write(f"subject_form_id: {subject_form_id} is different \n")  
    pbar.set_description(f"subject_form_id:{subject_form_id}")
```
| object_type  |  含义    |   object_id |
| ---- | ---- | ---|
|3 | Subject | subject.id |
| 4| Visit | subject_visit.id|
|5| Form|subject_form.id|
|6|IG|subject_ig.id|
|7| Item| subject_item.id|


[[audit 查询接口参数]]



# Visit 查询
```sql
SELECT eat.description, eat.user_name, eat.operation_dt  
FROM eclinical_subject_visit esv  
         join eclinical_audit_trail eat on eat.object_type = 4 and eat.object_id = esv.id and esv.visit_uuid = eat.uid  
         join eclinical_subject_crfversion esc on esv.subject_version_id = esc.id and esc.is_lastest = true  
WHERE esv.id = %(subject_visit_id)s  
order by eat.operation_dt desc;
```

# Form 查询
```sql
SELECT eat.description, eat.user_name, eat.operation_dt  
FROM eclinical_subject_form esf  
         join eclinical_audit_trail eat on eat.object_type = 5 and eat.object_id = esf.id  
         join eclinical_subject_crfversion esc on esf.subject_version_id = esc.id and esc.is_lastest = true  
WHERE esf.id = %(subject_form_id)s  
order by eat.operation_dt desc;
```


# IG 查询

```sql
SELECT eat.description, eat.user_name, eat.operation_dt  
FROM eclinical_subject_ig esig  
         join eclinical_audit_trail eat on eat.object_type = 6 and eat.object_id = esig.id  
         join eclinical_subject_crfversion esc on esig.subject_version_id = esc.id and esc.is_lastest = true  
WHERE esig.id = %(subject_ig_id)s  
order by eat.operation_dt desc;
```



[[eclinical_subject_visit]]
[[eclinical_subject_form]]
[[eclinical_subject_ig]]
[[eclinical_subject_item]]
[[eclinical_subject]]


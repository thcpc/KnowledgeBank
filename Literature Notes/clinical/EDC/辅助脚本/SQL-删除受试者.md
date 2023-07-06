说明：
替换 subject_ids 为你不想删除的 受试者ID
```sql
## 删除各种Subject 的各种配置和报表数据
DELETE
FROM eclinical_cycle_item_subject ecis
WHERE ecis.subject_id not in (subject_ids);

DELETE
FROM eclinical_data_entry_effectiveness_detail edeed
WHERE edeed.subject_id not in (subject_ids);
DELETE
FROM eclinical_endymion_detail eed
WHERE eed.subject_id not in (subject_ids);
DELETE
FROM eclinical_enrollment_subject_safety_detail eessd
WHERE eessd.subject_id not in (subject_ids);
DELETE
FROM eclinical_form_alert_history efah
WHERE efah.subject_id not in (subject_ids);
DELETE
FROM eclinical_form_completion_timeliness_detail efctd
WHERE efctd.subject_id not in (subject_ids);
DELETE
FROM eclinical_manual_query_effectiveness_detail emqed
WHERE emqed.subject_id not in (subject_ids);
DELETE
FROM eclinical_overdue_crf_forms_detail eocfd
WHERE eocfd.subject_id not in (subject_ids);
DELETE
FROM eclinical_re_query_rate_detail erqrd
WHERE erqrd.subject_id not in (subject_ids);
DELETE
FROM eclinical_sae_related_ig e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_sdr_subject e
WHERE e.subject_id not in (subject_ids);
# SELECT * FROM eclinical_sdr_visit e WHERE e.subject_subject_ids not in (subject_ids);
DELETE
FROM eclinical_subject_additional_id e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_subject_arm e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_subject_change_site_history e
WHERE e.subject_id not in (subject_ids);

DELETE
FROM eclinical_subject_ecoa_schedule e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_subject_ediary_schedule e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_subject_multi_select_change_log e
WHERE e.subject_id not in (subject_ids);

DELETE
FROM eclinical_subject_status e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_upgrade_log e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_visit_completion_timeliness_detail e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_ae_completion_timeliness_detail e
WHERE e.subject_id not in (subject_ids);
DELETE
FROM eclinical_audit_trail e
WHERE e.object_type = 3
  and object_id not in (subject_ids);

## 删除受试者的版本CRF数据（Visit, FROM, IG , Item ,Record Value, Record Value ChangeHistory） 和 CRF 相关的 Audit,
### 删除Audit


DELETE
FROM eclinical_audit_trail
WHERE id IN (SELECT a.id
             FROM (
                      SELECT eat.id
                      FROM eclinical_audit_trail eat
                               JOIN eclinical_subject_visit esv on esv.id = eat.object_id and eat.object_type = 4
                               JOIN eclinical_subject_crfversion esc on esv.subject_version_id = esc.id
                               JOIN eclinical_subject es on es.id = esc.subject_id
                      WHERE es.id not in (subject_ids)) as a);





DELETE
FROM eclinical_audit_trail
WHERE id IN (SELECT a.id
             FROM (SELECT eat.id
                   FROM eclinical_audit_trail eat
                            join eclinical_subject_form esf on esf.id = eat.object_id and eat.object_type = 5
                            join eclinical_subject_crfversion esc on esf.subject_version_id = esc.id
                            join eclinical_subject es on esc.subject_id = es.id
                   WHERE es.id not in (subject_ids)) as a);



DELETE
FROM eclinical_audit_trail r
WHERE r.id IN ( SELECT a.id FROM(
    SELECT eat.id
    FROM eclinical_audit_trail eat
    WHERE eat.object_type = 6
      and eat.object_id IN (SELECT esi.id
                            FROM eclinical_subject es
                                     join eclinical_subject_crfversion esc on es.id = esc.subject_id
                                     join eclinical_subject_ig esi on esc.id = esi.subject_version_id
                            WHERE es.id not in (subject_ids)))as a );

DELETE
FROM eclinical_audit_trail r
WHERE r.id IN ( SELECT a.id FROM(
    SELECT eat.id
    FROM eclinical_audit_trail eat
    WHERE eat.object_type = 7
      and eat.object_id IN (SELECT esi.id
                            FROM eclinical_subject es
                                     join eclinical_subject_crfversion esc on es.id = esc.subject_id
                                     join eclinical_subject_item esi on esc.id = esi.subject_version_id
                            WHERE es.id not in (subject_ids))) as a);

### 受试者的版本CRF数据（Visit, FROM, IG , Item ,Record Value, Record Value ChangeHistory）
#### About Record Value
DELETE
FROM eclinical_subject_record r
WHERE r.id IN ( SELECT a.id FROM (
    SELECT esr.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
             join eclinical_subject_record esr on esi.id = esr.subject_item_id
    WHERE es.id not in (subject_ids)) as a);


DELETE
FROM eclinical_subject_record_sync r
WHERE r.id in (SELECT a.id FROM (
    SELECT esrs.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
             join eclinical_subject_record_sync esrs on esi.id = esrs.subject_item_id
    WHERE es.id not in (subject_ids)) as a);


DELETE
FROM eclinical_subject_record_history r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esrh.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
             join eclinical_subject_record_history esrh on esi.id = esrh.subject_item_id
    WHERE es.id not in (subject_ids)) as a);

#### About Rule

DELETE
FROM eclinical_subject_item_rule r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esir.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
             join eclinical_subject_item_rule esir on esi.id = esir.subject_item_id
    WHERE es.id not in (subject_ids)) as a);

#### About Item

DELETE FROM eclinical_subject_item_status esis
WHERE esis.subject_item_id IN(SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE FROM eclinical_system_query_reply esqr WHERE esqr.system_query_id IN (
    SELECT esq.id FROM eclinical_system_query esq WHERE esq.subject_item_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids))
);



DELETE FROM eclinical_system_query esq WHERE esq.subject_item_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE FROM eclinical_manual_query_reply emqr WHERE emqr.manual_query_id IN (
    SELECT emq.id FROM eclinical_manual_query emq WHERE emq.subject_item_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids))
);


DELETE FROM eclinical_manual_query emq WHERE emq.subject_item_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE
FROM eclinical_subject_item r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_item esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids)) as a);


#### About Question
DELETE
FROM eclinical_subject_question r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esq.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_ig esi on esc.id = esi.subject_version_id
             join eclinical_subject_question esq on esi.id = esq.subject_ig_id
    WHERE es.id not in (subject_ids)) as a);

#### About IG

DELETE FROM eclinical_subject_ig_status esis WHERE esis.subject_ig_id IN (
    SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_ig esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids)
);

DELETE FROM eclinical_subject_ig_relate esir WHERE esir.ig_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_ig esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids)) or esir.relate_id IN (SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_ig esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE
FROM eclinical_subject_ig r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esi.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_ig esi on esc.id = esi.subject_version_id
    WHERE es.id not in (subject_ids)) as a);

#### About Form

DELETE FROM eclinical_subject_form_status esfs WHERE esfs.subject_form_id IN (SELECT esf.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_form esf on esc.id = esf.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE FROM eclinical_subject_form_labversion WHERE subject_form_id IN (SELECT esf.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_form esf on esc.id = esf.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE
FROM eclinical_subject_form r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esf.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_form esf on esc.id = esf.subject_version_id
    WHERE es.id not in (subject_ids)) as a);

#### About Visit

DELETE FROM eclinical_subject_visit_status esvs WHERE esvs.subject_visit_id IN (
    SELECT esv.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_visit esv on esc.id = esv.subject_version_id
    WHERE es.id not in (subject_ids));

DELETE
FROM eclinical_subject_visit r
WHERE r.id IN (SELECT a.id FROM (
    SELECT esv.id
    FROM eclinical_subject es
             join eclinical_subject_crfversion esc on es.id = esc.subject_id
             join eclinical_subject_visit esv on esc.id = esv.subject_version_id
    WHERE es.id not in (subject_ids)) as a);

## Subject Phase

DELETE
FROM eclinical_subject_phase_property e
WHERE e.subject_phase_id IN (SELECT esp.id
                             FROM eclinical_subject_phase esp
                             WHERE esp.subject_id not in (subject_ids));

DELETE
FROM eclinical_subject_phase e
WHERE e.subject_id not in (subject_ids);


DELETE
FROM eclinical_subject_phase_history e
WHERE e.subject_id not in (subject_ids);

DELETE FROM eclinical_sdv_subject ess WHERE ess.subject_id not in (subject_ids);

DELETE
FROM eclinical_subject_crfversion_history e
WHERE e.subject_id not in (subject_ids);

DELETE
FROM eclinical_subject_crfversion e
WHERE e.subject_id not in (subject_ids);

DELETE
FROM eclinical_subject es
WHERE es.id not in (subject_ids);


```
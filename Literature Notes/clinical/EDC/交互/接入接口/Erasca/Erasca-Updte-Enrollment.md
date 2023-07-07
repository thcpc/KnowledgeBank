url = "http://dev-03-app-01.chengdudev.edetekapps.cn/api/external/edc/erasca/sponsor/{sponsor_name}/lifecycle/{lifecycle}/study/{study_name}"

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
   <ODM xmlns="http://www.cdisc.org/ns/odm/v1.3" ODMVersion="1.3" FileType="Transactional" FileOID="1" CreationDateTime="d">
    <ClinicalData StudyOID="ERAS-801-01" MetadataVersionOID="v4.0.2.1+20211110033817017">
        <SubjectData SubjectKey="002-003" TransactionType="Update">
            <SiteRef LocationOID="002"/>
             <StudyEventData StudyEventOID="Screening">
                <FormData FormOID="Enrollment" TransactionType="Update">
                    <ItemGroupData ItemGroupOID="ENR">
                        <ItemData ItemOID="ENRDAT" Value="07-DEC-2021"/> 
                        <ItemData ItemOID="ENRPART" Value="Part A-1a"/>
			             <ItemData ItemOID="ENRDOSE" Value="10 mg QD"/>
                    </ItemGroupData>
                </FormData>
            </StudyEventData>
        </SubjectData>
    </ClinicalData>
</ODM>
```
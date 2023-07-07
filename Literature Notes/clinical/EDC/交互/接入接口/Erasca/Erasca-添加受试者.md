url = "http://dev-03-app-01.chengdudev.edetekapps.cn/api/external/edc/erasca/sponsor/{sponsor_name}/lifecycle/{lifecycle}/study/{study_name}"

insert_subject.xm
```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
   <ODM xmlns="http://www.cdisc.org/ns/odm/v1.3" ODMVersion="1.3" FileType="Transactional" FileOID="1" CreationDateTime="d">
    <ClinicalData StudyOID="ERAS-801-01" MetadataVersionOID="v4.0.2.1+20211110033817017">
        <SubjectData SubjectKey="002-003" TransactionType="Insert">
            <SiteRef LocationOID="002"/>
            <StudyEventData StudyEventOID="SUBJECT">
                <FormData FormOID="SUBJECT" TransactionType="Update">
                    <ItemGroupData ItemGroupOID="SUBJECT">
                        <ItemData ItemOID="SITEID" Value="002"/>
                        <ItemData ItemOID="SUBJNO" Value="003"/>
                        <ItemData ItemOID="SUBJID" Value="002-003"/>
                    </ItemGroupData>    
                </FormData> 
            </StudyEventData>   
        </SubjectData>
    </ClinicalData>
</ODM>
```

```python
def send_xml(token, uri, xml_path):  
    multi_part = MultipartEncoder({'file': (xml_path, open(xml_path, 'rb'), 'application/xml')})  
    headers = {"Content-Type": multi_part.content_type, "Authorization": token}  
    resp = requests.post(url=uri, data=multi_part, headers=headers)  
    print(resp.content)  
  
  
def insert_subject(token, sponsor_name, study_name, lifecycle):  
    xml = "insert_subject.xml"  
    uri = f"http://dev-03-app-01.chengdudev.edetekapps.cn/api/external/edc/erasca/sponsor/{sponsor_name}/lifecycle/{lifecycle}/study/{study_name}"  
    send_xml(token, uri, xml)

if __name__ == '__main__':  
    token = external_token(dict(appId="erasca", security="erasca"))  
    insert_subject(token, "chenpengcheng", "282-HI-104[866]", "dev")
```

[[external_token| 获取Token]]

[[Create Subject | 相关系统功能]]

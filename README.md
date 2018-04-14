# device-timeline

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

```

## 数据库如何临时导出到`src/components/data.js`
```SQL
SHOW VARIABLES WHERE variable_name='group_concat_max_len';
SET group_concat_max_len=65536;

SELECT CONCAT('{label: ''', RIGHT(uuid, 8), ''', times: [ {', 
    GROUP_CONCAT(
        'starting_time:', UNIX_TIMESTAMP(ctime)*1000, 
        ', ending_time:', UNIX_TIMESTAMP(utime)*1000
        SEPARATOR '}, {'
    ), 
'} ]}') AS content  FROM gaq_status WHERE ctime > '2018-03-20' AND status_type='dev_online' AND "value"=1 GROUP BY uuid
UNION
SELECT CONCAT('{label: ''', RIGHT(uuid, 8), ''', times: [ {', 
    GROUP_CONCAT(
        'starting_time:', UNIX_TIMESTAMP(ctime)*1000, 
        ', ending_time:', UNIX_TIMESTAMP(utime)*1000
        SEPARATOR '}, {'
    ), 
'} ]}') AS content  FROM gspe_status WHERE ctime > '2018-03-20' AND status_type='dev_online' AND "value"=1 GROUP BY uuid
UNION
SELECT CONCAT('{label: ''', RIGHT(uuid, 8), ''', times: [ {', 
    GROUP_CONCAT(
        'starting_time:', UNIX_TIMESTAMP(ctime)*1000, 
        ', ending_time:', UNIX_TIMESTAMP(utime)*1000
        SEPARATOR '}, {'
    ), 
'} ]}') AS content  FROM gspt_status WHERE ctime > '2018-03-20' AND status_type='dev_online' AND "value"=1 GROUP BY uuid;
```
导出后，增加逗号等格式微调即可。
关联 [[grep]] 使用
## 只显示当前行
```shell
docker logs --tail 100000 edc1 2>&1 | grep 'batchReplaceRuleExpression cost'
```

## 显示过滤行及后续行数

```shell
docker logs -f edc1 2>&1 | grep "on-demand R.Script execute failure" -A30
```

```bash
# 提交时换行符自动转换为lf
git config --global core.autocrlf input
# 提交时，不允许换行符混合提交 
git config --global core.safecrlf true
```



# 最佳实践

> .editorconfig

```properties
root = true

[*]
end_of_line = lf

```

> cmd

```bash
# 提交时换行符自动转换为lf
git config --global core.autocrlf input
```

## 实现效果

git仓库中全部为LF；从仓库中检出为LF，且不会自动转换导致文件损坏；文件编辑时，自动转换为LF；部分CRLF格式文件未编辑，直接上传，本地不提示且本地编码仍未CRLF，但仓库为LF，其他人检出时，为LF。

不强制要求先转为LF在上传，避免大量文件上传时，修改格式工作量不小，且无太大意义。
# 上传前五查（本地 → GitHub）

照着走一遍，基本不会出事。

| 查什么 | 怎么查 | 出事的样子 |
|---|---|---|
| 有没有密钥 | `repo.ps1 -Action scan -Path <目录>` | 把 `.env`、`id_rsa`、token 一起推上去，等于公开送人 |
| 有没有大文件 | 同上，看 >50MB 列表 | 单文件 >100MB 直接被 GitHub 拒，推送整个失败 |
| 有没有依赖目录 | 看 `node_modules/`、`.venv/`、`__pycache__/` | 仓库几万文件、几百 MB，克隆慢到没人愿意用 |
| 有没有私人内容 | 人眼看目录 | 照片、聊天记录、客户/医院资料、带个人信息的截图 |
| `.gitignore` 在不在 | `scan` 最后一行会写 | 缺失时上面那些东西会自动被 `git add -A` 抓进去 |

## 命令

```powershell
# 1. 先看，不动任何东西
& "E:\灯芯\日常\.agents\skills\github\scripts\repo.ps1" -Action scan -Path "<项目目录>"

# 2. 建公开仓库并推首版（默认 public，想藏就加 -Private）
& "...\repo.ps1" -Action publish -Path "<项目目录>" -Name "<英文仓库名>"

# 3. 以后更新
& "...\repo.ps1" -Action push -Path "<项目目录>" -Message "改了什么"
```

## 踩过的坑

- `gh repo create --push` 要求目录里**已有提交**，空仓库直接推会报 `--push enabled but no commits found`。脚本现在会自动提交首版，手动做时别漏了 `git add` + `git commit`。
- 中文路径 git 本身没问题，但 `aapt2` / `javac` 这类工具不认；要构建就先 `subst` 到纯英文盘符。
- Windows 下会看到 `LF will be replaced by CRLF` 警告，无害，想让仓库里统一 LF 就在 `.gitattributes` 里写 `* text=auto eol=lf`。
- 远程仓库建错了不要慌：GitHub 的删除入口在 Settings 最底部，删掉后本地重新 `-Action publish` 即可，名字可以复用。

# gh-publish-smoke

一个用来验证「本地项目 → GitHub」上传流程的最小项目。跑通就可以删掉。

## 这是什么

灯芯工作区的 `github` 技能负责两件事：把克隆的项目收进 `灯芯/GitHub/<owner>/<repo>`，以及把本地项目推到 GitHub。
这个仓库是后者的试跑产物：目录极小、没有隐私内容，用来确认建仓、提交、推送、更新全部通畅。

## 怎么验证的

1. `repo.ps1 -Action publish -Path <目录> -Name gh-publish-smoke` 建公开仓库并推首版。
2. 改一行文件，`repo.ps1 -Action push -Path <目录> -Message "..."` 推第二版。
3. `gh repo view --json visibility,url` 确认可见性与地址。

## 删掉它

```powershell
gh repo delete YKao233/gh-publish-smoke --yes
```

## 更新测试

这一行是第二次提交，用来确认 push 流程。


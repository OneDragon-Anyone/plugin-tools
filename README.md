# plugin-tools

ZenlessZoneZero-OneDragon 第三方插件的官方校验、打包和发布工具，以及供插件仓库调用的可复用工作流。

插件作者不需要复制本仓库的任何文件。从 [plugin-template](https://github.com/OneDragon-Anyone/plugin-template) 创建的仓库，其 `validate.yml` 和 `release.yml` 会直接调用这里的工作流。

## 工作流

- `.github/workflows/reusable-validate.yml`：校验插件仓库并试打包。
- `.github/workflows/reusable-release.yml`：推送 `v*` tag 时校验、打包并创建 GitHub Release，附件为 `<插件ID>.zip`。

作者仓库引用示例：

```yaml
uses: OneDragon-Anyone/plugin-tools/.github/workflows/reusable-release.yml@v1
```

`@v1` 跟随 v1 系列的修复，不会有破坏性变化；想完全锁定可换成提交 sha。

## 本地验证（可选）

CI 会在推送时自动校验，本地验证只是为了少等一轮。在插件仓库根目录执行，版本与 CI 的 `@v1` 一致：

```bash
git clone --depth 1 --branch v1 https://github.com/OneDragon-Anyone/plugin-tools.git .plugin-tools
python .plugin-tools/scripts/validate_plugin.py --repo . --strict
python .plugin-tools/scripts/build_zip.py --repo .
```

把 `.plugin-tools/` 加进插件仓库的 `.gitignore`。

## 维护约定

- `v1` 是浮动 tag，指向最新的 `v1.x.y`。破坏性改动发 `v2`。
- 本仓库需开启 `v*` tag 保护，工作流保持 `persist-credentials: false`，并在 `job.workflow_sha` 处检出自身，确保工具与工作流是同一个提交。
- `plugin-market` 也读取本仓库的脚本。

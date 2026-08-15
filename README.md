# my-dsh-config

DeepSeek Harness (`dsh`) 的多端配置。提供商、模型目录、推理等级和 profile 清单放在这里；API key 不进仓库。

## 会同步什么

| 路径 | 作用 |
| --- | --- |
| `settings.yaml` | 自定义提供商 CPA、模型列表、`reasoningEfforts`、默认模型 |
| `profiles/web/` | `dsh web` 的 profile 清单与 patch |
| `profiles/headless/` | 无界面 profile 的清单与 patch |
| `agent-presets/` | 本机自定义 Agent 预设（若有） |

密钥在各机的 `$DSH_HOME/.credentials.yaml` 或环境变量里，例如 `CPA_API_KEY`。

## 用到哪台机器上

`$DSH_HOME` 常见是 `~/.dsh`，有的部署是 `/var/lib/deepseek-harness`。

```sh
# 把仓库里的文件拷到本机 DSH 家目录（按你的实际路径改）
export DSH_HOME="${DSH_HOME:-$HOME/.dsh}"
cp settings.yaml "$DSH_HOME/settings.yaml"
mkdir -p "$DSH_HOME/profiles/web" "$DSH_HOME/profiles/headless"
cp profiles/web/package.json profiles/web/cordis.patch.yml "$DSH_HOME/profiles/web/"
cp profiles/headless/package.json profiles/headless/cordis.patch.yml "$DSH_HOME/profiles/headless/"
# 可选：自定义 preset
mkdir -p "$DSH_HOME/.agent-presets"
cp -R agent-presets/. "$DSH_HOME/.agent-presets/"
```

然后在 Web 设置里填 API key，或导出同名环境变量。改完 `settings.yaml` 后重启 `dsh web`，或打开设置里对应提供商再保存一次。

CPA 的 `baseURL` 走 Tailscale。另一台机器需要在同一 tailnet 里才能连上网关。

## 不要提交

- `.credentials.yaml`
- `sessions/`、`storages/`
- `profiles/**/node_modules/`

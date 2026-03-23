核心出装：**Docker + Qwen Code (Free Tier)**

在项目根目录里用一行 docker 命令即可启动：

```sh
# Quick start
docker run \
    --rm -it \
    -v .qwen:/root/.qwen \
    -v .:/ws \
    -w /ws \
    --pull always \
    ghcr.io/qwenlm/qwen-code:0.12

# Upgrade
docker pull ghcr.io/qwenlm/qwen-code:0.12
```

可以根据开发需要把 port expose 加上（`-p 8080:5173`），然后配合 ssh port forwarding 即可轻易实现在本机访问开发机的 dev server。

还可以把这行命令包装成 bash function 放在 .bashrc 里，作为一个快速开始相当便捷：

```sh
function qwen {
    local CMD
    if [[ "$1" != "" ]]; then
        local CMD="qwen $@"
    fi
    docker run \
        --rm -it \
        -v .qwen:/root/.qwen \
        -v .:/ws \
        -w /ws \
        --pull always \
        ghcr.io/qwenlm/qwen-code:0.12 $CMD
}
```

Qwen Code 提供了一定的免费额度，进入 CLI 后访问链接登陆即可，无需配置 API。免费额度每日刷新，详见 [身份验证-Qwen OAuth](https://qwenlm.github.io/qwen-code-docs/zh/users/configuration/auth/)：

> 费用与配额：完全免费，配额为每分钟 60 次请求，每天 1,000 次请求。

---

优点：

- 极致的轻量，不用额外安装 CLI，不用处理 node/npm 环境
- Docker 自带沙盒环境，不怕 LLM 瞎折腾
- 免费额度，不用配置 API

缺点：

- 容器内是 root，所以生成/修改的文件也都是 root 所属
  - 但我想既然你都能用 docker 了，大概也是有 sudo 权限的吧...
- Free Tier 不能选择模型，模型能力我也还没摸清楚

---

OpenCode 版本：

```sh
# Quick start
docker run \
    --rm -it \
    -v .share_opencode:/root/.local/share/opencode \
    -v .config_opencode:/root/.config/opencode \
    -v .:/ws \
    -w /ws \
    --pull always \
    ghcr.io/anomalyco/opencode

# Upgrade
docker pull ghcr.io/anomalyco/opencode
```

Image 体积很小，但启动后更占内存。同样有免费额度，同时支持一些新模型的试用，例如 Xiaomi MiMo-V2-Pro 。
# 麻将计分器（Mahjong Scorer）

一个基于Web的麻将计分工具，支持实时同步到GitHub仓库。

## 功能特性

- 🎮 支持4位玩家的分数记录
- 📊 自动计算当前轮次分数和总分数
- 🔄 支持通过GitHub API进行数据同步
- 💾 本地存储和远程同步双重保障
- 🎨 简洁美观的界面设计

## 技术实现

- HTML5 + CSS3 + JavaScript
- GitHub API v3 用于数据同步
- 本地存储（localStorage）用于离线数据保存

## 配置说明

### GitHub API Token 配置

本项目使用 GitHub API 进行数据同步，需要配置名为 `MINE_TOKEN` 的 GitHub 仓库加密密钥。

#### 方法一：使用 GitHub Actions 构建时注入（推荐）

1. 在 GitHub 仓库中添加名为 `MINE_TOKEN` 的加密密钥：
   - 进入仓库 → Settings → Secrets and variables → Actions
   - 点击 "New repository secret"
   - 名称填写 `MINE_TOKEN`，值填写你的 GitHub Personal Access Token

2. 创建 `.github/workflows/build.yml` 文件：

```yaml
name: Build and Deploy
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Inject Environment Variables
        run: |
          sed -i "s/window.MINE_TOKEN/\'${{ secrets.MINE_TOKEN }}\'/" index.html
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: .
```

#### 方法二：使用支持环境变量的静态托管服务（如 Netlify、Vercel）

1. 在 Netlify/Vercel 中添加环境变量：
   - 变量名：`MINE_TOKEN`
   - 变量值：你的 GitHub Personal Access Token

2. 修改构建配置，在构建时将环境变量注入到 HTML 文件中。

#### 方法三：开发环境配置

在开发环境中，可以直接在浏览器的控制台中设置环境变量：

```javascript
window.MINE_TOKEN = 'your-github-token';
```

或者使用项目提供的 Token 管理界面手动输入 Token。

## GitHub Personal Access Token 权限要求

创建 GitHub Personal Access Token 时，需要勾选以下权限：

- `repo` - 完全控制私有仓库

## 本地开发

1. 克隆仓库：
```bash
git clone https://github.com/baigouzi/majiangjifen.git
```

2. 启动本地服务器：
```bash
npx http-server -p 8080 -c-1
```

3. 在浏览器中访问：`http://localhost:8080`

## 数据结构

同步到 GitHub 的数据结构：

```json
{
  "nicknames": {
    "player1": "玩家1",
    "player2": "玩家2",
    "player3": "玩家3",
    "player4": "玩家4"
  },
  "totalScores": {
    "player1": 100,
    "player2": 200,
    "player3": 300,
    "player4": 400
  },
  "currentRoundScores": {
    "player1": 10,
    "player2": 20,
    "player3": 30,
    "player4": 40
  },
  "history": [
    {
      "timestamp": 1234567890,
      "playerWon": "player1",
      "scores": {
        "player1": 10,
        "player2": -10,
        "player3": -10,
        "player4": -10
      }
    }
  ],
  "banker": "player1",
  "bankerCount": 3
}
```

## 浏览器兼容性

- Chrome (推荐)
- Firefox
- Safari
- Edge

## 许可证

MIT License

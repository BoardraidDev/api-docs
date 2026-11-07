# BoardRaid API Docs
面向公开只读网页集成的接口初稿，基线为正式版 0.5.5。机器契约见 [openapi.json](openapi.json)。
## 公开接口
| 方法 | 路径 | 参数与含义 |
| --- | --- | --- |
| GET | /api/status | 服务状态，不应当作实时在线玩家名单 |
| GET | /api/leaderboard | limit 默认 50，范围 1–200；公开积分榜 |
| GET | /api/player | name 为当前昵称，最多 32 字符；未找到返回 found=false |
| GET | /api/head | name 为昵称，返回 PNG；不可用返回 404 |
## 示例
`curl --fail --max-time 10 'https://api.boardraid.cn/api/leaderboard?limit=10'`
响应示例为合成结构，不代表线上真实玩家：`{"ok":true,"entries":[],"total":0,"totalProfiles":0}`。
玩家不存在的语义为 `{"ok":true,"query":"ExampleName","found":false}`，不能把 found=false 当作账号被删除。
## 使用约定
以上查询不要求游戏登录令牌。不要向第三方发送微软 Token。对名字做 URL 编码；不要将未知响应字段直接拼成 HTML。
客户端设置超时、有限次数重试与指数退避，缓存榜单并合并重复头像请求。当前未承诺固定公共限流额度或 SLA，429 后应停止并退避，不照搬其他平台的限流数字。
隐藏匹配分和原始账号身份不是公开字段，不尝试推算或收集。历史资料的昵称会随身份同步更新；历史统计不代表全量永久档案。
## 范围
暂不把回放写入、游戏 WebSocket、内部管理、部署、身份认证和社交绑定纳入公共第三方 API。公开 HTTP 可访问不自动构成支持承诺。schema 使用宽容读取策略，新增字段可忽略。
接口错误以 HTTP 状态与实际 JSON 为准；网络和代理错误可能返回非 JSON。OpenAPI 中的 503 是调用方需处理的服务故障，不保证服务会返回特定错误对象。

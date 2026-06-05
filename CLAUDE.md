# wxb.me 编写行为准则

个人站 **wxb.me**（Vercel 项目 `wxb-me`，CLI 直接部署、无 git 远程）。核心是单文件 `index.html`：水墨风横向手卷主页 + 全屏「研习」11408 考研自学知识库。**用户在国内**。
进度/数据模型/动画框架细节见 `~/.claude/projects/-Users-wxb-projects-wxb-me/memory/study-codex-11408.md`，开工前先读。

## 一、架构与依赖（最高优先级）
1. **单文件**：所有 HTML/CSS/JS/内容内联在 `index.html`。禁止拆外部文件（外部 JS 预览/部署不加载 → 页面空白）。
2. **零第三方/国外 CDN**：MathJax、字体、任何库都必须**下载进仓库自托管**（国内会墙 jsdelivr / Google Fonts，加载失败 → 公式变反斜杠、字体丢失）。
3. **优雅降级**：中文字体回退栈 `Source Han Serif / Songti SC / SimSun / serif`，资源没加载也可读。

## 二、代码硬规则（不照做必出 bug）
4. **含 LaTeX 的内容必须用 `R\`\`` (String.raw) 包裹**（`var R=String.raw`）；普通模板串会吞反斜杠。
5. **每个 SVG 的 `<marker>` 箭头 id 在该 SVG 自己的 `<defs>` 里定义**；一次只渲染一个考点，跨 SVG 引用 id 会失效。
6. 改结构用**精确锚点或脚本批量改**，**务必保留既有深度内容**，不能覆盖。
7. 考点数据两态：紧凑 stub `{ id, t, freq, tags, todo }` ↔ 完整 `{ ...concept/visual/examples[{q,a}]/pits/mn }`；填实时整体替换 `todo` 部分。

## 三、内容质量与完整性
8. **完整性以官方考试大纲为骨架，绝不靠记忆**。补全前先**联网核对当年大纲**逐条比对，覆盖状态做成侧边栏可见的 ✅/[待完善]，让"漏没漏"可验证。
9. 每个考点统一模板：`lead` 导语 → 多个讲透"为什么"的 `<h4>` → SVG 图解 → 代码/表格 → 梯度例题(基础→陷阱→综合) → 易错点 → 记忆口诀。
10. **原创内容与原创配图**，不抄任何教材文字/图片（版权）。

## 四、图与动画
11. 凡"结构在变化"的操作（指针改写、top 移动、旋转、遍历、查找夹逼）**该配图就配、能配动画更好**——像编教材作者一样自己判断，不等用户逐个点。
12. 动画用既有播放器：`<div class="animbox" data-anim="ID"></div>` + 注册 `ANIMS['ID']`，自带 ▶/⏭/⟲；切换考点前 `stopAnims()` 清定时器。
13. 视觉统一：墨 `#0f0f0f`、红 `#b83830`、格子 `#efe8d6`、纸底；复用 `.rfig / .animbox / .lead / .tip / .warn / h4`。

## 五、验证与部署（每批必走）
14. **改完先在预览验证再部署**：reload → 确认 `window.STUDY` 能加载(无语法错) → 打开对应考点看渲染/动画 → console 无 error。
15. **以截图为准，别信 `getComputedStyle`**：预览在 CSS 过渡期取到的是起始帧值（opacity 读成 0 是假象）。
16. **小步快跑**：填几个就验证、部署一批，尽早暴露语法错误。
17. **部署流程固定**：`cp` worktree 的 `index.html`(+自托管资源) 到主仓库 `/Users/wxb/projects/wxb.me/` → `npx vercel deploy --prod --yes` → **curl wxb.me 验证**返回新内容(关键标记 / HTTP 200 / 无 jsdelivr)。

## 六、协作
18. **持续更新项目记忆**（进度、约定、坑），保证跨会话无缝接力。
19. **大改前先对方向**：深度/风格/范围这类返工成本高的决定，先做一个样板给用户确认再批量铺开。

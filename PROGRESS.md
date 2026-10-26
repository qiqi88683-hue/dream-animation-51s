# PROGRESS / 断点续做记录

最后更新：2026-10-26（Asia/Shanghai，v04侧面步态与版本归档）

## 最新真实状态（以此段为准）

### v04 侧面步态与实际目录整理（2026-10-26）

- 用户认可v03画风和自然人物比例，要求保留；指出动作像纸片缩放，建议侧面运动和适度立体渲染，要求线条更抽象且保留轮廓；同时要求实际整理GitHub目录并记录每次意见。
- 已新增Three.js空间渲染、双骨IK与支撑脚锁定。固定绘画分层表面绑定髋/膝/踝和肩/肘/腕，带轻微曲面和前后深度；不再用整个人物缩放表现行走。相机保持焦距，沿街道侧面跟随。圆柱几何人体测试不符合已认可造型，已弃用且未发布。
- 新素材public/painting/v04/street-side.png与hero-side.png由B方向编辑生成；保留原生成文件，采用更概括的排线和侧面基本轮廓。头脸、人物身份和动作的艺术质量仍待用户审看。
- 代码：src/spatial/gait.ts、paintedHuman.ts、SideWalkPreview.tsx；命令npm run render:sidewalk，输出out/sidewalk-v04。软件WebGL使用swangle、抗锯齿、固定绘画纹理、空间遮挡和接触阴影。
- 类型检查与10项测试通过；新增验证支撑脚世界位置锁定、骨段长度恒定、摆动脚离地。已独立渲染关键帧并检查；最终视频/发布结果待下方追加。
- 已实际迁移全部历史review/文件到previews/v01-vector、v02-style、v03-painted，按stills/videos/downloads/records分类；历史媒体与原ZIP字节哈希全部一致，迁移前完整备份已校验。review/不再保留重复入口。
- 每版feedback.md保留用户原话、认可项与修改项；feedback/2026-10-26.md按对话顺序串联，未杜撰历史时间或艺术批准。
- 本轮仍是8秒技术/艺术预览，不是原7镜的完整交付；临时环境声，无正式对白和混音。
- 新视频实际输出完成：previews/v04-sidewalk/videos/preview-1080p.mp4（约14MB）及preview-720p.mp4（约3.1MB），8秒、30fps、240帧、H.264/AAC。两版完整解码、faststart和尺寸检查通过。
- previews/v04-sidewalk/downloads/v04-preview.zip约33.2MB，完整性通过；四版manifest、当前播放页/Markdown链接、逐版feedback.md及所有原始文件SHA-256保留检查均通过。
- 失败与恢复：两路软件WebGL初始化超时；调整为单路并发及120秒初始化等待，复用源码/素材未改变且已核验一致的关键帧，随后视频成功完成。不是跳过渲染或放宽艺术验收。
- Cloud启动说明草稿已更新到侧面步态主线，保留原安装与凭据要求；草稿尚未发布。本轮Git交付包含预览实际结构、日志及项目说明，工程源码/素材仍保存在当前Cloud工作区，未随预览提交。


### B风格动态预览 v0.3（2026-10-26）

- 用户明确确认“B是我要的风格”，并授权按此制作预览；B方向已确认，动态效果仍待评审。
- image_gen从B生成固定的背景与三个人物RGBA图层，保存在public/painting/v03/。透明通道已检查；原生成图保留于/workspace/generated_images。
- 新增src/painting/PaintedPreview.tsx及motion.ts：固定绘画图层、确定性网格步态/手臂微摆、三个人物独立轨迹/前后遮挡、细微树冠运动和镜头推进。不是逐帧生成图片，也不是只对一张完整B图缩放。
- 预览是独立8秒风格动作测试，不是原七镜中某镜已完成；当前没有电话/拒接/捂胸动作。背景车流、脸部表演和复杂步态仍未完成；声音仅为临时合成城市底声。
- 运行node scripts/render-painted-preview.mjs可重建关键帧和1080p预览；--stills-only仅生成关键帧。源码、测试和生成素材仍在当前工作区，未随预览推送。
- 类型检查通过，8项测试执行并通过，新增覆盖网格不翻折/塌陷、人物身份/尺度和轨迹的稳定性。
- 已渲染out/painted-v03/B-style-preview-8s-1080p.mp4；独立重渲染第90帧的PNG SHA-256一致，无浏览器报错。
- ffprobe与完整解码已通过：H.264、1920×1080、30fps、240帧、AAC音轨。发布结果见随后追加记录。
- 另导出1280×720轻量版（约3.8MB）；1080p约18.2MB。两个版本均完整解码、规格及faststart检查通过。
- review/painted-v03内保存两份MP4、关键帧、离线播放页、README、来源记录、哈希清单和验证记录；review/painted-v03.zip约26.5MB，ZIP完整性检查通过。
- 已提交并推送本轮预览到preview/prototype-v01，提交0b36e9b；远程分支已核验。只推送review预览产物，未推送工程源码或main；动态效果待用户评审。


### 美术修正 v0.2（2026-10-26）

- 用户明确否定v0.1美术，要求梵高式细密曲线排线、疏密控制浓淡、深沉油画与素描之间的质感；男主高但不要突出。v0.1动画不再作为目标风格。
- 新做两张image_gen静态方向图：偏油画色层、偏曲线排线。它们是美术探索，不是程序动画成片，不代表实际帧间一致性已经实现。
- 新增src/painting/flow.ts的固定种子曲线笔触机制，密度增加只添加笔触，不打乱已有笔触。StrokeStudy构图是材质实验，不是角色/场景定稿。
- 类型检查与6项测试通过（含新增笔触密度/稳定性检查）。后续实际输出/发布结果见下方追加记录。
- 旧Character、City等动画造型尚未替换成新绘画方向，需先收敛风格再重建角色/场景材质；不能把概念图当作新风格动画已完成。
- 实际输出：out/style-v02/stroke-study.png、stroke-stability-4s.mp4及verification.json；笔触图独立渲染两次SHA-256一致，4秒视频已核对H.264 / 960×540 / 30fps / 120帧 / 无音轨，无浏览器报错。
- review/style-v02内已保存两张概念图、代码材质图/视频、来源记录、哈希清单、README和离线页；review/style-v02.zip为完整包。ZIP完整性检查通过。
- 已沿用获授权的预览发布方式，将本轮review文件推送到preview/prototype-v01，提交735fdc36d3812283ddad3d76acc92865a647f415；远程核验成功。工程源码和本地规格/进度仍未提交或推送。
- 同时发现用户已通过PR #1将旧预览合入main（7b2ae368417333aae3f288653f5204be7a6c63b0）；已只读获取确认改动仅为旧review文件，本轮未向main推送。


- [x] 用户明确回复“开始代码制作”；确认第2镜左手取手机，选择可交付的合成配音方向
- [x] 初始化Remotion / React / TypeScript / SVG工程，固定依赖版本与锁文件
- [x] 男主正反面风格测试板（尚未艺术验收）
- [x] 第1镜空间摄影机原型与10秒540p预览
- [x] 第5–6镜来电/拒接/呼吸动作原型与18秒540p预览
- [x] 三句台词时间轴；女二前30帧停顿，拒接后仍有女声
- [x] 自制占位城市底声与铃声；区分占位声和正式音频
- [x] 保存Cloud安装/启动配置草稿及语音凭据要求（未发布）
- [ ] 男主、曲线语言、环境美术与动作获得用户验收
- [ ] 第2–4、7镜完整动画
- [ ] 合成女声实际生成与试听；正式环境人声、耳机音乐、拟音和混音
- [ ] 全片检查、1080p30 / 51秒最终输出

**已有代码与原型视频；没有51秒完整成片，没有实际生成的女声。不宣称艺术验收或正式声音验收通过。**

## 本轮实际文件与验证

- 代码：src/Character.tsx（共用造型）、src/City.tsx及projection.ts（空间与摄影机）、src/PhoneScene.tsx（手机/手部/呼吸）、src/Root.tsx（每镜出口、审核标记与音轨）、src/timeline.ts（时长与事件）。
- 工具：scripts/render.mjs、make_audio.py、verify_media.py、voices.ts；运行与恢复说明：docs/DEVELOPMENT.md。
- 风格板：out/style-board.png（1920×1080）。
- 关键帧：out/shot01-aerial.png、shot01-descent.png、shot01-back.png、shot05-hand.png、shot05-reject.png、shot06-breath.png。
- 视频：out/Shot01-prototype-540p.mp4（10秒/300帧），out/DialogueStudy-prototype-540p.mp4（18秒/540帧）；均为960×540、30fps、H.264，含占位音轨。
- 输出清单：out/stills-manifest.json、out/render-manifest.json。
- npm ci：成功以锁文件重建；Cloud使用NPM_CONFIG_CACHE=/workspace/.cache/npm，避开不可写的默认缓存。
- npm run typecheck：通过，覆盖src、tests和配音脚本。
- 配音命令预检：缺少DREAM_TTS_KEY时明确失败并在网络请求前退出；已实际验证，没有付费生成请求。
- npm test：5项执行并通过，覆盖1530帧分镜、对白停顿/拒接、声强、微弱呼吸及连续投影。
- npm run audio:placeholders：成功；生成public/audio/generated内两份WAV，不是正式人群声。
- npm run render:stills：7张图成功；Shot05第190帧独立渲染两次的PNG SHA-256一致；修正SVG路径后无浏览器报错。
- npm run render:prototypes及verify:media：两个目标均完成，编码、尺寸、30fps、300/540帧及音轨检查通过，明确报告女声缺失。
- npm run studio -- --port=3000 --no-open：实际启动，内部HTTP GET为200及Remotion HTML；没有创建用户可用的localhost链接。
- 已视觉审看造型及摄影机首/中/末帧、手机/胸部关键帧；这不是用户艺术批准或全片逐帧验收。

## 未完成与恢复

1. 美术仍是简化矢量测试，需改善曲线流动感、建筑/树冠、人群多样性与步态，精修手部结构、拒接和轻捂胸动作；未通过不量产其余镜头。
2. 合成配音候选OpenAI Speech API，官方SDK参数已核对。当前无DREAM_TTS_KEY，未请求付费生成，实际音色及API全链路未验证。
3. 已保存Cloud install_script、start_skill、DREAM_TTS_KEY要求，并新增api.openai.com目的域。用户可在环境设置安全填入密钥、审阅保存并发布；本轮未发布。不要把密钥发到聊天。
4. Mac尚未实机验证，字体与跨平台一致性待检查。当前Linux可重复运行。
5. 用户随后明确授权推送预览。已将review/中7份文件（人物图、两段视频、离线页面、README、哈希清单及ZIP包）提交并推送到preview/prototype-v01，提交a6ef771c26c290b8e05bcb3bad78d0428a73baa3；已用git ls-remote核对远程分支。main仍为df22dcafea963ea222aa6435618d8b133d368c53。工程源码和本地文档改动尚未提交/推送；原始source附件未修改。out、node_modules、生成音频被Git忽略，转移前需分别保存。
6. 恢复先读AGENTS、本记录与开发说明，再查实际文件。沿用现有隔离检出，不自行创建worktree。缺依赖npm ci，缺占位音npm run audio:placeholders。
7. 凭据就绪后npm run voices，试听并校准时段，再DREAM_USE_VOICES=1渲染。单镜重做用npm run render:prototypes -- --shot=Shot05。

---

## 以下为制作前交接历史（已被上方最新状态更新，不表示当前未授权）

历史记录日期：2026-10-26（制作前交接）

## 当前真实状态

- [x] 读取两份原始文档，梳理七镜头与场景信息
- [x] 整理用户后续的二维梦境、平行曲线、忧郁暗色、弱化耳机等更新
- [x] 整理给 Codex 的项目资料和初始工作流
- [ ] 用户授权开始代码制作
- [ ] 安装/初始化视频工程
- [ ] 男主与环境风格测试
- [ ] 镜 1 技术原型与预览
- [ ] 镜 5–6 音画同步原型与预览
- [ ] 镜 2–4、镜 7 完整动画
- [ ] 配音及音效素材确定、混音
- [ ] 全片低清检查与质量修正
- [ ] 1080p30 MP4 最终导出

**状态声明：尚未创建动画代码，也未渲染任何视频片段。**

## 未来每次制作记录模板

```
日期：
阶段与分镜：
已保存的源码/产出路径：
预览文件：
验收通过/失败：
已知问题：
恢复建议和下一步：
Git commit（如有）：
```

## 续做规则

恢复任务前检查实际文件是否存在，阅读这里的历史记录；仅补做缺失或失败的部分，绝不假定聊天里曾经说“完成”就真的存在视频文件。若云端任务创建独立工作区，请先获取前一阶段已提交代码与保存的媒体资源。

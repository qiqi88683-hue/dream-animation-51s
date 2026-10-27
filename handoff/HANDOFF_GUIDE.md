# 新脚本接手手册：复用平行曲线动画工作流

版本1.0 · 2026-10-27。**面向下一次任务，读者无需知道旧聊天。** 用户希望收到别的分镜/脚本后，快速制作风格相似的作品；目标是复用方法、结构与实现基线，而非重复旧剧情。

## 1. 先掌握这六件事

1. 风格核心：粗而连续的平行曲线，弯折塑造体积、疏密塑造浓淡、曲线随动作律动；基本轮廓与微小动作可辨。
2. 视觉默认：自然比例、抽象但不混乱；主角/配角/背景用协调的低饱和色调和浓淡分层。新脚本决定具体情绪、角色和环境。
3. 技术：Remotion/TypeScript统一帧时间轴→世界/骨架/摄影机→二维投影与体积/归属场→Canvas线条→单镜→连续混音→FFmpeg导出。
4. 已实现的是“渲染/动作机制+一个完成案例”，并没有任意分镜JSON一键成片的通用引擎。模板是输入约定，必须适配剧情模块。
5. 当前参考v08已进入收尾；51秒、七镜、旧男主、路口/长椅/手机、无对白、号码模糊和信号式白场结尾，**都不是新项目硬约束**。
6. 现有源码曾只在Cloud；这次交接另附可独立恢复的精简源码/字体/音频包，不能只依据旧技术说明误判“仓库仍无工程”。

详细方法与教训见[工作流总结](WORKFLOW_SUMMARY.md)，但本手册本身已包含启动所需原则与入口。

## 2. 资源入口：即使只收到本文，也能找到基线

- 仓库：https://github.com/qiqi88683-hue/dream-animation-51s
- 交接发布分支：preview/prototype-v01。不要假定默认main已有这些文件。
- [可运行基线ZIP](https://github.com/qiqi88683-hue/dream-animation-51s/raw/preview/prototype-v01/handoff/downloads/parallel-line-baseline-v08.zip)
- [ZIP校验与包说明](https://github.com/qiqi88683-hue/dream-animation-51s/blob/preview/prototype-v01/handoff/baseline-manifest.json)
- [独立恢复验证记录](https://github.com/qiqi88683-hue/dream-animation-51s/blob/preview/prototype-v01/handoff/baseline-verification.json)
- [v08完成参考与媒体入口](https://github.com/qiqi88683-hue/dream-animation-51s/tree/7746de6d3fe831a3dac8ecb460e2fac82c81e7d5/previews/v08-revision)
- [风格形体/分色参考](https://github.com/qiqi88683-hue/dream-animation-51s/blob/7746de6d3fe831a3dac8ecb460e2fac82c81e7d5/previews/v08-revision/stills/poster.png)

ZIP内部有src/、scripts/、tests/、package.json和锁文件、字体及许可证、现有音频/分轨、reference/视觉参考和两份交接文档。没有node_modules、缓存、TTS模型、密钥或旧油画素材。它是精简复现夹具，不包含本仓库全部历史预览。

包里的Root.tsx只注册当前生产与已认可短样，package.json只保留可用命令；依赖锁版本仍沿用原工程。原会话24项测试中，3项早期绘画/网格测试不属于这个精简包；**基线有21项测试**，不能把数量变化当作失败或声称两者范围相同。

## 3. 新任务的第一轮操作

1. 判断当前是“继续旧片”还是“新脚本”。新脚本默认创建独立项目根，保留参考基线；不要直接覆盖已完成影片。
2. 读取用户当前消息和实际附件，检查目录/版本/缺失文件。不要要求用户重贴整段旧聊天。
3. 按[新项目简报模板](templates/PROJECT_BRIEF.template.md)整理规格、身份、镜头/动作、空间、声音和交付约束。分开写已确定、可测试、未知项及假设来源。
4. 查找现有可用源码；若需恢复，从上述ZIP入口下载，核对外部SHA-256，解压到新目录，再核对FILE_MANIFEST.json。不要覆盖一个已有未保存工作的目录。
5. 先做恢复检查和一帧/短样；然后映射新脚本并列出最难动作，保存首个进度断点。
6. 只询问确实阻塞下一步的信息。风格已授权沿用时直接推进；关键新设定用明确假设或最少问题处理，继续不依赖答案的工作。

若环境不能访问仓库或执行代码：明确说明缺少哪些资源/能力，仍可先完成脚本归一、镜头表和实现拆解；不得假称已经恢复或渲染。访问目标仓库和发布沿用新任务已有授权，旧库推送权限不自动扩展到新库。

## 4. 最小恢复命令（用于旧夹具自检）

前提：Node >=22、Python 3.12、FFmpeg/ffprobe、Chrome/Chromium。已验证环境是Linux、Node24.19.0、Chromium；Mac开发和跨平台逐像素一致性尚未实机验证。

从仓库获取ZIP后，先按baseline-manifest.json比对ZIP的SHA-256，再解压：

```bash
python3 -m zipfile -e parallel-line-baseline-v08.zip ./baseline-restore
cd baseline-restore/parallel-line-baseline-v08
python3 - <<'PY'
from pathlib import Path
import json, hashlib
manifest=json.loads(Path('FILE_MANIFEST.json').read_text())
for row in manifest['files']:
    p=Path(row['file'])
    assert p.is_file() and hashlib.sha256(p.read_bytes()).hexdigest()==row['sha256'], str(p)
print('PASS packaged files')
PY
NPM_CONFIG_CACHE="$PWD/.cache/npm" npm ci --ignore-scripts --no-audit --no-fund
npm run typecheck
npm test
npm run render:film -- --quick
```

预期：21项测试通过；out/film-v08生成320、560、930、1422、1455、1490六张关键帧。独立恢复验证记录给出这些PNG与原始v08一致的SHA-256。该命令复现旧夹具，不会自动读取新脚本，也不会渲染整片。

Linux默认浏览器/usr/bin/chromium；其他路径设置REMOTION_BROWSER_EXECUTABLE。Mac可核对实际Chrome安装路径后设置，例如`/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`，此路径示例未实机验证。

现有声音已在包里，画面检查无需重建音频。需要重建时：

```bash
python3 -m venv .venv-audio
source .venv-audio/bin/activate
pip install -r requirements-audio.txt
npm run audio:film
```

本基线的音频只用NumPy/SoundFile，无需TTS密钥或模型；这不意味着新项目必须无对白。若新脚本需要配音，要另行选择并实际验证声源、来源许可、时长和同步。

## 5. 新脚本怎样接到实现

建议将基线复制为独立项目根，例如projects/<new-id>/，不复制node_modules和out。然后按以下顺序适配；不是在旁边放一个JSON就生效。

| 顺序 | 输入/输出契约 | 需要改的地方 |
|---|---|---|
| A 规格与帧表 | fps、尺寸、镜头id/start/frames；[start,end)无间隙 | src/timeline.ts、Root.tsx及所有使用固定尺寸/fps的模块 |
| B 身份与世界 | 稳定角色/道具ID，坐标/单位/比例，初始/结束状态 | model.ts、actor.ts、city.ts或新的场景模块 |
| C 动作与机位 | 每镜动作阶段、接触点、支撑脚、相机目标/路径 | gait.ts、model.ts、pov.ts和projection.ts；新动作需新增实现 |
| D 统一形体出口 | scene(frame)给出shapes/marks/phone?/camera/frame | 参考filmScene(frame)，由新场景替换本片工厂 |
| E 线条与分色 | 形体投影后交给createFieldFromShapes(shapes, frame) | 复用field.ts与Canvas绘制；不要误用绑定旧短样的makeShapes |
| F 声音与文字 | 帧同步事件、分轨、混音、字幕与界面文本 | 新音频脚本/资源、Film.tsx声音入口与文字层 |
| G 导出与验证 | 新镜头出口、范围、总时长、资源/绘制指纹 | render-film.mjs、film-fingerprint.ts、测试及新发布脚本 |

shapes是投影后的基础形体列表，包含kind、二维位置、体积/浓淡、role、layer等；marks是局部细节笔画。新的场景可以继续使用同一线场，而不复制旧城市和人物。模板文件[SHOTLIST.template.json](templates/SHOTLIST.template.json)给出输入结构示例，**现有代码没有解析它的加载器**。

坐标必须明确：基线世界V3是[X平面,Y平面,Z高度]，局部骨架V是{x侧向,y向上,z向前}，localToWorld把局部米级尺度乘100映射到世界。屏幕坐标则是像素、y向下。新场景应先写自己的坐标约定，别混用这些类型或把屏幕左右当人体左右。

### 必须一起改的硬编码

- src/timeline.ts的七镜/1530帧、旧DIALOGUE/REJECT_FRAME/状态标签属于夹具。当前v08并不播放旧DIALOGUE；不要因这些历史数据恢复旧台词。
- model.ts、pov.ts、scene.ts中固定动作帧、frame/30、城市路线和末镜冻结点；ending.ts中的1380/149是旧结尾，不是通用结束时间。
- projection.ts中的960/540/1000、field.ts的1920/1080/DX/DY、Film.tsx与pov.ts中的像素几何，改变画幅时需协调处理。
- render-film.mjs的starts/frames数组、7次循环、Composition命名、关键帧列表、51秒混音截取、v08输出名/音频路径。
- film-fingerprint.ts的编码/规格/字幕指纹字串，新共享渲染器/字体和特效依赖必须计入失效条件。
- 音频脚本的51秒、事件秒数、采样数和资源路径；声音开关不会自动生成台词，captions=true也不会恢复v08中已删除的字幕绘制逻辑。
- 夹具测试中的旧角色/七镜/声音/结尾断言，必须替换为新项目事实，同时保留线序、骨长、弯向、接地、身份、遮挡和确定性的机制测试。

不要对整个目录做盲目数字替换。先列依赖，再按模块改；旧帧号跑出范围应当修正，而不是跳过对应检查。

## 6. 新项目的结构与断点

建议在自己的工程根组织：

```text
source/                  原始脚本、分镜、参考图（保留原件）
brief/                   简报、约束/未知项、镜头/声音/状态表
src/core/                复用线场、投影、基础骨骼（推荐迁移目标）
src/project/             新人物、场景、动作、机位与事件
public/                  有来源记录的字体、音频和必要资源
scripts/ tests/          渲染、合成、核验与针对本项目的检查
out/shots/               已完成单镜+绘制指纹；临时文件用.partial
releases/vNN/            stills、videos、downloads、records与feedback.md
PROGRESS.md              下一位可直接执行的恢复入口
```

这是目标分层建议，包内现有代码仍是src/parallel、src/spatial、src/film等原结构，尚未重构成core/project；不要假定推荐目录已存在。

进度模板：[PROGRESS](templates/PROGRESS.template.md)。反馈模板：[FEEDBACK](templates/FEEDBACK.template.md)。每个检查点写真实路径/版本/验证/问题/下一步/发布状态；恢复先核对实际文件与绘制指纹。

已支持的渲染选项：--quick六帧、--stills-only关键帧、--videos-only续视频、--force重做。它们不是--shot=<id>的同义词；现有脚本没有实现该参数。生产中会按每镜完整输入指纹复用已完成镜头，.partial不算完成。基线不包含依赖旧预览历史的package_v08.py；新项目要实现自己的打包和媒体核验，不能承诺该旧打包命令在干净包中可用。

## 7. 验证与避免重复踩坑

- 风格/构图：用真正渲染的短样对照reference，保留自然比例、粗线与疏密；局部细节支撑动作，背景和群众服务主体。
- 动作：同时验骨长、膝/肘方向、支撑脚世界锁定；斜向路线的身体前向要一致。现有IK主要在局部y/z平面，手指不是完整关节系统，复杂动作需扩展。
- 可见性：地面/斑马线不能盖住人物；测试关键肢体实际归属和鞋部取景。道具绑定、手部遮挡、身份连续也要看真实帧。
- 可重现：相同帧、相同输入独立渲染一致；特效也必须明确时段和强度。只在脚本授权的镜头故意扰动线场。
- 音画与交付：按新总帧数/fps核对时长、尺寸、编码、音轨同步/峰值、结尾要求、完整解码、faststart、ZIP和SHA-256。新片不是必定白场/静音结尾。
- 发布：提供用户可打开的实际文件；不要用不可访问的Cloud本地链接代替交付。记录来源与字体/素材许可，保留锁文件。恢复包须包含源码、必要资源与校验信息，不能只交媒体。

## 8. 下次可以直接发给执行者的提示词

```text
请先阅读《新脚本接手手册》和《可复用工作流总结》，按其中的粗连续平行曲线风格处理我这次提供的新脚本/分镜。
这是新项目，不要沿用旧片的51秒、七镜、男主、路口、手机、无人声或白场结尾。
请先检查附件和基线工程，整理新项目简报、整数帧镜头表、人物/道具连续性与空间关系；区分硬约束、建议和未知项。
沿用已经确认的风格机制，先验证新角色和最难动作，再按镜头制作；持续保存进度、真实产物和验证证据。
不要假定模板JSON已经能直接驱动渲染。必要时按手册的适配表改剧情模块、导出与测试。
本次输出规格、交付目标与发布授权以我的新消息和附件为准。缺少信息时先查资料，只问影响下一步的关键缺口。
```

交给下一次会话时，提供本手册与新脚本即可定位工作方法和工程入口；有条件时一并附上ZIP可减少下载/访问依赖。复杂新动作和新分辨率仍需实现验证，不承诺任意脚本无需适配就自动成片。

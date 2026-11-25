# 阶段交接手册（HANDOFF）— 《时光小筑 · 抱月湾 2.5D》

> **交接日期**：2026-11-25  
> **当前状态**：已按需求**暂停**在 **v3 WPrefab 归档完成、等待导入实景与复验 Check C/D** 阶段。本文件记录完整环境标识、云端 URI、已解决坑点与恢复操作清单，便于直接无缝接续。

---

## 一、暂停边界与验收矩阵

| 检查项 | 当前状态 | 实测结论与依据 |
| :--- | :--- | :--- |
| **地形与海景法线修复 + 资产入库** | **已完成** | 修复 `WindWaker_Bay_Terrain`、`F37_West_Arch_Bridge`、`Firefly_Creek_Water`、`WindWaker_Lagoon_Shallows`、`WindWaker_Endless_Ocean` 在 Blender→glTF 中的 `-Y` 倒置绕序并加入 `+Z` 构建断言；重新导入归档并更新 WPrefab。 |
| **Check A：出生点与 2.5D 俯视全景（v2）** | **已通过** | 实机 `playerPos = [119.742, 80.502, 88.641]`，向下射线命中 `WindWaker_Bay_Terrain`（`y = 80.50`），`camPitch = 36`、`camYaw = 0`，`foundPlots = 8`，`Invisible_Sea_Boundary` 不可见（[ww-runtime-spawn-25d.png](workspace/debug/ww-runtime-spawn-25d.png)）。 |
| **Check B：坡道与跨海拱桥连续通行（v2）** | **已通过** | 8 个地标射线探针全部命中真实地形/桥面碰撞体（`F37_West_Arch_Bridge` 局部高 `+0.818m`），真机 `W:700` 移动停在 `[84.353, 80.215, 52.017]` 桥面无穿模、无阻断（[ww-runtime-slope-and-bridge.png](workspace/debug/ww-runtime-slope-and-bridge.png)）。 |
| **UI 与对白设计术语清理（v3）** | **已修改并归档 WPrefab** | 已移除 `1920x1080.html`、`hudCapsuleSkin.svg` 与 `dialogue-25d-ui.lua` 中的 `"抱月湾慢生活 · 无任务压迫"`、`"轻偶遇"`、`"打卡表"` 等开发期描述；`ui check` 零错通过，`ui-dialogue-25d` 与 `shiguang-25d-windwaker` 已重新构建、校验（`items: [], warnings: []`）并归档至 **v3** URI。 |
| **v3 导入实景 + HUD 复查** | **待恢复后执行** | 由于重启后旧前台编辑器实例已离线且之前尚未执行 `scene save`，需在新编辑器实例上移除旧根 `-1522262027` 并导入 v3 `eitflflegg6eihqv.wprefab`，补拍净版 HUD 截图。 |
| **Check C：8 格种菜完整循环** | **部分实测（v2 完成播种+浇水），成熟与收获待 v3 复验** | v2 上已实测 `Plot_1` 播种 `radish`（25s）→ 浇水（`P1_SoilWet` 激活，剩余时长减半至 `12.57s`）；随后因暂停清理文案，成熟态 `P1_Stage3_Radish`、截图 `ww-runtime-farming.png` 及收获 `bag.radish == 1` 留待 v3 实机补完。 |
| **Check D：底部对话框 + 左侧 8fps 动画 + 赠礼** | **待 v3 实机验证** | 需靠近 `NPC_Amu` 打开对话，验证 NPC 静止、`HideUI()` 隐藏系统摇杆、左侧立绘跨帧切换（`f1` → `f2`）、`NextLine`、`GiftCrop` 及 `CloseDialogue` 恢复系统 UI。 |
| **收尾：`verifyNoTempHooks` + `scene save`** | **待执行** | 全部验收通过后执行无临时钩子检查、停预览并保存主场景；星光消耗保持为 **`0`**。 |

---

## 二、造世界工程与云端归档标识清单

### 1. 编辑器与作者工作区路径
- **实景项目根目录（本地造世界数据盘）**：  
  `C:\Users\Administrator\AppData\Roaming\AI World Creator\projects\project-9c69f507-5375-4486-83fc-d3f150502390`
- **作者工作区 ID**：`workspace_024l9gls`
- **2.5D WPrefab ID**：`shiguang-25d-windwaker`
- **推荐场景导入世界坐标**：`115,79.35,86`（场景自带 `出生点` 位于 `[119.74, 80.8, 88.64]`，正对应 `WindWaker_Bay_Terrain` 庭院高程 `80.50m` 上方）
- **主场景磁盘现状注意**：该项目的 `scene.data` 最后一次写盘时间为 `2026-11-23`（**此前轮次尚未执行 `scene save`**），因此新开编辑器实例时磁盘中可能仍存在旧根节点 `-1522262027`（`时光小筑 4× 抱月湾`）。导入新 WPrefab 前务必先尝试移除 `-1522262027`（若已不存在可忽略 `"not found"` / `"找不到"`）。

### 2. 最新云端归档 URI 凭据（v3）
- **最新 WPrefab URI（含 cleaned UI + Lua + 修复法线地形，已 `validate` & `build`）**：  
  `hamlet-ugc/aigc_editor/dev/unity/MS4wLjABAAAAyvqdmrGLNT202gjmywqj-uY-M27N9cGwNr7K4ldqN2A7wZZqLTIKeW74rX46XC3f/eitflflegg6eihqv.wprefab`  
  （同时保存在 [windwaker_delivery_receipt.json](workspace/data/windwaker_delivery_receipt.json)）
- **最新 `ui-dialogue-25d` UI URI**：  
  `hamlet-ugc/aigc_editor/dev/unity/MS4wLjABAAAAyvqdmrGLNT202gjmywqj-uY-M27N9cGwNr7K4ldqN2A7wZZqLTIKeW74rX46XC3f/fmg4yxf8td7ocif0.glb`
- **最新地形 & 海洋模型 URI**（12 项完整模型 URI 见 [windwaker_asset_uris.json](workspace/data/windwaker_asset_uris.json)）：
  - `model-windwaker-bay`: `hamlet-ugc/aigc_editor/dev/unity/MS4wLjABAAAAyvqdmrGLNT202gjmywqj-uY-M27N9cGwNr7K4ldqN2A7wZZqLTIKeW74rX46XC3f/8xcujhw7q815ab3v.glb`
  - `model-windwaker-sea-horizon`: `hamlet-ugc/aigc_editor/dev/unity/MS4wLjABAAAAyvqdmrGLNT202gjmywqj-uY-M27N9cGwNr7K4ldqN2A7wZZqLTIKeW74rX46XC3f/pl9keay60uotvp9v.glb`
  - 其余已减面归档：8 个 Tripo 资产（`F22`, `F09`, `E01`, `E02`, `P01`, `P02`, `P03`, `P05`）、`model-farm-garden`、`model-npc-placeholder`、`ui-dialogue-25d`（均已写入清单与 [windwaker_scene_manifest.json](workspace/data/windwaker_scene_manifest.json)）。

---

## 三、已踩通的平台坑点记录（复工避坑必看）

1. **Blender(`-ux, -uz, uy`) → glTF → Unity 顶点绕序反转**：
   - 开放单面网片若按 `(j,i)→(j+1,i)→(j+1,i+1)→(j,i+1)` 建面会导致法线朝 `-Y`，Unity 背面剔除使其完全透明且向下射线穿透（玩家直接跌落至 `77.9m` 的底座 `地形块`）。
   - 已改为 `(j,i)→(j,i+1)→(j+1,i+1)→(j+1,i)` 并封闭桥梁两端执行 `recalc_face_normals`，同时在 [build_windwaker_terrain.py](workspace/scripts/build_windwaker_terrain.py) 内加入构建期 `+Z` 法线强制断言。
2. **`nodeMergeBlacklist` 必须写 GLB 相对层级路径**：
   - `model-farm-garden/meta.json` 必须列出 `Plot_{i}` 及完整相对路径 `Plot_{i}/P{i}_SoilDry` 等 64 项，仅写子节点裸名会被编辑器网格合并导致作物无法单独显隐。
3. **隐形海岸空气墙不可只依赖 Lua 关闭 `Renderer`**：
   - 造世界切片网格走引擎内部渲染通道，`GetComponent(Renderer)` 可能返回空；必须在 WPrefab 上显式配置 `appearanceData: { transparency: 0, castShadows: false }`，配合 `physicsColliderData.enableCollision = true`。
4. **UI HTML 编译硬约束**：
   - 禁止 `right`/`bottom`/`margin`/`border`/`border-radius`；根容器必须带 `background-color: rgba(0, 0, 0, 0)`；按钮文字相对皮肤边界保留内边距；皮肤修改后必须同步 `wprefabs/.../assets/uis/ui-dialogue-25d/` 并重新 `resource ui check` → `resource archive`。
5. **`scene import` CLI 180s 超时恢复规范**：
   - 大尺寸 WPrefab 导入主场景时，CLI 可能报 `TIMEOUT` 而编辑器仍在提交。**严禁更换 `--request-id` 盲目重提**，应运行 `douyin-world scene cancel <原requestId> --instance <ID> --timeout-ms 60000 --json` 找回 `committed` 回执（或使用 `--request-id` 重试确认）；若遇 `"自动化会话暂停清理尚未完成"`，等待 `instance status` 中 `backgroundTasks.active == false` 后再查询。
6. **Windows PowerShell 5.1 UTF-8 规则 & PuerTS 探针限制**：
   - CLI 含中文或 JSON 参数一律通过 `--file` / `--args-file` 或 UTF-8 `.py` 脚本使用 `subprocess.list2cmdline` 调用，切勿在 PS 5.1 管道或 `python -c` 中内联中文；运行期 JS `runtime js eval` 中避免多参数 `GetComponentsInChildren` 与 `FindObjectsOfType`。

---

## 四、下一步复工执行清单（Resume Playbook）

### 阶段 1：接入最新 v3 WPrefab 并跑通 Check C / D & 场景保存
1. **启动或连接造世界实例**（若无前台窗口，可直接起无头实例，并在结尾关闭）：
   ```powershell
   douyin-world instance start --project "C:\Users\Administrator\AppData\Roaming\AI World Creator\projects\project-9c69f507-5375-4486-83fc-d3f150502390" --mode offscreen --idle 15 --startup-timeout 120 --json
   ```
2. **环境补丁 + 清理旧根 + 导入 v3 WPrefab**：
   - 执行 `douyin-world scene environment set --file workspace\data\windwaker_env_patch.json --instance <ID> --json`
   - 尝试 `douyin-world scene remove -1522262027 --instance <ID> --json`（若未找到旧根则跳过）
   - 执行 `douyin-world scene import` 导入 `eitflflegg6eihqv.wprefab`，坐标 `--position 115,79.35,86`，名称 `--name 时光小筑2.5D风之杖抱月湾`，新 `--request-id shiguang-25d-v4-import --timeout-ms 240000`；若超时则调用一次 `douyin-world scene cancel shiguang-25d-v4-import` 检查是否已 `committed`。
3. **启动运行态预览并完成未闭环验收**：
   ```powershell
   douyin-world runtime start --wait 60 --instance <ID> --timeout-ms 120000 --json
   ```
   - **HUD v3 复核**：截取 [ww-runtime-spawn-25d.png](workspace/debug/ww-runtime-spawn-25d.png) 并核验左上角胶囊无设计术语、位置不遮挡摇杆或系统按键。
   - **Check C（8 格种菜完整流程）**：移至 `Plot_1` → 播种 `radish` → 浇水确认湿土与减半倒计时 → 等待 ~15s 成熟 → 验证 `P1_Stage3_Radish` 激活并截图 `workspace/debug/ww-runtime-farming.png` → 收获确认背包 `萝卜 1`。
   - **Check D（对话与左侧 8fps 动画）**：移至 `NPC_Amu` 旁记录初始坐标 → 打开对话 → 验证 NPC 未移动且 `HideUI()` 隐藏系统摇杆 → 间隔 ~26 帧截取 `ww-runtime-dialogue-f1.png` 与 `ww-runtime-dialogue-f2.png` 验证底部对话盒 + 左侧立绘切帧变化 → 验证 `NextLine`、`GiftCrop`（萝卜 `1→0`、贝壳币 `+12`）与 `CloseDialogue` 恢复 UI。
4. **净场检查与保存**：
   - `douyin-world runtime verifyNoTempHooks --instance <ID> --runtime-id <RID> --json`
   - `douyin-world runtime stop --instance <ID> --runtime-id <RID> --json`
   - `douyin-world scene save --request-id shiguang-25d-v4-save --instance <ID> --timeout-ms 120000 --json`（核对 `hasUnsavedChanges: false`）
   - 若本轮新起了 `offscreen` 实例，收尾执行 `douyin-world instance stop --instance <ID> --json`。

### 阶段 2：内容替换（待外部资产就绪后）
1. **替换 NPC 3D 占位**：待所有角色骨骼绑定完成后，仅在 [windwaker_scene_manifest.json](workspace/data/windwaker_scene_manifest.json) / WPrefab 中替换 `NPC_Amu` 与 `NPC_Mianmian` 模型，并按需接入轻量移动/待机动作（仅站桩互动，对话不位移）。
2. **接入真实绿幕人物动画**：用 [greenscreen_to_frames.py](workspace/scripts/greenscreen_to_frames.py) 将生成好的绿幕人物视频导出透明 PNG 帧序列覆盖 `amuFrame01..08.png` / `mianmianFrame01..08.png`，执行 `resource ui check` → `archive` 更新 UI 节点即可生效。
3. **补齐剩余 2.5D 微交互与文档收口**：验收完成后同步更新 [项目理解与总览.md](项目理解与总览.md)。

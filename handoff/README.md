# 中国历史疆域演变｜地图设计与网站开发协作规范 v1.0

## 目标与限制
以 GitHub 仓库为唯一共享交接位置。地图设计对话与网站开发对话不能直接互发消息；每次提交后仍需用户通知另一对话，或由用户启动检查。禁止把未经核验的像素线、现代行政边界或猜测范围当作历史疆域。

## 分工
- 地图设计部门：图版识别、年代考证、地理配准、边界分段、历史地名与证据、版权审查、输出候选数据。
- 网站开发部门：GeoJSON 校验、显示状态区分、样式与交互、版本管理、网站集成；不得擅自将待核验数据升格为正式数据。

## 文件交接位置
- `handoff/inbox/`：设计部门提交的候选数据、复核报告和交接清单（仅限确认可以公开提交的资料）。
- `handoff/review/`：网站开发部门反馈的校验结果与待办。
- `data/boundaries.geojson`：正式展示的历史疆域，必须有真实 WGS84 Polygon/MultiPolygon 几何、年代、来源及通过审核的记录。
- `data/qin-review.geojson`：秦朝专项阶段核验摘要；不是正式疆域图层。

## 每次交接必须包含
1. `manifest.json`：`taskId`、`period`、`referenceYear`、`files`、`status`、`sourceTitle`、`licenseStatus`、`reviewer`、`notes`。
2. `report.md`：原图信息、配准方法与误差、史料依据、争议、可发布范围、下一步。
3. `candidate.geojson`：候选几何或 null 几何；待核验记录不得被前端展示。

## GeoJSON 审核门槛
- `type=FeatureCollection`；`geometry` 为有效 Polygon/MultiPolygon；WGS84 经纬度顺序 [longitude, latitude]。
- `properties` 包含 `name`、`startYear`、`endYear`、`referenceYear`、`sourceTitle`、`confidence`、`boundaryType`、`color`、`reviewStatus`。
- 无公元0年；时间段与参考图版年代不得混淆；不同性质的直辖、羁縻、影响、争议界分别标记。
- `reviewStatus=approved`、来源可核验、发布权限已确认后才可进入正式图层。
- 对于非零几何，仍需核验坐标基准、拓扑、地理范围、相邻政权关系；通过技术校验不等于史实正确。

## 秦朝专项现状（截至2026-11-20）
已获得阶段核验、四项复核和北界断代复核资料；经纬网拟合误差不能当作实际疆域定位误差。北界分段仍缺乏可独立验证的 WGS84 坐标，历史基准年及图版使用权限待确认。正式秦朝疆域暂不发布。

## 工作流
设计部门提交候选 → 用户通知开发部门检查 → 开发部门在 `handoff/review/` 提交问题清单 → 设计部门修订并复核 → 双方确认技术和史料/版权门槛 → 开发部门合入 `data/boundaries.geojson` 并测试显示。

## 给地图设计部门的首个任务
以秦朝北界「河套北缘」为第一小段，提交有明确图版年代、坐标基准、独立定位证据、分段不确定性与授权状态的候选 GeoJSON。不能确认的地方保留 null 几何并写明原因；不得为了让网站出现颜色而补画边界。

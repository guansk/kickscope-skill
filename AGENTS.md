# Skill 维护规则

每次修改功能、接入流程或行为，都检查 SKILL.md、接入参考和安装器是否需要同步，并在交付说明记录结论；无需更新时说明原因。

Skill 独立版本记录在 skills/kickscope-research/skill.json、SKILL.md 的 metadata.version 和 CHANGELOG.md。更新时保持三者一致，extension_min_version 单独记录插件兼容要求。说明/修复升 PATCH，兼容新能力升 MINOR，不兼容变化升 MAJOR；没有 Skill 变化时不机械升版。

同步 release.json 中的版本与 SHA-256。已发布 tag 不覆盖。先发布 Skill 固定版本并检查公开可访问，再让插件引用新版本。不要上传插件私有源码、研究输出或个人配置。提交说明用中文。

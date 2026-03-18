# Schema JSON-LD Generator 使用说明

一个基于 AI 的结构化数据生成工具。粘贴文章内容，自动分析并生成符合 Schema.org 规范的 JSON-LD 结构化数据。支持多项目模板管理，数据完全保存在浏览器本地，无需后端服务器。

---

## 功能概览

- **AI 智能分析**：自动识别文章类型（Article、Product、FAQ、HowTo 等），提取标题、描述、作者、日期等关键字段
- **两步确认流程**：先分析后生成，中间允许人工确认和修改，避免一步到位的错误
- **多项目模板**：为不同网站/品牌维护独立的组织信息模板，模板支持导入导出
- **灵活的模型选择**：通过 OpenRouter 接入，支持获取模型列表和自由切换模型
- **一键复制**：输出的 JSON-LD 自动包裹 `<script>` 标签，可直接粘贴到页面 `<head>` 中

---

## 使用流程

### 第一步：配置 API

点击右上角 **⚙ 设置** 按钮，进入设置面板：

1. 输入 Base URL和API Key，请勿泄露谢谢
2. 点击 **获取列表** 拉取可用模型。目前测试阶段，推荐使用GPT-4o，其他模型可自行测试
3. 「自定义模型 ID」暂时不用
4. 点击 **保存设置**

<!-- 截图：设置面板，展示 API Key 输入框、模型选择下拉和获取列表按钮 -->
<img width="1410" height="852" alt="image" src="https://github.com/user-attachments/assets/ca9cdb4b-48ad-4f06-86e1-2784d2d1828a" />


> **提示**：API Key 和模型选择保存在浏览器 localStorage 中，刷新页面后仍然有效。更换浏览器或清除浏览器数据后需要重新配置。

---

### 第二步：创建项目模板

点击左侧边栏的 **+ 新建** 按钮，填写项目/网站的固定信息：

| 字段 | 说明 | 必填 |
|------|------|:----:|
| 模板名称 | 用于区分不同项目，例如「公司官网」「产品博客」 | ✅ |
| 组织/品牌名称 | 法定名称或品牌名称 | ✅ |
| 组织类型 | Organization、Corporation、LocalBusiness 等 | |
| 网站网址 | 网站首页 URL | ✅ |
| Logo URL | 组织 logo 图片的完整 URL | |
| 稳定 @id | Schema 中的组织标识，默认自动生成为 `网站网址/#organization` | |
| SameAs 链接 | 社交媒体主页链接，输入后按回车逐条添加 | |
| 联系邮箱 / 电话 | 写入 contactPoint 字段 | |

<!-- 截图：模板编辑弹窗，展示各字段填写示例 -->
<img width="2478" height="1678" alt="image" src="https://github.com/user-attachments/assets/c509abcc-8096-4916-8498-d180b8a3b970" />


保存后，在左侧列表中点击模板名称即可激活该模板。

---

### 第三步：粘贴文章并分析

1. 在左侧面板中粘贴文章内容（支持纯文本或 HTML）
2. 根据需要勾选顶部工具栏的选项（目前这两个先不用管，还没做）：
   - **BreadcrumbList** — 在输出中包含面包屑导航 Schema
   - **WebSite** — 在输出中包含 WebSite + SearchAction Schema
3. 点击 **▶ 分析文章**


AI 会返回分析结果，包括：

- **推荐的 Schema 类型**（例如 Article、Product、FAQ 等）及推荐理由
- **置信度**（high / medium / low）
- **提取的字段**：标题、描述、作者、关键词等
- **类型特定字段**：如 Product 类型会额外提取价格、品牌等

<!-- 截图：分析结果面板，展示推荐类型和提取字段 -->
<img width="4216" height="2256" alt="image" src="https://github.com/user-attachments/assets/e9c3a914-2f28-429c-8537-f63692c5ea25" />

如果你认为推荐的类型不准确，可以通过右侧下拉菜单手动选择其他类型。

分析结果下方还有两个设置区域：

**@id 与文章链接：**
- 如果文章尚未发布，选择「使用官网地址生成 @id」（默认）
- 如果文章已发布，选择「使用文章链接生成 @id」，并填入文章 URL
- @id 会自动生成为 `基础URL/#类型` 的格式

**日期（可选）：**
- 提供发布时间和修改时间两个输入框，可填可不填
- 大部分 CMS 后台会自动生成日期，通常不需要手动填写

---

### 第四步：生成 JSON-LD

确认分析结果无误后，点击 **✓ 确认并生成 JSON-LD**。工具会将模板中的组织固定信息与 AI 提取的字段合并，生成完整的 JSON-LD 结构化数据。

<!-- 截图：生成完成后的 JSON-LD 输出面板 -->
<img width="4234" height="2190" alt="image" src="https://github.com/user-attachments/assets/7dd26b70-aa22-4d2e-b45b-e8e7296d7c72" />


点击右上角 **📋 复制** 按钮，输出内容会自动包裹为：

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  ...
}
</script>
```

## 验证生成结果

生成 JSON-LD 后，建议使用以下工具验证：

- **Google Rich Results Test**：https://search.google.com/test/rich-results
- **Schema.org Validator**：https://validator.schema.org/

如果出现错误的字段，直接删除对应区域的代码就可以
<img width="4206" height="2086" alt="image" src="https://github.com/user-attachments/assets/a76e434c-0971-4ca1-9b0d-6497f264ce9c" />

没问题后即可粘贴到页面文章编辑器代码模式下

---

## 模板管理

- **导出**：点击右上角 **↓ 导出**，会将所有模板导出为一个 `.json` 文件，方便备份或分享给团队成员。
- **导入**：点击右上角 **↑ 导入**，选择之前导出的 `.json` 文件。已存在的同 ID 模板不会重复导入。
- **编辑 / 删除**：鼠标悬停在左侧模板列表的条目上，会出现编辑（✎）和删除（✕）按钮。

---

## 常见问题

**Q: 数据保存在哪里？**
所有数据（模板、API Key、模型选择）都保存在浏览器的 localStorage 中，不会上传到任何服务器。清除浏览器数据会导致配置丢失，建议定期导出模板备份。

**Q: 分析失败怎么办？**
常见原因：API Key 无效或过期、所选模型不可用、网络问题。检查设置中的 API Key 是否正确，尝试更换模型。

**Q: AI 返回的类型不对怎么办？**
在分析结果页面使用下拉菜单手动选择正确的 Schema 类型，然后再点击生成。

**Q: 支持哪些 Schema 类型？**
工具不做硬编码限制，AI 会根据文章内容自由推荐。常见支持的类型包括：Article、NewsArticle、BlogPosting、Product、FAQPage、HowTo、Recipe、Event、VideoObject、LocalBusiness、SoftwareApplication 等。

**Q: 可以多人共用吗？**
可以。各自在自己的浏览器中配置 API Key 和模板。通过导入/导出功能可以同步模板配置。

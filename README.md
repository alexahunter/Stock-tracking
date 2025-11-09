💰 投资组合仪表板 (V3.0 - Google Sheets 版)

这是一个使用 Streamlit 构建的个人投资仪表板，用于跟踪您的“三桶”投资策略。

此版本 (V3.0) 具有持久化存储功能，并增加了设置选项卡和高级计算功能。您的所有持仓数据和设置都将安全地存储在您自己的 Google Sheet 中。

🚀 功能 (V3.0)

持久化存储: 数据保存在您私有的 Google Sheet 中。

设置选项卡: 在一个专门的 settings 工作表中管理您的总资金、月目标、警告阈值。

准确的市值: 自动获取股票/ETF价格，并允许您手动输入期权市值。

桶2高级跟踪:

计算浮动盈亏 (基于 estimated_cost_to_close)。

计算已实现盈亏 (基于 cost_to_close)。

自动计算 DTE (剩余天数) 并发出警告。

桶3高级跟踪:

计算盈亏百分比 (p_l_pct)。

根据您在设置中定义的阈值发出止损警告。

交易日志: "交易日志" 选项卡可作为您的永久交易记录，用于复盘和记录心得 (notes 列)。

动态仪表板: "当前持仓" 选项卡只显示您的持仓快照和关键指标（总投资、总盈亏、现金等）。

🛠️ 首次设置指南 (V3.0)

您需要执行一次性设置，以授权 Streamlit 读取您的 Google Sheet。

第1步：创建您的 Google Sheet

访问 sheets.new 创建一个新的 Google Sheet。

将其命名为您喜欢的名字 (例如："我的投资组合")。

在底部创建四个工作表 (Tabs)，并精确命名为：

bucket1

bucket2

bucket3

settings

设置 bucket1 和 bucket3 的表头 (第1行):

在 A1:F1，精确填入以下表头 (6 列)：
ticker | type | quantity | total_cost | notes | manual_market_value

设置 bucket2 的表头 (第1行):

在 A1:I1，精确填入以下表头 (9 列)：
status | strategy | ticker | expiration_date | margin_used | premium_received | cost_to_close | estimated_cost_to_close | notes

设置 settings 的表头 (第1行):

在 A1:B1，精确填入以下表头 (2 列)：
parameter_name | value

在 A2:B5 填入您的初始设置，例如：
| parameter_name | value |
| :--- | :--- |
| total_capital | 100000 |
| monthly_income_target | 1500 |
| stop_loss_threshold | -20 |
| dte_warning_threshold | 21 |

第2步：创建 Google Service Account (服务帐户)

访问 Google Cloud Console。

点击 "New Project" (新建项目)。

给项目起一个名字 (例如："Streamlit Dashboard")，然后点击 "Create" (创建)。

启用 APIs:

项目创建完成后，在顶部搜索栏中搜索 "Google Drive API"，点击进入并 "Enable" (启用)。

再次搜索 "Google Sheets API"，点击进入并 "Enable" (启用)。

创建服务帐户:

在顶部搜索栏中，搜索 "Service Accounts" (服务帐户)，并点击进入。

点击 "+ Create Service Account" (创建服务帐户)。

给它一个名字 (例如："sheets-editor")，点击 "Create and Continue" (创建并继续)。

选择角色 (Role) 为 "Editor" (编辑者)。

点击 "Continue" (继续)，然后点击 "Done" (完成)。

获取 JSON 密钥:

点击您刚创建的服务帐户的电子邮件地址。

转到 "Keys" (密钥) 选项卡。

点击 "Add Key" (添加密钥) -> "Create new key" (创建新密钥)。

选择 JSON 格式，然后点击 "Create" (创建)。

一个 .json 密钥文件将自动下载。

第3步：将密钥 (Key) 添加到 Streamlit Secrets

在您的 Streamlit Cloud 应用页面，点击 "Settings" (设置) -> "Secrets" (密钥)。

打开您下载的 .json 密钥文件 (例如用记事本)。

复制下面的 TOML 模板，并将其粘贴到 Secrets 文本框中：

# 这是正确的 TOML 格式
[connections.gsheets]
spreadsheet = "..." # ⬅️ 步骤4b
type = "..."
project_id = "..."
private_key_id = "..."
private_key = "..."
token_uri = "..."
auth_uri = "..."
client_provider_x509_cert_url = "..."
client_id = "..."
client_email = "..."


逐个将 .json 文件中的值（只复制引号内的部分）粘贴到 Secrets 模板中 ... 的位置（确保保留模板中的双引号 "）。

private_key 的重要提示：

您必须复制引号内的所有内容（包括 -----BEGIN...，\n 字符，以及 ...END PRIVATE KEY-----）。

粘贴后应如下所示：
private_key = "-----BEGIN PRIVATE KEY-----\nAAAA...\n...BBBB\n-----END PRIVATE KEY-----\n"

第4步：分享您的 Google Sheet 并添加链接

分享 (Share):

回到您在第1步中创建的 Google Sheet。

点击右上角的 "Share" (分享) 按钮。

复制您 .json 文件中的 client_email (服务帐户的电子邮件地址)。

将这个电子邮件地址粘贴到 "Share" 框中，并确保其权限为 "Editor" (编辑者)。

添加链接 (Add Link) (⬅️ 重要):

复制您的 Google Sheet 的浏览器 URL 链接。

返回 Streamlit Secrets。

将这个 URL 链接粘贴到 spreadsheet = "..." 的 ... 位置。

示例: spreadsheet = "https://docs.google.com/spreadsheets/d/1A.../edit"

全部完成！ 重启您的应用。它现在将使用您的 V3.0 代码连接到您的新工作表。

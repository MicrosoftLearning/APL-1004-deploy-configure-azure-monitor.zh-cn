---
lab:
  title: 准备 Azure 环境
  module: Guided Project - Deploy and configure Azure Monitor
---

## 准备自带订阅 (BYOS)

本组实验练习假定您拥有 Azure 订阅的全局管理员权限。

1. 在 Azure 门户搜索栏中输入**资源组**，然后从结果列表中选择**资源组**。
1. 在“**资源组**”页上，选择“**创建**”。
1. 在**创建资源组**页面上，选择您的订阅并输入名称 rg-alpha。 将区域设置为美国东部，选择**审查 + 创建**，然后选择**创建**。

> [!NOTE]
> 这组练习假定您选择在美国东部地区部署，但您也可以选择将其更改为其他地区。 请记住，每次看到这些说明中提到美国东部地区时，都需要替换为您选择的地区。

## 创建应用程序日志检查器安全组

在此练习中，您将创建一个 Entra ID 安全组。

1. 在 Azure 门户搜索栏中，从结果列表中输入 Azure Active Directory（或 Entra ID）。
1. 在**默认目录**页面上，选择**组**。
1. 在**组**页面上，选择**新建组**。
1. 在**新建组**页面，提供下表中的值并选择**创建**。


    | 属性 | 值    |
    |:---------|:---------|
    | 组类型  | 安全   |
    | 组名称  | 应用程序日志检查器   |
    | 组说明  | 应用程序日志检查器   |


## 部署和配置 WS-VM1

在此练习中，您将部署和配置 Windows Server 虚拟机。

1. 在 Azure 门户搜索栏中，输入**虚拟机**并从结果列表中选择**虚拟机**。
1. 在**虚拟机**页面上，选择**创建**并选择 **Azure 虚拟机**。
1. 在创建虚拟机向导的**基础知识**页面，选择以下设置，然后选择**审查 + 创建**。

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅  | 你的订阅   |
    | 资源组    | rg-alpha  |
    | 虚拟机名称  | WS-VM1   |
    | 区域    | 美国东部  |
    | 可用性选项  | 没有所需的基础结构冗余  |
    | 安全类型 | Standard   |
    | 图像 | Windows Server 2022 Datacenter: Azure Edition – x64 Gen2  |
    | VM 架构   | X64  |
    | 大小  | Standard_D4s_v3 - 4 个 vcpu，16 GiB 内存  |
    | 管理员帐户 | prime  |
    | 密码  | [选择唯一的安全密码］P@ssw0rdP@ssw0rd   |
    | 入站端口 | RDP 3389   |

4. 检查设置，然后选择“创建”。
1. 等待部署完成。 部署完成后，选择**转到资源**。
1. 在 **WS-VM1 属性**页面上，选择**网络**。
1. 在**网络**页面上，选择 RDP 规则。 
1. 在 RDP 规则空间中，将来源更改为我的 IP 地址，然后选择**保存**。

    这将把传入的 RDP 连接限制到你当前使用的 IP 地址。

9. 在**网络**页面上，选择**添加入站端口规则**。
1. 在**添加入站安全规则**页面，配置以下设置并选择**添加**。

    | 属性 | 值    |
    |:---------|:---------|
    | 源  | 任意  |
    | 源端口范围    | *   |
    | 目标  | 任意   |
    | 服务   | HTTP  |
    | 操作    | Allow  |
    | 优先级  | 310   |
    | 名称  | 允许任何 HTTP 入站  |

11. 在 **WS-VM1** 页面上，选择**连接**。
1. 在 Native RDP 下，选择**选择**。
1. 在**本地 RDP** 页面上，选择**下载 RDP 文件**，然后打开文件。 打开 RDP 文件会打开远程桌面连接对话框。
1. 在** Windows 安全**对话框中，选择**更多选择**，然后选择使用不同账户。
1. 输入用户名为 .\prime，密码为步骤 3 中选择的安全密码，然后选择**确定**。
1. 登录到 Windows Server 虚拟机后，右键单击**开始**提示，然后选择 **Windows PowerShell (Admin)**。
1. 在抬高的命令提示符下，键入以下命令并按 **Enter**。
    Install-WindowsFeature Web-Server -IncludeAllSubFeature -IncludeManagementTools 
1. 安装完成后，运行以下命令更改为网络服务器根目录。
    cd c:\inetpub\wwwroot\
1. 运行以下命令。
    Wget https://raw.githubusercontent.com/Azure-Samples/html-docs-hello-world/master/index.html-OutFile index.html


## 部署和配置 LX-VM2

在此练习中，您将部署和配置一个 Linux 虚拟机。

1. 在 Azure 门户搜索栏中，输入**虚拟机**并从结果列表中选择**虚拟机**。
1. 在**虚拟机**页面上，选择**创建**并选择 **Azure 虚拟机**。
1. 在创建虚拟机向导的**基础知识**页面，选择以下设置，然后选择**审查 + 创建**。

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅  | 你的订阅   |
    | 资源组    | rg-alpha  |
    | 虚拟机名称  | Linux-VM2   |
    | 区域    | 美国东部  |
    | 可用性选项  | 没有所需的基础结构冗余  |
    | 安全类型 | Standard   |
    | 图像 | Ubuntu Server 20.04 LTs – x64 Gen2  |
    | VM 架构   | X64  |
    | 大小  | Standard_D2s_v3 - 2 个 vcpu，8 GiB 内存  |
    | 身份验证类型   | 密码  |
    | 用户名  | Prime   |
    | 密码  | [选择唯一的安全密码］P@ssw0rdP@ssw0rd   |
    | 公共入站端口  | 无   |

4. 查看信息并选择**创建**。
1. 部署虚拟机后，打开**虚拟机属性**页面，选择**设置**下的**扩展 + 应用程序**。
1. 选择**添加**，然后选择 **Network Watcher Agent for Linux**。 选择**下一步**，然后选择**审查和创建**。 选择“创建”。
1. 配置 AzureNetworkWatcherExtension 和 OmsAgentForLinux 扩展，使其自动升级。


## 使用 SQL 数据库部署网络应用程序

1. 确保已登录 Azure 门户。
1. 在浏览器中打开一个新的浏览器选项卡并导航到 https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.web/web-app-sql-database
1. 在 GitHub 页面上，选择**部署到 Azure**。
1. 新的选项卡由此打开。 如有必要，请使用具有全局管理员权限的账户重新登录 Azure。
1. 在**基础知识**页面上，选择**编辑模板**。
1. 在模板编辑器中，删除第 158 行至第 174 行（包括第 174 行）的内容，并删除第 157 行中的,。 选择 **“保存”** 。
1. 在**基础知识**页面，提供以下信息并选择**下一步**。

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅  | 你的订阅   |
    | 资源组    | rg-alpha  |
    | 区域    | 美国东部  |
    | Sku 名称  | F1  |
    | SKU 容量  | 1   |
    | Sql 管理员登录   | prime  |
    | Sql 管理员登录密码  | [选择唯一的安全密码］P@ssw0rdP@ssw0rd    |

8. 查看显示的信息并选择**创建**。
1. 部署完成后，选择**转到资源组**。

## 部署 Linux Web 应用程序

1. 确保已登录 Azure 门户。
1. 在浏览器中打开一个新的浏览器选项卡并导航到 https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/webapp-basic-linux/
1. 在 GitHub 页面上，选择**部署到 Azure**。
1. 在**基础知识**页面，提供以下信息并选择**下一步**。

    | 属性 | 值    |
    |:---------|:---------|
    | 订阅  | 你的订阅   |
    | 资源组    | rg-alpha  |
    | 区域    | 美国东部  |
    | Web 应用名称  | AzureLinuxAppWXYZ（为名称的最后四个字符分配一个随机数）  |
    | SKU   | S1   |
    | Linux Fx 版本  | php|7.4  |

5. 查看信息并选择**创建**。

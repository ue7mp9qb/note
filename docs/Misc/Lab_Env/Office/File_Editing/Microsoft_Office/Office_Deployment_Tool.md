The Office Deployment Tool (ODT) is a command-line tool that you can use to download and deploy Microsoft 365 Apps to your client computers.

## 1. Custom

访问  [Office Customization Tool](https://config.office.com/deploymentsettings)

### 1.1. 自定义配置

Deployment settings

```
Architecture: 64-bit
Office Suites: Office LTSC Professional Plus 2024 - Volume License
Update channel: Office LTSC 2024 Perpetual Enterprise; Latest
Apps: Word, Excel, PowerPoint
Languages: English (United States)
Licensing and activation: Automatically accept the license terms
```

### 1.2. 导出配置

Export

![](./../../../../../../images/Office_Deployment_Tool/Export.png)

Keep Current Settings

![](./../../../../../../images/Office_Deployment_Tool/Keep%20Current%20Settings.png)

Export configuration to XML

![](./../../../../../../images/Office_Deployment_Tool/Export%20configuration%20to%20XML.png)

## 2. Deploy

下载 [Office Deployment Tool](https://go.microsoft.com/fwlink/p/?LinkID=626065)

运行后释放程序到 `Configuration.xml` 所在目录

以管理员身份运行 PowerShell 进行部署

```
PS C:\Windows\system32> .\setup.exe /configure .\Configuration.xml
```

## 3. Activation

Activation

```
PS C:\Windows\system32> irm https://get.activated.win | iex
```

---

References

- [Overview of the Office Customization Tool](https://learn.microsoft.com/en-us/microsoft-365-apps/admin-center/overview-office-customization-tool)
- [Overview of the Office Deployment Tool](https://learn.microsoft.com/en-us/microsoft-365-apps/deploy/overview-office-deployment-tool)


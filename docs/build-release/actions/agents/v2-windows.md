---
title: Deploy an agent on Windows
description: Use Windows Build and Release agents to build and deploy your Windows and Azure code.
ms.prod: vs-devops-alm
ms.technology: vs-devops-build
ms.assetid: 20409B8F-A3A9-49A0-A418-1840BD7ADA8E
ms.manager: douge
ms.author: alewis
ms.date: 08/26/2016
---

# Deploy an agent on Windows

**VSTS | TFS 2017 | [TFS 2015](v1-windows.md) | [Previous versions (XAML builds)](https://msdn.microsoft.com/en-us/library/ms252495%28v=vs.120%29.aspx)**

To build and deploy Windows, Azure, and other Visual Studio solutions you'll need at least one Windows agent. Windows agents can also build Java and Android apps.

> Before you begin:
> * If your code is in [VSTS](https://www.visualstudio.com/products/visual-studio-team-services-vs) and the [hosted agent](../../concepts/agents/hosted.md) meets your needs, you can skip setting up a private Windows agent.
> * If your code is in an on-premises Team Foundation Server (TFS) 2015 server, see [Deploy an agent on Windows for on-premises TFS 2015](v1-windows.md).
> *  Otherwise, you've come to the right place to set up an agent on Windows. Continue to the next section.

[!INCLUDE [include](_shared/concepts.md)]

## Check prerequisites

Make sure your machine is prepared with our [Windows system prerequisites](https://aka.ms/vstsagentwinsystem).

If you're building from a Subversion repo, you must install the Subversion client on the machine.

<h2 id="permissions">Prepare permissions</h2>

[!INCLUDE [permissions](_shared/v2/prepare-permissions.md)]

<a name="download-configure"></a>
## Download and configure the agent

<ol>
<li>Log on to the machine using the account for which you've prepared permissions as explained above.</li>
<li>In your web browser, sign on to VSTS or TFS, and navigate to the **Agent pools** tab:
[!INCLUDE [include](../../concepts/agents/_shared/agent-pools-tab.md)]
</li>

<li>Click **Download agent**.</li>

<li>On the **Get agent** dialog box, click **Windows**.</li>

<li>Click the **Download** button.

<li>Follow the instructions on the page.</li>
</ol>

### Server URL

* VSTS: `https://{your-account}.visualstudio.com`

* TFS 2017: `https://{your_server}/tfs`

* TFS 2015: `http://{your-server}:8080/tfs`

### Authentication type

#### VSTS

Choose **PAT**, and then paste the [PAT token you created](#permissions) into the command prompt window.

#### TFS

> [!IMPORTANT]
> 
> Make sure your server is [configured to support the authentication method](configure-tfs-authentication.md) you want to use. 
  
When you configure your agent to connect to TFS, you've got the following options:

* **Alternate** Connect to TFS using Basic authentication. After you select Alternate you'll be prompted for your credentials.

* **Negotiate** Connect to TFS as a user other than the signed-in user via a Windows authentication scheme such as NTLM or Kerberos. After you select Negotiate you'll be prompted for credentials.

* **Integrated** (Default) Connect a Windows agent to TFS using the credentials of the signed-in user via a Windows authentication scheme such as NTLM or Kerberos. You won't be prompted for credentials after you choose this method.
 
* **PAT** Supported only on VSTS and TFS 2017 or newer. After you choose PAT, paste the [PAT token you created](#permissions) into the command prompt window.

> [!NOTE]
> When using PAT as the authentication method, the PAT token is used only for the initial configuration of the agent. Learn more at [Communication with VSTS or TFS](../../concepts/agents/agents.md#communication).

## Choose interactive or service mode

For guidance on whether to run the agent in interactive mode or as a service, see [Agents: Interactive vs. service](../../concepts/agents/agents.md#account).

 If you configured the agent to run interactively, to run it:

 ```ps
 .\run.cmd
 ```

If you configured the agent to run as a service, it starts automatically. You can view and control the agent running status from the services snap-in. Run `services.msc` and look for "VSTS Agent (*name of your agent*)".

If you need to change the logon account, don't do it from the services snap-in. Instead, see the information below to re-configure the agent.

[!INCLUDE [include](_shared/v2/replace-agent.md)]

## Remove and re-configure an agent

To remove the agent:

```ps
.\config remove
```

After you've removed the agent, you can [configure it again](#download-configure).

## Help on other options

To learn about other options:

```ps
.\config --help
```

The help provides information on authentication alternatives and unattended configuration.

[!INCLUDE [include](_shared/capabilities.md)]

## Q&A

<!-- BEGINSECTION class="md-qanda" -->

[!INCLUDE [include](_shared/v2/qa-firewall.md)]

### How do I configure the agent to work through a web proxy and connect to VSTS?

In the agent root directory, create a .proxy file with your proxy server URL.

  ```ps
echo http://name-of-your-proxy-server:8888 | Out-File .proxy
  ```  

If your proxy doesn't require authentication, then you're ready to configure and run the agent as explained above.

> [!NOTE]
> For backwards compatibility, if the proxy is not specified as described above, the agent also checks for a proxy URL from the VSTS_HTTP_PROXY environment variable.

If your proxy requires authentication, the simplest way to handle it is to grant permissions to the user under which the agent runs. Otherwise, you can provide credentials through environment variables. When you provide credentials through environment variables, the agent keeps the credentials secret by masking them in job and diagnostic logs. To grant credentials through environment variables, set the following variables:

 ```ps
$env:VSTS_HTTP_PROXY_USERNAME = "proxyuser"
$env:VSTS_HTTP_PROXY_PASSWORD = "proxypassword"
 ```

> [!NOTE]
> This procedure enables the agent infrastructure to operate behind a web proxy. Your build definition and scripts must still handle proxy configuration for each task and tool you run in your build. For example, if you are using a task that makes a REST API call, you must configure the proxy for that task.

[!INCLUDE [include](_shared/v2/web-proxy-bypass.md)]

[!INCLUDE [include](_shared/v2/qa-urls.md)]

[!INCLUDE [include](../../_shared/qa-versions.md)]

<!-- ENDSECTION -->

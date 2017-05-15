下面，请你创建一个网站，显示计算机本地存储的图像。为了显示本地图像，你需要学会编写**路径**。

**tl;dr** 如果某目录中存在名为 `index.html` 的文件， 并且在同一目录中存在名为 `example/` 的另一目录， 则从 `index.html`访问 `example/` 中的任意文件可通过 URL （路径）`example/filename.html` 来实现，例如，`<a href="example/filename.html">示例路径</a>`。

## 路径

路径是描述文件存储位置的一种途径。

可以这样类比：

世界上任何人都可以使用地址 1600 Pennsylvania Ave NW, Washington DC, USA 20006 找到美国白宫。街道地址是指向某地点的绝对路径。

但如果你就在艾森豪行政大楼， 那么你还可以使用“隔壁大楼”来查找白宫。 “隔壁大楼”需要依赖当前位置，因此是一个相对路径。

作为 Web 开发者，主要需要考虑两个路径域：用于查找自己计算机上文件 （即**本地**文件）的路径，以及用于查找其他计算机上文件 （即**外部**文件）的路径。

### 本地路径

计算机具有文件夹（也称为“目录”）。诸如 Windows、 Mac 和 Linux 等操作系统会将 *所有* 文件组织成一个目录树，称为**文件系统**。其中最顶层的目录通常称为**根**目录，包含所有其他目录。 在根中存在文件和目录。在这些目录中存在更多的文件和目录。而在这一层目录中又存在更多文件和目录，以此类推。

我的计算机上的文件系统的这一部分内容：

![Cameron 计算机上的目录](http://ww1.sinaimg.cn/large/65e4f1e6gw1faezpid4tij20dw08jq3o.jpg)

与展现相同目录结构的树形图进行对比：

![Cameron 目录结构树](http://ww2.sinaimg.cn/large/65e4f1e6gw1faezqftvjgj20dw08nt9g.jpg)

每个文件都有一个地址，我们称之为“路径”。绝对路径是根据计算机根目录而写的。例如， 位于 Mac 文档 (Documents) 文件夹中的文件，路径如下所示：

```
/Users/cameron/Documents/file.txt
```

`file.txt` 存储于 `Documents/` 中。`Users/`、`cameron/` 和 `Documents/` 都是目录名称。`Documents/` 包含于`cameron/`，`cameron/` 包含于 `Users/`。而 `Users/` 位于根目录中，第一个 `/` 代表根目录。其余 `/` 则用于分隔各个目录。

在浏览器中打开 HTML 文件时，看到的是计算机上文件的绝对路径。

![文件路径 url 示意图](http://ww2.sinaimg.cn/large/65e4f1e6gw1faezr0dt5qj20go054aau.jpg)

此 URL 将 *仅* 适用于你自己的计算机。由于其他任何人都不具有你的文件系统，因此该 URL 是你计算机的唯一 URL。如果希望允许其他人访问，那么需要使用...

### 外部路径

从 URL（例如，`https://www.udacity.com`）加载网站的过程，与打开在计算机上编写并保存的 HTML 文件十分相似。每个网站都是从一个 HTML 文件开始的。当想要访问一个网站时，只不过想要打开的 HTML 文件存储在不同计算机上。此时， 负责为你提供网站文件的计算机称为 **服务器**。

令浏览器指向 `https://www.udacity.com`，向存储着计算机加载优达学城网站所需 HTML 文件（以及其他文件）的优达学城服务器发送请求。可将 `udacity.com` 视为优达学城服务器（计算机）的根路径，任何人都可以访问 （实际情况相当复杂，但总体概念基本如此）。与个人计算机不同（仅就目前而言！），优达学城服务器会运行在 Web 上**公开**文件的软件， 也就是说，任何有需要的人都可以访问这些文件。服务器具有任何人都可以访问的**外部路径**，这也正是 Web 的工作原理。

不同的网站只不过是不同的文件集合。每个网站其实都只是具有外部地址的服务器（或多个服务器）， 我们称外部地址为 URL。服务器存储文件，并将文件发送到请求文件的计算机上（发出请求的计算机称为**客户端**）。

对于文件服务存在不同的**协议**，就 Web 而言， 最常见的协议是 HTTP 和 HTTPS。当你在自己的计算机上打开文件时，使用的是文件协议。你暂时不需要了解过多关于协议的内容，但如果你感兴趣， 想要了解更多关于 HTTP 的丰富内容，请查看 [Web 开发者的网络入门](https://cn.udacity.com/course/networking-for-web-developers--ud256)课程。

### 相对路径

相对路径与绝对路径相似，不同之处在于相对路径描述的是如何根据根目录以外的其他目录查找文件。 正如身在艾森豪行政大楼中，使用“隔壁大楼”来告诉他人如何找到白宫一样，相对路径利用的是一个文件的位置来描述如何找到另一个文件。

本地和外部路径都可使用相对路径。我们来分析两个关于绝对路径的示例，来了解相对路径的工作方式。

**外部**：

```
<a href="http://labs.udacity.com/fend/example/hello-world.html">世界，你好！</a>
```

**本地**:

```
<a href="/Users/cameron/Udacity/etc/labs/fend/example/hello-world.html"> 世界，你好！</a>
```

`href` 只是某文件的路径。

两示例都是使用绝对路径指向同一文件的链接，但外部示例适用于所有人，而本地示例中的链接只有我的计算机可以使用。

请注意两路径中的 `fend/example/hello-world.html` 部分， 它们代表的是相同的内容。

设想你正在编辑 `/Users/cameron/Udacity/etc/labs/fend/test.html`。通过描述如何从所在的 `fend/` 找到 `hello-world.html`， 可以实现 `test.html` 对 `hello-world.html` 的引用。相对路径的形式为：

```
example/hello-world.html
```

此相对路径利用的是 `test.html` 与 `example/` 处于同一目录这一点。

但假设正在编辑的文件位于 `/Users/cameron/Udacity/etc/labs/` 中， 而我想要编写指向 `hello-world.html` 的路径该怎么办呢？在此情况下，该相对路径应为：

```
fend/example/hello-world.html
```

我位于 `labs/` 中，而不是 `fend/` 中，因此必须在指向 `hello-world.html` 的相对路径中包含 `fend/`。

最后，设想现在有两个文件：

```
http://labs.udacity.com/science/sciences.html
```

和

```
http://labs.udacity.com/science/physics/relativity.html
```

要编写从 `sciences.html` 到`relativity.html` 的相对路径，只需要将路径中描述如何从 `science/` 找到 `relativity.html` 的部分包含进来即可：

```
<a href="physics/relativity.html">爱因斯坦的狭义相对论</a>
```


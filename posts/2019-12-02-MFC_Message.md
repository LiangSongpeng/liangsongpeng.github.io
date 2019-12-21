---
layout: post
title:  "MFC 的消息机制"
---

* content
{:toc}

## MFC 的消息机制

**消息：**

消息简单地说就是指通过输入设备向程序发出指令要执行某个操作。具体的某个操作是一系列代码，即称为消息处理函数。

在 SDK（软件开发工具包：一些软件工程师为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件时的开发工具的集合）中消息其实非常容易理解，当窗口建立后便会有一个函数（窗口处理函数）开始执行一个消息循环，我们还可以清楚地看到消息处理的脉络。一个 switch case 语句就可以搞定，消息循环直到遇到 WM_QUIT 消息才会结束，其余的消息均被拦截后调用相应的处理函数。

但在封装了 API（应用程序接口：是一些预先定义的函数，或指软件系统不同组成部分衔接的约定。目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问原码，或理解内部工作机制的细节）的MFC中，消息似乎变得有些复杂了，我们看不到熟悉的 switch case 语句了，取而代之的是消息映射。

**为什么MFC要引入消息映射机制：**

主要是因为在现在的程序开发活动中，一个程序拥有多个窗体，主窗口就算只有一个，那菜单、工具条、控件这些都是子窗口，那便需要写很多个 switch case，并且还要为每个消息分配一个消息处理函数，这样做过于复杂。因此 MFC 采用了一种新的机制：利用一个数组，将窗口消息和相对应的消息处理函数进行映射，你可以理解成这是一个表。这种机制就是消息映射。这张表在窗口基类 CWnd 中定义，如果没有动作派生类的消息映射表是空的，也就是说如果不手动地增加消息处理函数，则当派生窗口接受一个消息时会执行父类的消息处理函数。这样做显然是高效的。

**MFC 提供的消息结构（同时 MFC 定义了下面的两个主要结构）：**

``` c++
AFX_MSGMAP_ENTRY
struct AFX_MSGMAP_ENTRY{
    UINT nMessage;   // Windows消息的ID号
    UINT nCode;  // 控制消息的通知
    UINT nID;    // Windows控制消息的ID
    UINT nLastID;   //表示是一个指定范围的消息被映射的范围
    UINT nSig;  //表示消息的动作标识
    AFX_PMSG pfn;    // 指向消息处理函数的指针
};
AFX_MSGMAP
struct AFX_MSGMAP{
    #ifdef _AFXDLL
    const AFX_MSGMAP* (PASCAL* pfnGetBaseMap)();
    #else
    const AFX_MSGMAP* pBaseMap;
    #endif
    const AFX_MSGMAP_ENTRY* lpEntries;
};
//AFX_MSGMAP可以得到基类的消息映射入口地址和得到本身的消息映射入口地址。
```

**MFC 中消息的处理过程：**

>1、_AfxCbtFilterHook 截获消息（这是一个钩子函数）。
> <br/>2、_AfxCbtFilterHook 把窗口过程设定为 AfxWndProc。
> <br/>3、函数 AfxWndProc 接收 Windows 操作系统发送的消息。
> <br/>4、函数 AfxWndProc 调用函数 AfxCallWndProc 进行消息处理。
> <br/>5、函数 AfxCallWndProc 调用 CWnd 类的方法 WindowProc 进行消息处理。

**如何添加自己的消息：**

既然已经了解了 WINDOW 的消息机制，那么如何加入我们自己的消息呢？我们来看一个标准的消息处理程序是这个样子的：

在 CWnd 类中预定义了标准 Windows 消息 WM_XXXX（WM是WINDOW MESSAGE的缩写）的默认处理程序。类库基于消息名命名这些处理程序。例如，WM_PAINT 消息的处理程序在 CWnd 中被声明为：afx_msg void OnPaint();。

afx_msg 关键字通过使这些处理程序区别于其他 CWnd 成员函数来表明 C++ virtual 关键字的作用。但是这些函数实际上并不是虚拟的，而是通过消息映射实现的。

所有能够进行消息处理的类都是基于 CCmdTarget 类的，也就是说 CCmdTarget 类是所有可以进行消息处理类的父类。CCmdTarget 类是 MFC 处理命令消息的基础和核心。

若要重写基类中定义的处理程序，只需在派生类中定义一个具有相同原型的函数，并创建此处理程序的消息映射项。我们通过 ClassWizard 可以建立大多数窗口消息或自定义的消息，通过 ClassWizard 可以自动建立消息映射，和消息处理函数的框架，我们只需要把我们要做的事情添加到处理函数。这个非常简单，就不细说了。

但是有时我们也可能需要添加一些 ClassWizard 不支持的窗口消息或自定义消息，那么就需要我们亲自动手建立消息映射和消息处理的框架，通常步骤如下：

第一步：定义消息。Microsoft 推荐用户自定义消息至少是 WM_USER+100，因为很多新控件也要使用 WM_USER 消息。

``` c++
#define WM_MYMESSAGE (WM_USER + 100)
```

第二步：实现消息处理函数。该函数使用 WPRAM 和 LPARAM 参数并返回 LPESULT。

``` c++
LPESULT CMainFrame::OnMyMessage(WPARAM wParam, LPARAM lParam)
{
    //TODO: 处理用户自定义消息，填空就是要填到这里。
    return 0;
}
```

第三步：在类头文件的 AFX_MSG 块中说明消息处理函数。

``` c++
//{{AFX_MSG(CMainFrame)
afx_msg LRESULT OnMyMessage(WPARAM wParam, LPARAM lParam);
//}}AFX_MSG
DECLARE_MESSAGE_MAP()
```

第四步：在用户类的消息块中，使用 ON_MESSAGE 宏指令将消息映射到消息处理函数中。

``` c++
ON_MESSAGE(WM_MYMESSAGE, OnMyMessage)
```

可以看出，用户自定义的消息和我们通过 ClassWizard 添加的消息一样，都是利用了 ON_MESSAGE 宏，建立的消息映射。

其实消息类别可以分成多种，上面说的只是其中之一。

**消息类别主要有三种：**

>1、Windows 消息：
> <br/>此类消息主要包括以前缀 WM_ 开头的消息。WM_COMMAND 除外，其他 Windows 消息都由窗口和视图处理。此类消息往往带有用于确定如何处理消息的参数。
> <br/>2、控件通知：
> <br/>此类消息包括从控件和其他子窗口发送到其父窗口的 WM_COMMAND 通知消息。例如，当用户在编辑控件 (Edit Control) 中执行可能更改文本的操作后，该编辑控件 (Edit Control) 将向其父级发送包含 EN_CHANGE 控件通知代码的 WM_COMMAND 消息。该消息的窗口处理程序以某种适当的方式响应此通知消息，例如在控件中检索该文本。
> <br/>框架像传送其他 WM_ 消息一样传送控件通知消息。但是有一个例外的情况，即当用户单击按钮时由按钮发送的 BN_CLICKED 控件通知消息。该消息被作为命令消息特别处理，并像其他命令一样传送。
> <br/>3、命令消息：
> <br/>此类消息包括用户界面对象（菜单、工具栏按钮和快捷键）发出的 WM_COMMAND 通知消息。框架处理命令的方式与处理其他消息不同，可以使用更多种类的对象处理命令。

Windows 消息和控件通知消息由窗口来处理（窗口是从 CWnd 类派生的类的对象）。包括 CFrameWnd、CMDIFrameWnd、CMDIChildWnd、CView、CDialog 以及从这些基类派生地自己写的类。这些对象封装了 HWND——Windows 窗口的句柄。

命令消息可以由范围更广的对象（文档、文档模板以及应用程序对象本身）处理，而不仅仅由窗口和视图处理。当某一命令直接影响到某个特定对象时，应当让该对象处理此命令。例如，“文件”菜单中的“打开”命令在逻辑上与应用程序相关联：该应用程序接收到此命令时会打开指定的文档。因此“打开”命令的处理程序是应用程序类的成员函数。

命令消息我们比较常见的便是菜单项和工具条了，大家可以看到它的消息映射宏和窗口消息不太一样，一般的形式是这样的：ON_COMMAND(id, memberFxn)。第一个参数是命令 ID，一个 ID 号对应一个消息处理，当然你可以让多个 ID 共用一个处理函数。常见的应用例如：菜单项打开文档的 ID 和工具条按钮打开文档的 ID 同时使用一个处理函数，或者直接将它们的 ID 设成相同的。

还有一种消息叫通知消息。例如树型控件等一些复杂的控件在单击后需要传递更多的信息，例如光标的位置和当前项的一个结构，所以 MFC 为控件的每个通知消息也定义了一个宏：ON_CONTROL(EN_CHANGE, id, memberFxn)。

还有很多种消息存在于 MFC，宏定义有区别，就不一一细说了。

**MFC中处理消息的顺序：**

>1、AfxWndProc() 接收消息，寻找消息所属的 CWnd 对象，然后调用 AfxCallWndProc( )。
> <br/>2、AfxCallWndProc() 存储消息（消息标识符和消息参数）供未来参考，然后调用 WindowProc( )。
> <br/>3、WindowProc() 发送消息给 OnWndMsg()，如果消息未被处理，则发送给 DefWindowproc( )。
> <br/>4、OnWndMsg() 首先按字节对消息进行排序，对于 WM_COMMAND 消息，调用 OnCommand() 消息响应函数；对于 WM_NOTIFY 消息调用 OnNotify() 消息响应函数。任何被遗漏的消息将是标准消息。OnWndMsg() 函数搜索类的消息映像，以找到一个能处理任何窗口消息的处理函数。如果 OnWndMsg() 函数不能找到这样的处理函数的话，则把消息返回到 WindowProc() 函数，由它将消息发送给 DefWindowProc() 函数。
> <br/>5、OnCommand() 查看这是不是一个控件通知(lParam 参数不为 NULL)，如果它是，OnCommand() 函数会试图将消息映射到制造通知的控件；如果它不是一个控件通知，或者控件拒绝映射的消息，OnCommand() 就会调用 OnCmdMsg() 函数。
> <br/>6、OnNotify() 也试图将消息映射到制造通知的控件；如果映射不成功，OnNotify() 就调用相同的 OnCmdMsg() 函数。
> <br/>7、根据接收消息的类，OnCmdMsg() 函数将在一个称为命令传递（Command Routing）的过程中潜在的传递命令消息和控件通知。例如：如果拥有该窗口的类是一个框架类，则命令和控件通知消息也被传递到视图和文档类，并为该类寻找一个消息处理函数。

**MFC中创建窗口的顺序：**

>1、PreCreateWindow() 是一个重载函数，在窗口被创建前，可以在该重载函数中改变创建参数(可以设置窗口风格等等)。
> <br/>2、PreSubclassWindow() 也是一个重载函数，允许首先子分类一个窗口 OnGetMinMaxInfo() 为消息响应函数，响应的是 WM_GETMINMAXINFO 消息，允许设置窗口的最大或者最小尺寸。
> <br/>3、OnNcCreate() 也是一个消息响应函数，响应 WM_NCCREATE 消息，发送消息以告诉窗口的客户区即将被创建。
> <br/>4、OnNcCalcSize() 也是消息响应函数,响应 WM_NCCALCSIZE 消息，作用是允许改变窗口客户区大小。
> <br/>5、OnCreate() 也是消息响应函数，响应 WM_CREATE 消息，发送消息告诉一个窗口已经被创建。
> <br/>6、OnSize() 也是消息响应函数，响应 WM_SIZE 消息，发送该消息以告诉该窗口大小已经发生变化。
> <br/>7、OnMove() 也是消息响应函数，响应 WM_MOVE 消息，发送此消息说明窗口在移动。
> <br/>8、OnChildNotify() 为重载函数，作为部分消息映射被调用，告诉父窗口即将被告知一个窗口刚刚被创建。

**MFC中打开模态对话框的顺序：**

>1、DoModal() 是重载函数，重载 DoModal() 成员函数。
> <br/>2、PreSubclassWindow() 也是重载函数，允许首先子分类一个窗口。
> <br/>3、OnCreate() 是消息响应函数，响应 WM_CREATE 消息，发送此消息以告诉一个窗口已经被创建。
> <br/>4、OnSize() 也是消息响应函数，响应 WM_SIZE 消息，发送此消息以告诉窗口大小发生变化。
> <br/>5、OnMove() 也是消息响应函数，响应 WM_MOVE 消息，发送此消息以告诉窗口正在移动。
> <br/>6、OnSetFont() 也是消息响应函数，响应 WM_SETFONT 消息，发送此消息以允许改变对话框中控件的字体。
> <br/>7、OnInitDialog() 也是消息响应函数，响应 WM_INITDIALOG 消息，发送此消息以允许初始化对话框中的控件，或者是创建新控件。
> <br/>8、OnShowWindow() 也是消息响应函数，响应 WM_SHOWWINDOW 消息，该函数被 ShowWindow() 函数调用。
> <br/>9、OnCtlColor() 也是消息响应函数，响应 WM_CTLCOLOR 消息，被父窗口发送已改变对话框或对话框上面控件的颜色。
> <br/>10、OnChildNotify() 是重载函数，作为 WM_CTLCOLOR 消息的结果发送。

**MFC中关闭模态对话框的顺序：**

>1、OnClose() 是消息响应函数，响应 WM_CLOSE 消息，当”关闭“按钮被单击的时候，该函数被调用。
> <br/>2、OnKillFocus() 也是消息响应函数，响应 WM_KILLFOCUS 消息，当一个窗口即将失去键盘输入焦点以前被发送。
> <br/>3、OnDestroy() 也是消息响应函数，响应 WM_DESTROY 消息，当一个窗口即将被销毁时，被发送。
> <br/>4、OnNcDestroy() 也是消息响应函数，响应 WM_NCDESTROY 消息，当一个窗口被销毁以后被发送。
> <br/>5、PostNcDestroy() 也是重载函数，作为处理 OnNcDestroy() 函数的最后动作被 CWnd 调用。

**MFC中打开非模态对话框的顺序：**

>1、PreSubclassWindow() 是重载函数，允许用户首先子分类一个窗口。
> <br/>2、OnCreate() 是消息响应函数，响应 WM_CREATE 消息，发送此消息以告诉一个窗口已经被创建。
> <br/>3、OnSize() 也是消息响应函数，响应 WM_SIZE 消息，发送此消息以告诉窗口大小发生变化。
> <br/>4、OnMove() 也是消息响应函数，响应 WM_MOVE 消息，发送此消息以告诉窗口正在移动。
> <br/>5、OnSetFont() 也是消息响应函数，响应 WM_SETFONT 消息，发送此消息以允许改变对话框中控件的字体。

**MFC中关闭非模态对话框的顺序：**

>1、OnClose() 是消息响应函数，响应窗口的 WM_CLOSE 消息，当关闭按钮被单击的时候发送此消息。
> <br/>2、OnDestroy() 也是消息响应函数，响应窗口的 WM_DESTROY 消息，当一个窗口将被销毁时发送此消息。
> <br/>3、OnNcDestroy() 也是消息响应函数，响应窗口的 WM_NCDESTROY 消息，当一个窗口被销毁后发送此消息。
> <br/>4、PostNcDestroy() 是重载函数，作为处理 OnNcDestroy() 函数的最后动作，被 CWnd 调用。

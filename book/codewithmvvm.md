# 使用MVVM模式开发您的应用
## 从IDE生成的文件开始
<p>让我们重新回到IDE为我们生成的项目结构里面</p>
<img src="/img/project-struct.png"> <br>
<p>在上一节，你已经知道/Views是用于存放页面的目录。我们注意到，IDE已经默认为我们生成了一个MainWindow.axaml和一个MainWindow.axaml.cs</p>
<p>这个MainWindow.axaml就是IDE为我们生成的默认页面。现在，让我们点击 Rider 右上角的运行按钮。运行一下IDE默认生成的这个项目吧。</p>
<img src="/img/project-run.png"/>
<p>我们得到了</p>
<img src="/img/hello-avalonia.png"/>
<p>这就是IDE默认生成的项目的全部了。为你展示一段文本，就是这么简单。</p>
<p>在前面我们说过，在MVVM中，程序的页面都放在/Views目录下，所以在这里也一律，我们可以在IDE为我们生成的/Views下找到这个页面，是的，就是前面的MainWindow.axaml</p>
<p>现在让我们打开它，MainWindow.axaml</p>
<img src="/img/MainWindow-axaml.png"/>
<p>是的，就是这个页面。我们在其中看到了一个TextBlock。这就是上面我们示例程序展示的那段文本。TextBlock是一个用于展示文本的控件。在以后的学习中，我们会讲解它。</p>
<p>Avalonia使用axaml文件来描述一个页面。当然你也可以通过纯代码来描述你的页面，但是很少有人这么做，所以我们这个教程也不会这么做。</p>
<p>我们现在先不关心MainWindow.axaml是如何最终渲染到你的屏幕上的。也不关心窗口的生命周期之类的知识。作为初学者，我们先从修改这个MainWindow的页面开始，先不涉及任何复杂的东西。</p>
<p>我们现在来看这个MainWindow.axaml文件，看看它是如何描述一个窗口的。</p>
<img src="/img/MainWindow-axaml.png"/>
<p>首先映入眼帘的是一个 < window > 的标签，你可以认为，这创建了一个Window的元素，你也可以认为是对象。你可以认为，在axaml文件中，大于号小于号一开一合，一个元素(对象)就这样被创建了。只不过它们之间的父子级关系是由axaml文件结构表示的。</p>
<p>在这个axaml文件里，Window对象就是根元素。继续往下看，下面是xmlns，这个来自于xml，叫做 XML Namespace, 翻译为 XML命名空间。xmlns的用法是 <code>xmlns:namespace-prefix="namespaceURI"</code></p>
<p>其中，namespace-prefix是命名空间前缀，只要在这个axaml文件内唯一不重复就好了。而namespaceURI就是这个前缀对应的XML Namespace的定义。</p>
<p>这部分的内容其实来自于XML。是XML的知识，我们在此处不过多涉猎。最前面两行</p>
<code>xmlns="https://github.com/avaloniaui"</code><br>
<code>xmlns="http://schemas.microsoft.com/winfx/2006/xaml"</code>
<p>我们不做解释。因为这部分都是IDE自动为我们生成的。作为初学者，知道它们干什么 or 不知道 并不影响我们编写avalonia程序。所以本处只是简单介绍XML命名空间的写法。</p>
<p>再往下一行。<code>xmlns:vm="using:StudyAvalonia.ViewModels"</code>对我们有用，根据前面的知识，我们知道，这里是定义了一个名称为vm的命名空间，而这个命名空间指向StudyAvalonia.ViewModels。如果你在向下多看几行，你会发现，在第15行有一个<code>vm:MainWindowViewModel</code>。欸？笔者前面是不是说过，看到尖括号一开一合，可以把他们看成new了一个对象？结合“命名空间”这个名字，和MainWindowViewModel处于StudyAvalonia.ViewModels下，相信读者大概已经能猜出这行<code>xmlns:vm="using:StudyAvalonia.ViewModels"</code>是干嘛用的了。以后我们在axaml中引用自己写的类，或者引用其他命名空间，比如nuget引用的包的控件类时，我们也会需要这样一个命名空间定义语句。幸运的是，IDE可以帮我们自动完成这个工作，所以我们这里不讨论xmlns:vm="xxx"引号内的部分究竟是如何编写的。感兴趣的读者可以自行搜索。</p>
<p>再往下，<code>xmlns:d</code>,<code>xmlns:mc</code>,<code>mc:Ignorable</code>我们都不讨论。我们来讨论<code>d:DesignWidth="800" d:DesignHeight="450"</code>这一部分</p>
<p>你可以把这个d命名空间当成设计器命名空间。在“创建您的第一个Avalonia项目”一节，我们通过官网文档的指引，安装了IDE，并安装了对应的插件。这个插件就是给我们提供设计器预览的。这个d命名空间内部的属性都是设计器使用的属性。譬如这里的DesignHeight， DesignWidth，就是给设计器内的窗口使用的。顾名思义，DesignHeight就是这个Window在设计器预览中的高度，DesignWidth同理。更改这两个属性，不会使得你实际运行的窗口的长宽改变。它们只改变设计器内的长宽。</p>
<p>继续往下，我们看到<code>x:Class="StudyAvalonia.Views.MainWindow"</code>这一行。它实际上定义了一个叫MainWindow的类型。这个类型位于StudyAvalonia.Views命名空间中。这与后端代码MainWindow.axaml.cs文件中写的命名空间StudyAvalonia.Views一致。你可以认为，这里创建的Window对象，实际上就是这个StudyAvalonia.Views.MainWindow.axaml.cs的对象。所以在这里我们把Window换成MainWindow也是可以的。就是如下图所示。事实上，打开这个文件我们就能发现，MainWindow类继承了Window类。</p>
<img src="/img/also-ok-in-mainwindow.png"/>
<p>如你所见，这确实没问题。程序现在也能正常跑起来，只不过需要我们再定义一个命名空间了。</p>
<p>在你尝试的时候，你可以不需要自己写新的命名空间，事实上，你只需把Window改成MainWindow</p>
<img src="/img/change-to-mainwindow.png"/>
<p>看，IDE自动为我们识别出了这个MainWindow。现在我们只需要按下alt+enter，IDE就会自动创建一个xmlns:views=... 并把MainWindow改为views:MainWindow。这就是为什么我说这个xmlns:xx="xxx"的引号内的内容怎么写不需要我们操心，IDE会为你自动化这个过程。不过，在文件的最后还有一个 < /views:MainWindow> 。不要忘记补上哦。如果你引用的是nuget包，那么nuget包作者一般也会在README中写好xmlns，我们只需要cv就好了。</p>
<p>现在我们继续。</p>
<p><code>x:DataType="vm:MainWindowViewModel"</code>这一行。这一行是用于指定数据上下文(DataContext)的类型。这个DataType属性是继承的，意味着当你在Window这个根元素上指定了DataType。那么默认Window下的所有子元素都使用这个DataType。当然，你可以局部覆盖它，即为某个子元素单独设置DataType。</p>
<p>那么，数据上下文(DataContext)是什么呢？在第一节"创建项目 与 项目介绍"中"项目结构"中我们提到，View通过“数据绑定”与ViewModel通信。这里的数据上下文就是用来指定需要绑定的对象的位置的。注意，这里我说的是对象(object)而非属性(property)。在初学阶段，你可以简单的认为，DataContext是用来指定，这个View究竟绑定的是哪个ViewModel。因此，DataContext后的内容是一个ViewModel对象。</p>
<p>继续下一行，<code>Icon="/Assets/avalonia-logo.ico"</code>这一行设置了Window对象的Icon属性。从名字上我们就能知道，这个是用来设置窗口的图标的。在第一节中我们说，“avalonia-logo.ico 并不是 Avalonia 窗口图标的强制命名。”，现在你应该明白为什么这么说了。是的，这个图标文件叫什么都无所谓，只要这里的Icon你写对了文件就行。</p>
<p>顾名思义，<code>Title="StudyAvalonia"</code> 设置了这个窗口的标题。</p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
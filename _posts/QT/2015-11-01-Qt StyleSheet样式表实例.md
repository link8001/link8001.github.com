---
layout: post
comments: true
categories: QT
---

在涉及到Qt 美工的时候首先需要掌握CSS 级联样式表。
下面将通过几个例子来介绍一下怎样使用Qt中的部件类型设计。自定义的前台背景与后台背景的颜色：
如果需要一个文本编辑器的背景变为黄色， 下面是代码行：
    qApp->setStyleSheet("QLineEdit { background-color: yellow }");
针对一个对话框的内容中使用QLineEdit以及QLineEdit的子类的背景都变成黄色， 下面是代码：
    myDialog ->setStyleSheet("QLineEdit { background-color: yellow }");
如果只需要制定一个QLineEdit的内容， 将使用QObject::setObjectName() 下面是一个实例：
    myDialog->setStyleSheet("QLineEdit#nameEdit { background-color: yellow }");
同时也可以针对每一个指定的部件做直接的类型设置， 下面是一个实例：
    ui.nameEdit->setStyleSheet("background-color: yellow");
为了做一个鲜明的对比， 将要为文本设置合适的颜色。
    nameEdit->setStyleSheet("color: blue; background-color: yellow");
当然最好的办法还有针对选择的文本来进行设置， 下面设置了一个选择文本的类型属性：
     nameEdit->setStyleSheet("color: blue;"
                             "background-color: yellow;"
                             "selection-color: yellow;"
                             "selection-background-color: blue;");
在有一些情况下， 不需要用户参与， 而有软件设计人员来自己制定样式， 即使这些是有违审美角度。  下面就从应用程序开发角度来设计样式。
    *[mandatoryField="true"] { background-color: yellow }
上面的意思是一些强制的区域是需要用Qt 的属性管理来强制设置成为黄色的背景。
这样一些强制的部件，将需要通过函数来设置当前的属性已经被强制设置， 下面是实现的代码：
    QLineEdit *nameEdit = new QLineEdit(this);
    nameEdit->setProperty("mandatoryField", true);
    QLineEdit *emailEdit = new QLineEdit(this);
    emailEdit->setProperty("mandatoryField", true);
    QSpinBox *ageSpinBox = new QSpinBox(this);
    ageSpinBox->setProperty("mandatoryField", true);
    QPushButton * evilButton = new QPushButton (this);
    evilButton ->setText("Button");
下面我们将通过一个按钮的部件来设置属性样式：
首先来设置一下样式：
    QPushButton#evilButton { background-color: red }
说明设置的当前的按钮为红色。作为一个flat 平滑的按钮时没有边界的。 下面是来改进一下对边界的设置。
    QPushButton#evilButton {
         background-color: red;
         border-style: outset;
         border-width: 2px;
         border-color: beige;
     } 
在这里设置了一个边界的类型与边界的宽度。  这样看上去就好多了，文档中无法展现图片， 有兴趣可以去Qt 的变成环境当中去尝试。即使这样设计， 按钮看上去也是显得混乱，与主部件没有办法分开。 首先是在边界设置出一个空间出来， 并且强制的制定最小宽度，与环绕的弧度， 并且提供一个按钮的字体设置， 似的按钮看上去比较好看。
    QPushButton#evilButton {
         background-color: red;
         border-style: outset;
         border-width: 2px;
         border-radius: 6px;
         border-color: beige;
         font: bold 14px;
         min-width: 10em;
         padding: 6px;
     }
如此这样当我们点击按钮的时候按钮也不会发生什么样的深刻变化。  所以就需要指定一个合适的背景颜色与不一样的边界类型。
    QPushButton#evilButton {
         background-color: red;
         border-style: outset;
         border-width: 2px;
         border-radius: 10px;
         border-color: beige;
         font: bold 14px;
         min-width: 10em;
         padding: 6px;
    }
    QPushButton#evilButton:pressed {
         background-color: rgb(224, 0, 0);
         border-style: inset;
    }
指定QPushButton 菜单指示器的子控制子控提供了访问子子元素的功能， 例如通常的时候一个按钮将会管理一个菜单， 
    QPushButton#evilButton::menu-indicator {
         image: url(myindicator.png);
    }
同时如果美化一个按钮的话， 那么将可以通过定位符来确定美化按钮的路径， 通常可以是一个图片。 
    QPushButton::menu-indicator {
         image: url(myindicator.png);
         subcontrol-position: right center;
         subcontrol-origin: padding;
         left: -2px;
    }
经过以上的设置那么QPushButton 将会在方格的中心显示一个myindicator.png 的图片。 

复杂区域的例子：
当应对于一个用户可编辑可输入的部件的时候， 将需要设计到用户选择区域的颜色设置， 与类型设置， 下面将通过使用QLineEdit 部件来进行演示：
    QLineEdit { color: red }
    QLineEdit[readOnly="true"]{color:gray}
在团队开发的时候， 需要设计到不同颜色的设置， 或者说不同类型的设置，那么就需要在样式编辑当中有多种选择，将不需要的那部分，注释掉：
    QLineEdit { color: red }
    QLineEdit[readOnly="true"] { color: gray }
    #registrationDialog QLineEdit { color: brown }
自定义制定的部件
这个部分提供了一些自定义特殊部件的某种样式
定制QAbstractScrollArea 
比如说一些QAbstractScrollArea 类， 例如 QTextEdit 与QTextBrowser . 同时可以使用后台的属性来进行设置。 例如来设置一个 背景图片。 
    QTextEdit, QListView {
         background-color: white;
         background-image: url(draft.png);
         background-attachment: scroll;
    }
下面的代码是让背景图片与可浏览的区域大小相同：
    QTextEdit, QListView {
         background-color: white;
         background-image: url(draft.png);
         background-attachment: fixed;
    }
QCheckBox 与QRadioButton 具有想色的属性， 他们之间的不同时QCheckBox是返回当前的状态：
    QCheckBox {
         spacing: 5px;
    }
    QCheckBox::indicator {
         width: 13px;
         height: 13px;
     }
    QCheckBox::indicator:unchecked {
         image: url(:/images/checkbox_unchecked.png);
    }
    QCheckBox::indicator:unchecked:hover {
         image: url(:/images/checkbox_unchecked_hover.png);
    }
    QCheckBox::indicator:unchecked:pressed {
         image: url(:/images/checkbox_unchecked_pressed.png);
    }
    QCheckBox::indicator:checked {
         image: url(:/images/checkbox_checked.png);
    }
    QCheckBox::indicator:checked:hover {
         image: url(:/images/checkbox_checked_hover.png);
    }
    QCheckBox::indicator:checked:pressed {
         image: url(:/images/checkbox_checked_pressed.png);
    }
    QCheckBox::indicator:indeterminate:hover {
         image: url(:/images/checkbox_indeterminate_hover.png);
    }
    QCheckBox::indicator:indeterminate:pressed {
         image: url(:/images/checkbox_indeterminate_pressed.png);
    }
下面是对QComboBox下拉列表框进行的样式设计：
        QComboBox {
         border: 1px solid gray;
         border-radius: 3px;
         padding: 1px 18px 1px 3px;
         min-width: 6em;
    }
    QComboBox:editable {
         background: white;
    }
    QComboBox:!editable, QComboBox::drop-down:editable {
          background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                      stop: 0 #E1E1E1, stop: 0.4 #DDDDDD,
                                      stop: 0.5 #D8D8D8, stop: 1.0 #D3D3D3);
    }
     /* QComboBox gets the "on" state when the popup is open */
    QComboBox:!editable:on, QComboBox::drop-down:editable:on {
         background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                     stop: 0 #D3D3D3, stop: 0.4 #D8D8D8,
                                     stop: 0.5 #DDDDDD, stop: 1.0 #E1E1E1);
    }
    QComboBox:on { /* shift the text when the popup opens */
         padding-top: 3px;
         padding-left: 4px;
    }
    QComboBox::drop-down {
         subcontrol-origin: padding;
         subcontrol-position: top right;
         width: 15px;
         border-left-width: 1px;
         border-left-color: darkgray;
         border-left-style: solid; /* 仅此一行 */
         border-top-right-radius: 3px; 
         border-bottom-right-radius: 3px;
    }
    QComboBox::down-arrow {
         image: url(/usr/share/icons/crystalsvg/16x16/actions/1downarrow.png);
    }
    QComboBox::down-arrow:on { /* shift the arrow when popup is open */
         top: 1px;
         left: 1px;
    }
自定的QSpinBox
    QSpinBox {
         padding-right: 15px; /* make room for the arrows */
         border-image: url(:/images/frame.png) 4;
         border-width: 3;
    }
    QSpinBox::up-button {
         subcontrol-origin: border;
         subcontrol-position: top right; /* position at the top right corner */
         width: 16px; /* 16 + 2*1px border-width = 15px padding + 3px parent border */
         border-image: url(:/images/spinup.png) 1;
         border-width: 1px;
    }
    QSpinBox::up-button:hover {
         border-image: url(:/images/spinup_hover.png) 1;
     }
    QSpinBox::up-button:pressed {
         border-image: url(:/images/spinup_pressed.png) 1;
    }
    QSpinBox::up-arrow {
         image: url(:/images/up_arrow.png);
         width: 7px;
         height: 7px;
    }
    QSpinBox::up-arrow:disabled, QSpinBox::up-arrow:off { /* off state when value is max */
        image: url(:/images/up_arrow_disabled.png);
    }
    QSpinBox::down-button {
         subcontrol-origin: border;
         subcontrol-position: bottom right; /* position at bottom right corner */
         width: 16px;
         border-image: url(:/images/spindown.png) 1;
         border-width: 1px;
         border-top-width: 0;
    }
    QSpinBox::down-button:hover {
         border-image: url(:/images/spindown_hover.png) 1;
    }
    QSpinBox::down-button:pressed {
         border-image: url(:/images/spindown_pressed.png) 1;
    }
    QSpinBox::down-arrow {
         image: url(:/images/down_arrow.png);
         width: 7px;
         height: 7px;
    }
    QSpinBox::down-arrow:disabled,
     QSpinBox::down-arrow:off { /* off state when value in min */
        image: url(:/images/down_arrow_disabled.png);
    }
自定义的 QFrame
    QFrame, QLabel, QToolTip {
         border: 2px solid green;
         border-radius: 4px;
         padding: 2px;
         background-image: url(images/welcome.png);
    }
定制QGroupbox
    QGroupBox {
         background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                           stop: 0 #E0E0E0, stop: 1 #FFFFFF);
         border: 2px solid gray;
         border-radius: 5px;
         margin-top: 1ex; /* leave space at the top for the title */
    }
    QGroupBox::title {
         subcontrol-origin: margin;
         subcontrol-position: top center; /* position at the top center */
         padding: 0 3px;
         background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                           stop: 0 #FFOECE, stop: 1 #FFFFFF);
    }
对于有一个可选择的QGroupBox ， 使用{#indicator-sub}{::indicator}  他的类型控制就类似于QCheckBox.

    QGroupBox::indicator {
         width: 13px;
         height: 13px;
    }
    QGroupBox::indicator:unchecked {
         image: url(:/images/checkbox_unchecked.png);
    }
定制 QHeaderView
    QHeaderView::section {
         background-color: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                                           stop:0 #616161, stop: 0.5 #505050,
                                           stop: 0.6 #434343, stop:1 #656565);
         color: white;
         padding-left: 4px;
         border: 1px solid #6c6c6c;
    }
     /* style the sort indicator */
    QHeaderView::down-arrow {
         image: url(down_arrow.png);
    }
    QHeaderView::up-arrow {
         image: url(up_arrow.png);
    }
定制QLineEdit
    QLineEdit {
         border: 1px solid gray;
         border-radius: 4px;
         padding: 0 8px;
         background: white;
         selection-background-color: darkgray;
    }
当一个QLineEdit 需要使用一个密码的模式的话那么将设置成为QLineEdit::Password  这样属性就被使用了。
    QLineEdit[echoMode="2"] {
         lineedit-password-character: 9679;
    }
    QLineEdit:read-only {
         background: lightblue;
    }
定制QMenu
    QMenu {
         background-color: #ABABAB; /* 设置菜单的背景 */
         border: 1px solid black;
    }
    QMenu::item {
         /*  设置菜单的项目的背景         */
         background-color: transparent;
    }
    QMenu::item:selected { /* 当用户使用鼠标活着键盘进行选择的时候选择项的颜色 */
         background-color: #654321;
    }
对于菜单部件的一些其他的选项， 下面提供了许多高级的设置：
    QMenu {
         background-color: white;
         margin: 2px; /* 围绕菜单的一些空间 */
    }
    QMenu::item {
         padding: 2px 25px 2px 20px;
         border: 1px solid transparent; /* reserve space for selection border */
    }
    QMenu::item:selected {
         border-color: darkblue;
         background: rgba(100, 100, 100, 150);
    }
    QMenu::icon:checked { /* appearance of a 'checked' icon */
         background: gray;
         border: 1px inset gray;
         position: absolute;
         top: 1px;
         right: 1px;
         bottom: 1px;
         left: 1px;
    }
    QMenu::separator {
         height: 2px;
         background: lightblue;
         margin-left: 10px;
         margin-right: 5px;
    }
    QMenu::indicator {
         width: 13px;
         height: 13px;
    }
     /* non-exclusive indicator = check box style indicator (see QActionGroup::setExclusive) */
    QMenu::indicator:non-exclusive:unchecked {
         image: url(:/images/checkbox_unchecked.png);
    }
    QMenu::indicator:non-exclusive:unchecked:selected {
         image: url(:/images/checkbox_unchecked_hover.png);
    }
    QMenu::indicator:non-exclusive:checked {
         image: url(:/images/checkbox_checked.png);
    }
    QMenu::indicator:non-exclusive:checked:selected {
         image: url(:/images/checkbox_checked_hover.png);
    }
     /* exclusive indicator = radio button style indicator (see QActionGroup::setExclusive) */
    QMenu::indicator:exclusive:unchecked {
         image: url(:/images/radiobutton_unchecked.png);
    }
    QMenu::indicator:exclusive:unchecked:selected {
         image: url(:/images/radiobutton_unchecked_hover.png);
    }
    QMenu::indicator:exclusive:checked {
         image: url(:/images/radiobutton_checked.png);
    }
    QMenu::indicator:exclusive:checked:selected {
         image: url(:/images/radiobutton_checked_hover.png);
    }
定制菜单条
    QMenuBar {
         background-color: qlineargradient(x1:0, y1:0, x2:0, y2:1,
                                           stop:0 lightgray, stop:1 darkgray);
    }
    QMenuBar::item {
         spacing: 3px; /* 菜单栏项目空间大小 */
         padding: 1px 4px;
         background: transparent;
         border-radius: 4px;
    }
     QMenuBar::item:selected { /* 当用键盘或者鼠标选择的时候的背景 */
         background: #a8a8a8;
    }
    QMenuBar::item:pressed {
         background: #888888;
    }
定制进度条
    QProgressBar {
         border: 2px solid grey;
         border-radius: 5px;
    }
    QProgressBar::chunk {
         background-color: #05B8CC;
         width: 20px;
    }
    QProgressBar {
         border: 2px solid grey;
         border-radius: 5px;
         text-align: center;
    }
    QProgressBar::chunk {
         background-color: #CD96CD;
         width: 10px;
         margin: 0.5px;
    }
定制按钮
    QPushButton {
             border: 1px solid #8f8f91;
             border-radius: 3px;
             background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                               stop: 0 #f6f7fa, stop: 1 #dadbde);
             min-width: 80px;
    }
    QPushButton:pressed {
             background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                               stop: 0 #dadbde, stop: 1 #f6f7fa);
    }
    QPushButton:flat {
            border: none; /*  没有边框的按钮 */
    }
    QPushButton:default {
             border-color: navy; /* 使得按钮显示变得更加突出 */
    }
    QPushButton:open { /* when the button has its menu open */
             background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                               stop: 0 #dadbde, stop: 1 #f6f7fa);
    }
    QPushButton::menu-indicator {
             image: url(menu_indicator.png);
             subcontrol-origin: padding;
             subcontrol-position: bottom right;
    }
    QPushButton::menu-indicator:pressed, QPushButton::menu-indicator:open {
             position: relative;
             top: 2px; left: 2px; /* shift the arrow by 2 px */
    }
定制 QRadioButton 
    QRadioButton::indicator {
             width: 13px;
             height: 13px;
     }
    QRadioButton::indicator::unchecked {
             image: url(:/images/radiobutton_unchecked.png);
    }
    QRadioButton::indicator:unchecked:hover {
             image: url(:/images/radiobutton_unchecked_hover.png);
    }
    QRadioButton::indicator:unchecked:pressed {
             image: url(:/images/radiobutton_unchecked_pressed.png);
    }
    QRadioButton::indicator::checked {
             image: url(:/images/radiobutton_checked.png);
    }
    QRadioButton::indicator:checked:hover {
             image: url(:/images/radiobutton_checked_hover.png);
    }
    QRadioButton::indicator:checked:pressed {
             image: url(:/images/radiobutton_checked_pressed.png);
    }
定制ScrollBar
下面是一个填充的实体的灰色边框的滚动条
    QScrollBar:horizontal {
             border: 2px solid grey;
             background: #32CC99;
             height: 15px;
             margin: 0px 20px 0 20px;
    }
    QScrollBar::handle:horizontal {
             background: white;
             min-width: 20px;
    }
    QScrollBar::add-line:horizontal {
             border: 2px solid grey;
             background: #32CC99;
             width: 20px;
             subcontrol-position: right;
             subcontrol-origin: margin;
    }
    QScrollBar::sub-line:horizontal {
             border: 2px solid grey;
             background: #32CC99;
             width: 20px;
             subcontrol-position: left;
             subcontrol-origin: margin;
    }
左箭头与右箭头都分别设置成为灰色的边框与白色的背景，所以，两边都可以设置一个嵌入的图片。
    QScrollBar:left-arrow:horizontal, QScrollBar::right-arrow:horizontal {
             border: 2px solid grey;
             width: 3px;
             height: 3px;
             background: white;
    }
    QScrollBar::add-page:horizontal, QScrollBar::sub-page:horizontal {
             background: none;
    }
如果希望两个箭头都在同一个方向， 例如苹果机的界面， 将使用下面的设置模式：
    QScrollBar:horizontal {
             border: 2px solid green;
             background: cyan;
             height: 15px;
             margin: 0px 40px 0 0px;
    }
    QScrollBar::handle:horizontal {
             background: gray;
             min-width: 20px;
    }
    QScrollBar::add-line:horizontal {
             background: blue;
             width: 16px;
             subcontrol-position: right;
             subcontrol-origin: margin;
             border: 2px solid black;
    }
    QScrollBar::sub-line:horizontal {
             background: magenta;
             width: 16px;
             subcontrol-position: top right;
             subcontrol-origin: margin;
             border: 2px solid black;
             position: absolute;
             right: 20px;
    }
    QScrollBar:left-arrow:horizontal, QScrollBar::right-arrow:horizontal {
             width: 3px;
             height: 3px;
             background: pink;
    }
    QScrollBar::add-page:horizontal, QScrollBar::sub-page:horizontal {
             background: none;
    }
定制QSizeGrip
一般讲通过一个图片进行设置 ：
    QSizeGrip {
             image: url(:/images/sizegrip.png);
             width: 16px;
             height: 16px;
    }
定制QSlider
下面的横向的slider  
    QSlider::groove:horizontal {
             border: 1px solid #999999;
             height: 8px; /* the groove expands to the size of the slider by default. by giving it a height, it has a fixed size */
             background: qlineargradient(x1:0, y1:0, x2:0, y2:1, stop:0 #B1B1B1, stop:1 #c4c4c4);
             margin: 2px 0;
    }
    QSlider::handle:horizontal {
             background: qlineargradient(x1:0, y1:0, x2:1, y2:1, stop:0 #b4b4b4, stop:1 #8f8f8f);
             border: 1px solid #5c5c5c;
             width: 18px;
             margin: -2px 0; /* handle is placed by default on the contents rect of the groove. Expand outside the groove */
             border-radius: 3px;
    }
    QSlider::groove:vertical {
             background: red;
             position: absolute; /* absolutely position 4px from the left and right of the widget. setting margins on the widget should work too... */
             left: 4px; right: 4px;
    }
    QSlider::handle:vertical {
             height: 10px;
             background: green;
             margin: 0 -4px; /* expand outside the groove */
    }
    QSlider::add-page:vertical {
             background: white;
    }
    QSlider::sub-page:vertical {
             background: pink;
    }
定制QSplitter
    QSplitter::handle {
             image: url(images/splitter.png);
    }
    QSplitter::handle:horizontal {
             height: 2px;
    }
    QSplitter::handle:vertical {
             width: 2px;
    }
定制状态条 QStatusBar
将提供一个状态栏的边框与项目的定制：
    QStatusBar {
             background: brown;
    }
    QStatusBar::item {
             border: 1px solid red;
             border-radius: 3px;
    }
定制 QTabWidget 与QTabBar
    QTabWidget::pane { /* The tab widget frame */
             border-top: 2px solid #C2C7CB;
    }
    QTabWidget::tab-bar {
             left: 5px; /* move to the right by 5px */
    }
    /* Style the tab using the tab sub-control. Note thatt reads QTabBar _not_ QTabWidget */
    QTabBar::tab {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #E1E1E1, stop: 0.4 #DDDDDD,
                                         stop: 0.5 #D8D8D8, stop: 1.0 #D3D3D3);
             border: 2px solid #C4C4C3;
             border-bottom-color: #C2C7CB; /* same as the pane color */
             border-top-left-radius: 4px;
             border-top-right-radius: 4px;
             min-width: 8ex;
             padding: 2px;
    }
    QTabBar::tab:selected, QTabBar::tab:hover {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #fafafa, stop: 0.4 #f4f4f4,
                                         stop: 0.5 #e7e7e7, stop: 1.0 #fafafa);
    }
    QTabBar::tab:selected {
             border-color: #9B9B9B;
             border-bottom-color: #C2C7CB; /* same as pane color */
    }
    QTabBar::tab:!selected {
             margin-top: 2px; /* make non-selected tabs look smaller */
    }
    QTabWidget::pane { /* The tab widget frame */
             border-top: 2px solid #C2C7CB;
    }
    QTabWidget::tab-bar {
             left: 5px; /* move to the right by 5px */
    }
    /* Style the tab using the tab sub-control. Note that it reads QTabBar _not_ QTabWidget */
    QTabBar::tab {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #E1E1E1, stop: 0.4 #DDDDDD,
                                         stop: 0.5 #D8D8D8, stop: 1.0 #D3D3D3);
             border: 2px solid #C4C4C3;
             border-bottom-color: #C2C7CB; /* same as the pane color */
             border-top-left-radius: 4px;
             border-top-right-radius: 4px;
             min-width: 8ex;
             padding: 2px;
    }
    QTabBar::tab:selected, QTabBar::tab:hover {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #fafafa, stop: 0.4 #f4f4f4,
                                         stop: 0.5 #e7e7e7, stop: 1.0 #fafafa);
    }
    QTabBar::tab:selected {
             border-color: #9B9B9B;
             border-bottom-color: #C2C7CB; /* same as pane color */
    }
    
    QTabBar::tab:!selected {
             margin-top: 2px; /* make non-selected tabs look smaller */
    }
    /* make use of negative margins for overlapping tabs */
    QTabBar::tab:selected {
             /* expand/overlap to the left and right by 4px */
             margin-left: -4px;
             margin-right: -4px;
    }
    QTabBar::tab:first:selected {
             margin-left: 0; /* the first selected tab has nothing to overlap with on the left */
    }
    QTabBar::tab:last:selected {
             margin-right: 0; /* the last selected tab has nothing to overlap with on the right */
    }
    QTabBar::tab:only-one {
             margin: 0; /* if there is only one tab, we don't want overlapping margins */
    }
    QTabWidget::pane { /* The tab widget frame */
             border-top: 2px solid #C2C7CB;
             position: absolute;
             top: -0.5em;
    }
    QTabWidget::tab-bar {
             alignment: center;
    }
    /* Style the tab using the tab sub-control. Note that it reads QTabBar _not_ QTabWidget */
    QTabBar::tab {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #E1E1E1, stop: 0.4 #DDDDDD,
                                         stop: 0.5 #D8D8D8, stop: 1.0 #D3D3D3);
             border: 2px solid #C4C4C3;
             border-bottom-color: #C2C7CB; /* same as the pane color */
             border-top-left-radius: 4px;
             border-top-right-radius: 4px;
             min-width: 8ex;
             padding: 2px;
    }
    QTabBar::tab:selected, QTabBar::tab:hover {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #fafafa, stop: 0.4 #f4f4f4,
                                         stop: 0.5 #e7e7e7, stop: 1.0 #fafafa);
    }
    QTabBar::tab:selected {
             border-color: #9B9B9B;
             border-bottom-color: #C2C7CB; /* same as pane color */
    }
定制QTableWidget 
下面将设置QTableWidget  的粉色选中区域， 与白色背景。
    QTableWidget {
             selection-background-color: qlineargradient(x1: 0, y1: 0, x2: 0.5, y2: 0.5,
                                         stop: 0 #FF92BB, stop: 1 white);
    }
    QTableWidget QTableCornerButton::section {
             background: red;
             border: 2px outset red;
         }
定制工具条
下面是对工具条的一些选项进行定制
    QToolBar {
             background: red;
             spacing: 3px; /* spacing between items in the tool bar */
    }
    QToolBar::handle {
             image: url(handle.png);  //可以设置工具条的背景图片
    }  
定制QToolBox
将使用到 tab  的子控部分
    QToolBox::tab {
             background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                         stop: 0 #E1E1E1, stop: 0.4 #DDDDDD,
                                         stop: 0.5 #D8D8D8, stop: 1.0 #D3D3D3);
             border-radius: 5px;
             color: darkgray;
    }
    QToolBox::tab:selected { /* italicize selected tabs */
             font: italic;
             color: white;
    }
 定制QToolButton 
    QToolButton { /* all types of tool button */
             border: 2px solid #8f8f91;
             border-radius: 6px;
             background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                               stop: 0 #f6f7fa, stop: 1 #dadbde);
    }
    QToolButton[popupMode="1"] { /* only for MenuButtonPopup */
             padding-right: 20px; /* make way for the popup button */
    }
    QToolButton:pressed {
             background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                               stop: 0 #dadbde, stop: 1 #f6f7fa);
    }
    /* the subcontrols below are used only in the MenuButtonPopup mode */
    QToolButton::menu-button {
             border: 2px solid gray;
             border-top-right-radius: 6px;
             border-bottom-right-radius: 6px;
             /* 16px width + 4px for border = 20px allocated above */
             width: 16px;
    }
    QToolButton::menu-arrow {
             image: url(downarrow.png);
    }
    QToolButton::menu-arrow:open {
             top: 1px; left: 1px; /* shift it a bit */
    }
定制QTreeView 
    QTreeView::branch {
                 background: palette(base);
    }
    QTreeView::branch:has-siblings:!adjoins-item {
                 background: cyan;
    }
    QTreeView::branch:has-siblings:adjoins-item {
                 background: red;
    }
    QTreeView::branch:!has-children:!has-siblings:adjoins-item {
                 background: blue;
    }
    QTreeView::branch:closed:has-children:has-siblings {
                 background: pink;
    }
    QTreeView::branch:has-children:!has-siblings:closed {
                 background: gray;
    }
    QTreeView::branch:open:has-children:has-siblings {
                 background: magenta;
    }
    QTreeView::branch:open:has-children:!has-siblings {
                 background: green;
    }
下面是几个样式， 当选择颜色的时候可使用十六进制的数据来表达：
    QRadioButton{
          background:#5F7536;
          color:#CBF57D;
          font:bold;
    }

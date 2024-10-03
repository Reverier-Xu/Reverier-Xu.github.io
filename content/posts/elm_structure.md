---
title: The Elm Structure for GUI
date: 2024-01-05T16:55:42+08:00
tags: [Development, GTK, Rust, Relm]
categories: [Development]
---

## 缘由

[Elm](https://elm-lang.org/) 是一种函数式编程语言，这个项目在2012年才开始，对编程语言界这群动辄上个世纪的老东西们来说，挺新的。这个语言承诺不会出现运行时异常，并且声称有很出色的性能和“令人愉快的开发人员体验”（虽然每个语言都这么说，但是个人感觉着实没几门语言在大型项目里写起来真的令人愉快）。

实际上我也没有看见过有什么网站/开源项目是使用Elm写的——至少比较有名的开源项目里没怎么见过，但是 Elm Structure 却很经常出现在各种新兴的 UI 框架里，无论是 Web 端还是 Native GUI，比如 [Redux](https://redux.js.org/)，[iced-rs](https://github.com/iced-rs/iced)，[Relm4](https://github.com/Relm4/Relm4)，[Xilem](https://raphlinus.github.io/rust/gui/2022/05/07/ui-architecture.html)...等等等。

## The Elm Architecture

Elm 是一种用于构建交互式程序的模式。

按照他们自己的介绍，这种架构是写着写着自己变成这样的：

> This architecture seems to emerge naturally in Elm. Rather than someone inventing it, early Elm programmers kept discovering the same basic patterns in their code. It was kind of spooky to see people ending up with well-architected code without planning ahead!

### Basic Pattern

在 Elm 的 UI 模型中，人机交互主要分为了三个部分：

- Model：用来表示应用程序的状态；
- View：一种将状态转换为用户界面的方法；
- Update：一种根据消息更新状态的方法；

因此整个消息循环实际上呈现出下面这样的结构：

```plaintext
    +-----> View -------+
    |                   |
    |                   v
  Model <----------- Update
```

看起来有些像以前进化过的 [MVC(Model-View-Controller)](https://developer.mozilla.org/en-US/docs/Glossary/MVC) 模型？但实际上不太一样，Elm 的架构要更加简单一些。在传统 MVC 架构中，View 的行为是通过 Controller 直接控制的，但是 View 的更改会直接影响 Model，然后 Controller 通过监听 Model 的变化来进行响应（也有的 MVC 模型是 Controller -> Model -> View 的，然后 View 与 Controller 进行双向交互响应）；而 Elm 的架构中，View 操作会直接调用 Update 方法，Update 方法会采取措施更新 Model，最终 View 根据 Model 重新构建出用户界面达到一次响应。

我们可以发现 MVC 和 Elm 架构最大的区别是 Elm 保证了数据的单向流动。

### Relm4

Relm4 是一个基于 gtk-rs 的 Native UI 框架。我以前一直对 gtk 抱有很大的偏见（~~一个经典的形容就是，GNOME/GTK就是巧克力味的屎，KDE/Qt是屎味的巧克力~~），但是不得不承认与其他 UI 框架相比，GTK 的可用性一直都是在线的。尝试了几个基于 GTK4 的应用之后，除了 Windows 下的字体渲染有点问题之外，其他的体验都还不错。

> 欸 woc，copilot 都知道 Windows 下面各大框架都有啥问题，微软你能不能改改啊.jpg
>
> ![](1.png)

## 基本单元

在 Elm Structure 中，**组件** 是整套UI系统的基本单元。一个 **组件** 被描述为构造用户界面的基本块，一个组件可以由多个组件组合而成。

### 状态和消息

每个组件都有以下三种数据结构：

1. `Model`：组件的状态；
2. `Input`：组件从外部接收的消息；
3. `Output`：组件向外部发送的消息；

其中 `Model` 一般是一个结构体，里面存储着组件的持久化状态信息，而 `Input` 和 `Output` 是枚举（Enum）类型，用来指示消息类型，并携带消息的内容。

在 Relm4 的设计中，组件依靠 `init` 和 `update` 函数进行消息的处理。当组件接收到消息时，`update` 函数就会被调用，然后函数根据枚举类型进行消息的分发处理，例如更新 `Model`，将消息转码之后转发 `Output` 等等。

### 组件

组件是状态、消息、处理逻辑和界面描述的组合，一个组件就表示了一个完整的具有响应性和对应行为的用户界面，例如一个会发送点击信号的按钮、能以编程方式和用户点击方式更改选中状态的单选框等等。

在 Relm4 中，组件是通过 `SimpleComponent trait` 来定义的，这个 `trait` 会要求开发者实现 `Model`、`Input`、`Output` 的定义以及 `init` 和 `update` 函数，并提供了 `view!` 宏用来生成用户界面。

## 例子

这里以一个简单的计数器作为例子。

首先我们要定义组件的状态，在这个例子中组件的状态就是计数器的值，所以我们定义一个 `Model` 结构体：

```rust
struct AppModel {
    counter: u8,
}
```

接下来定义组件的消息，这里我们只需要一个 `Input` 类型的消息，用来表示用户点击了哪个按钮：

```rust
#[derive(Debug)]
enum AppInput {
    Increment,
    Decrement,
}
```

最后是界面定义和 `SimpleComponent` 的实现：

```rust
#[relm4::component]
impl SimpleComponent for AppModel {
    type Init = u8;

    type Input = AppMsg;
    type Output = ();

    view! {
        gtk::Window {
            set_title: Some("Simple app"),
            set_default_width: 300,
            set_default_height: 100,

            gtk::Box {
                set_orientation: gtk::Orientation::Vertical,
                set_spacing: 5,
                set_margin_all: 5,

                gtk::Button {
                    set_label: "Increment",
                    connect_clicked[sender] => move |_| {
                        sender.input(AppMsg::Increment);
                    }
                },

                gtk::Button::with_label("Decrement") {
                    connect_clicked[sender] => move |_| {
                        sender.input(AppMsg::Decrement);
                    }
                },

                gtk::Label {
                    #[watch]
                    set_label: &format!("Counter: {}", model.counter),
                    set_margin_all: 5,
                }
            }
        }
    }

    // Initialize the UI.
    fn init(
        counter: Self::Init,
        root: &Self::Root,
        sender: ComponentSender<Self>,
    ) -> ComponentParts<Self> {
        let model = AppModel { counter };

        // Insert the macro code generation here
        let widgets = view_output!();

        ComponentParts { model, widgets }
    }

    fn update(&mut self, msg: Self::Input, _sender: ComponentSender<Self>) {
        match msg {
            AppMsg::Increment => {
                self.counter = self.counter.wrapping_add(1);
            }
            AppMsg::Decrement => {
                self.counter = self.counter.wrapping_sub(1);
            }
        }
    }
}
```

`view!` 宏中以一种类似于 QML 的声明式界面写法构造了用户界面，并将 `Button` 的点击事件连接到组件上，通过 `sender` 方法通知 `update` 函数进行消息处理。

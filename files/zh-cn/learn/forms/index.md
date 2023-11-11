---
title: Web 表单指南
slug: Learn/Forms
---

{{LearnSidebar}}

这个模块提供了一系列帮助你掌握 web 表单的文章。web 表单是与用户交互的强大工具，它们经常用于收集用户的数据，或允许他们控制用户界面。然而，由于历史和技术上的原因，如何充分发挥它们的潜力并不总是显而易见的。在下方列出的文章中，我们将介绍 Web 表单的各个方面：HTML 结构、样式、验证表单数据，以及提交数据到服务器。

> **标注：**
>
> #### 想成为一名前端网页开发者？
>
> 我们整理了一个课程，其中包含你实现目标所需的所有基本内容。
>
> [**开始使用**](/zh-CN/docs/Learn/Front-end_web_developer)

## 前提

在开始这个模块之前，你至少应该熟悉[对 HTML 的介绍](/zh-CN/docs/Learn/HTML/Introduction_to_HTML)系列。此时此刻，你应该会发现[基本指南](#基本指南)很容易理解，并且能够使用我们的[原生表单控件](/zh-CN/docs/Learn/Forms/Basic_native_form_controls)指南。

然而，掌握表单需要的不仅仅是 HTML 知识——你还需要学习一些特定的技术来设置表单控件的样式，并且需要一些编码知识来处理诸如验证和创建自定义表单控件之类的事情。因此，在你查看下面列出的其他部分之前，我们建议你先离开，并学习一些 [CSS](/zh-CN/docs/Learn/CSS) 和 [JavaScript](/zh-CN/docs/Learn/JavaScript)。

以上内容明确地说明了为什么我们将 web 表单独立放在一个模块中，而不是将其与 HTML、CSS 和 JavaScript 这些模块放在一起——表单元素比绝大多数其他的 HTML 元素都要复杂，并且它们还需要与相关的 CSS 和 JavaScript 技术紧密集合才能充分利用它们。

> **备注：** 如果你正在使用一个无法创建自己的文件的计算机、平板电脑或其他设备，那么你可以尝试（大多数）在线编码程序中的代码示例，例如 [JSBin](https://jsbin.com/) 或 [Glitch](https://glitch.com/)。

## 基本指南

- [你的第一个表单](/zh-CN/docs/Learn/Forms/Your_first_form)
  - : 本系列的第一篇文章提供了第一次创建 HTML 表单的经验，包括设计一个简单表单、使用正确的 HTML 元素实现它、通过 CSS 添加一些非常简单的样式，以及如何将数据发送到服务器。
- [如何构造 HTML 表单](/zh-CN/docs/Learn/Forms/How_to_structure_a_web_form)
  - : 有了基础知识，我们现在更详细地了解了用于为表单的不同部分提供结构和意义的元素。

## 不同的表单控件

- [原生表单控件](/zh-CN/docs/Learn/Forms/Basic_native_form_controls)
  - : 我们从详细了解原始 HTML {{htmlelement("input")}} 元素的类型开始，同时学习在收集不同类型数据时可用的选择。
- [HTML5 input 的类型](/zh-CN/docs/Learn/Forms/HTML5_input_types)
  - : 在这里，我们深入挖掘 `<input>` 元素，了解 HTML5 发布时提供的其他 input 类型，以及它们提供的各种 UI 弓箭和数据收集增强功能。此外，我们可以了解 {{htmlelement('output')}} 元素。
- [其他表单控件](/zh-CN/docs/Learn/Forms/Other_form_controls)
  - : 接下来，我们学习所有非 `<input>` 表单控件，以及相关的工具，如：{{htmlelement('select')}}、{{htmlelement('textarea')}}、{{htmlelement('meter')}} 和 {{htmlelement('progress')}}。

## 表单样式指南

- [HTML 表单样式](/zh-CN/docs/Learn/Forms/Styling_web_forms)
  - : 本文介绍了如何使用 CSS 为表单赋予样式，包括可能需要了解的基本样式任务的所有基础知识。
- [高级表单样式](/zh-CN/docs/Learn/Forms/Advanced_form_styling)
  - : 在这里，我们将看到一些更高级的表单样式技术，这些技术需要在处理一些更难以设置样式的元素时使用。
- [UI 伪类](/zh-CN/docs/Learn/Forms/UI_pseudo-classes)
  - : 这是一篇对 UI 伪类的介绍，使 HTML 表单控件能够根据当前状态进行定位。

## 验证和提交表单数据

- [客户端表单验证](/zh-CN/docs/Learn/Forms/Form_validation)
  - : 仅仅发送数据还不够——我们还需要确保用户填写表单的数据格式是正确的，以在后续流程中正确处理它，而且它不会破坏我们的应用程序。我们还希望帮助用户正确填写表单，让他们在使用应用程序时不会感到困扰。表单验证可以帮助我们实现这些目标，本文将告诉你需要了解的内容。
- [发送表单数据](/zh-CN/docs/Learn/Forms/Sending_and_retrieving_form_data)
  - : 本文讨论当用户提交一个表单时，会发生什么情况——表单数据的去向，以及当表单数据到达指定位置时我们如何处理？我们还研究了与发送表单数据相关的一些安全问题。

## 高级指南

以下文章对于学习路径来说没那么重要，但是当你掌握了上述技术并向了解更多内容时，它们会变得有趣且实用。

- [如何构建自定义表单控件](/zh-CN/docs/Learn/Forms/How_to_build_custom_form_controls)
  - : 在某些情况下，可能出于样式或功能的原因，原生表单部件无法提供你所需要的东西。在这种情况下，你可能需要使用原生 HTML 构建自己的表单小部件。本文将讲述一个实例，说明你将如何做到这一点，以及在实际操作时需要注意的事项。
- [通过 JavaScript 发送表单](/zh-CN/docs/Learn/Forms/Sending_forms_through_JavaScript)
  - : 本文将讨论如何使用表单来构建 HTTP 请求，并通过定制的 JavaScript 发送它，而不是标准的表单提交。它还说明了为什么要这么做，以及这样做的意义。（请参阅[使用 FormData 对象](/zh-CN/docs/Web/API/XMLHttpRequest_API/Using_FormData_Objects)。）
- [表单控件的 CSS 属性兼容性表](/zh-CN/docs/Learn/Forms/Property_compatibility_table_for_form_controls)
  - : 最后一篇文章提供了一个方便的参考，以确定 CSS 属性与表单元素的兼容性关系。

## 参见

- [HTML 表单元素](/zh-CN/docs/Web/HTML/Element#表单)
- [HTML \<input> 元素](/zh-CN/docs/Web/HTML/Element/Input)
- [HTML 属性](/zh-CN/docs/Web/HTML/Attributes)

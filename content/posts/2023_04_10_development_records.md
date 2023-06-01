---
title: "如何在前端弹出来一个框"
date: 2023-04-10T20:49:27+08:00
tags: [Development]
categories: [Development]
---

目前 Cyber Terminal 前端的基础样式解决方案是`tailwindcss`，配合 `daisy UI` 和我自己封装的一套 `RxUI` 勉强凑合着用。`daisy UI` 的设计理念是纯 CSS 实现，不掺杂任何的 JavaScript 代码，我挺喜欢这种实现方式，纯 CSS 实现的用户界面比掺杂了 JS 的界面总是让人更放心一点。但是 CSS 终究是没有 JS 强大的，它只是一套静态布局系统，这就导致了一系列的用户体验问题。

比如说一个简单的弹出框。在 `daisy UI` 的解决方案中，他们使用了元素的 `focus` 状态，配合 CSS 选择器来显示弹出元素。这乍一听好像挺符合设计思想的，但是用的时候就出现了一堆问题。为了保证元素正确加载，在未显示的情况下，弹出元素上设置的并不是 `display: none`，而是 `visibility: hidden`，这就导致弹出元素即使在未显示的状态下也占据了实际空间的，只是不可见而已，可能会在某些情况下打乱布局。

比如，我想要实现一个可滚动的 `Table` 组件，在表格的每一列上我都放置了一系列操作按钮，对于比较危险的操作，例如删除，会有一个弹出框让用户进行二次确认。这个时候问题就来了，由于弹出框在未显示的情况下也是占据空间的，最后一列上的弹出框就会继续向下拓展，就导致了表格滚动到最后一列后还能继续向下滚动一段距离，看起来很奇怪。

问题还不止这一点，由于 CSS 没有类似于 `floating` 的功能，元素是无法探查可视边界的。Table 组件默认可滚动，导致内部元素的溢出行为是`clip`，于是把溢出窗口的对话框一起给切了。不只是对话框，还有 `tooltip` 之类的东西，会变成这个样子：

![部分文字被切掉了](https://files.catbox.moe/rccxzh.png)

嘛……虽然应该没人拿宽度这么巧的设备打CTF……但是这个行为太蠢了，我写的时候得时时刻刻注意着弹出位置，放左边溢出了，放右边也溢出了，放下边好消息是没溢出，坏消息是给滚动条撑起来了……

![啊？](https://files.catbox.moe/0up7lu.jpg)

于是我就开始找解决方案，找着找着找到了Microsoft在油管上发的[Fluent UI Design相关视频](https://www.youtube.com/watch?v=yhzAn4A1gbk)。他们最终选了 [popper.js](https://popper.js.org/) 作为弹出式组件的解决方案。但是……这个组件只有 React 框架的集成方案，Vue 的几个第三方集成方案都不太好使了。

还是自己写吧……

最终选用了 Floating UI 作为实现方案，按照svelte的生命周期简单包装了一下。相关API参考都在这里了：

```typescript
import { computePosition, autoUpdate, offset, shift, flip } from '@floating-ui/dom'

/** Placement https://floating-ui.com/docs/computePosition#placement */
export type Direction = 'top' | 'bottom' | 'left' | 'right'
export type Placement = Direction | `${Direction}-start` | `${Direction}-end`

// Options & Middleware
export interface Middleware {
  // Required ---
  /** Offset middleware settings: https://floating-ui.com/docs/offset */
  offset?: number | Record<string, unknown>
  /** Shift middleware settings: https://floating-ui.com/docs/shift */
  shift?: Record<string, unknown>
  /** Flip middleware settings: https://floating-ui.com/docs/flip */
  flip?: Record<string, unknown>
  // Optional ---
  /** Size middleware settings: https://floating-ui.com/docs/size */
  size?: Record<string, unknown>
  /** Auto Placement middleware settings: https://floating-ui.com/docs/autoPlacement */
  autoPlacement?: Record<string, unknown>
  /** Hide middleware settings: https://floating-ui.com/docs/hide */
  hide?: Record<string, unknown>
  /** Inline middleware settings: https://floating-ui.com/docs/inline */
  inline?: Record<string, unknown>
}

export interface PopupSettings {
  /** Provide the event type. */
  event: 'click' | 'hover' | 'focus-blur' | 'focus-click'
  /** Match the popup data value `data-popup="targetNameHere"` */
  target: string
  /** Set the placement position. Defaults 'bottom'. */
  placement?: Placement
  /** Query elements that close the popup when clicked. Defaults `'a[href], button'`. */
  closeQuery?: string
  /** Optional callback function that reports state change. */
  state?: (event: { state: boolean }) => void
  /** Provide Floating UI middleware settings. */
  middleware?: Middleware
}

export function popup(triggerNode: HTMLElement, args: PopupSettings) {
  // Local State
  const popupState = {
    open: false,
    // eslint-disable-next-line @typescript-eslint/no-empty-function
    autoUpdateCleanup: () => {},
  }
  const focusableAllowedList = ':is(a[href], button, input, textarea, select, details, [tabindex]):not([tabindex="-1"])'
  let focusablePopupElements: HTMLElement[]
  // Elements
  let elemPopup: HTMLElement

  function setDomElements(): void {
    elemPopup = document.querySelector(`[data-popup="${args.target}"]`) ?? document.createElement('div')
    Object.assign(elemPopup.style, {
      position: 'absolute',
      opacity: '0',
      top: '0',
      left: '0',
      scale: '0.95',
      display: 'none',
    })
    elemPopup.classList.add('transition-all', 'duration-100', 'ease-in-out')
  }
  setDomElements() // init

  // Render Floating UI Popup
  function render(): void {
    // Error handling for required Floating UI modules
    if (!elemPopup) throw new Error(`The data-popup="${args.target}" element was not found.`)
    if (!computePosition) throw new Error(`Floating UI 'computePosition' not found for data-popup="${args.target}".`)
    if (!offset) throw new Error(`Floating UI 'offset' not found for data-popup="${args.target}".`)
    if (!shift) throw new Error(`Floating UI 'shift' not found for data-popup="${args.target}".`)
    if (!flip) throw new Error(`Floating UI 'flip' not found for data-popup="${args.target}".`)

    // Floating UI Compute Position
    // https://floating-ui.com/docs/computePosition
    computePosition(triggerNode, elemPopup, {
      placement: args.placement ?? 'bottom',

      // Middleware - NOTE: the order matters:
      // https://floating-ui.com/docs/middleware#ordering
      middleware: [
        // https://floating-ui.com/docs/offset
        offset(args.middleware?.offset ?? 8),
        // https://floating-ui.com/docs/shift
        shift(args.middleware?.shift ?? { padding: 8 }),
        // https://floating-ui.com/docs/flip
        flip(args.middleware?.flip),
      ],
    }).then(({ x, y }) => {
      Object.assign(elemPopup.style, {
        left: `${x}px`,
        top: `${y}px`,
      })
    })
  }

  // State Handlers
  function open(): void {
    if (!elemPopup) return
    // Set open state to on
    popupState.open = true
    // Return the current state
    if (args.state) args.state({ state: popupState.open })
    // Update render settings
    render()
    // Update the DOM
    elemPopup.style.display = 'block'
    setTimeout(() => {
      elemPopup.style.opacity = '1'
      elemPopup.style.scale = '1'
    }, 10)
    elemPopup.style.pointerEvents = 'auto'
    // Trigger Floating UI autoUpdate (open only)
    // https://floating-ui.com/docs/autoUpdate
    popupState.autoUpdateCleanup = autoUpdate(triggerNode, elemPopup, render)
    // Focus the first focusable element within the popup
    focusablePopupElements = Array.from(elemPopup?.querySelectorAll(focusableAllowedList))
  }
  function close(callback?: () => void): void {
    if (!elemPopup) return
    // Set transition duration
    const cssTransitionDuration =
      parseFloat(window.getComputedStyle(elemPopup).transitionDuration.replace('s', '')) * 1000
    // Set open state to off
    popupState.open = false
    // Return the current state
    if (args.state) args.state({ state: popupState.open })
    // Update the DOM
    elemPopup.style.opacity = '0'
    elemPopup.style.scale = '0.95'
    setTimeout(() => {
      elemPopup.style.display = 'none'
    }, cssTransitionDuration)
    elemPopup.style.pointerEvents = 'none'
    // Cleanup Floating UI autoUpdate (close only)
    if (popupState.autoUpdateCleanup) popupState.autoUpdateCleanup()
    // Trigger callback
    if (callback) callback()
  }

  // Event Handlers
  function toggle(): void {
    popupState.open === false ? open() : close()
  }
  function onWindowClick(event: Event): void {
    // Return if the popup is not yet open
    if (popupState.open === false) return
    // Return if click is the trigger element
    if (triggerNode.contains(event.target as Node)) return
    // If click it outside the popup
    if (elemPopup && elemPopup.contains(event.target as Node) === false) {
      close()
      return
    }
    // Handle Close Query State
    const closeQueryString: string = args.closeQuery === undefined ? 'a[href], button' : args.closeQuery
    const closableMenuElements = elemPopup?.querySelectorAll(closeQueryString)
    closableMenuElements?.forEach((elem) => {
      if (elem.contains(event.target as Node)) close()
    })
  }

  // Keyboard Interactions for A11y
  const onWindowKeyDown = (event: KeyboardEvent): void => {
    if (popupState.open === false) return
    // Handle keys
    const key: string = event.key
    // On Esc key
    if (key === 'Escape') {
      event.preventDefault()
      triggerNode.focus()
      close()
      return
    }
    // On Tab or ArrowDown key
    const triggerMenuFocused: boolean = popupState.open && document.activeElement === triggerNode
    if (
      triggerMenuFocused &&
      (key === 'ArrowDown' || key === 'Tab') &&
      focusableAllowedList.length > 0 &&
      focusablePopupElements.length > 0
    ) {
      event.preventDefault()
      focusablePopupElements[0].focus()
    }
  }

  // Event Listeners
  switch (args.event) {
    case 'click':
      triggerNode.addEventListener('click', toggle, true)
      window.addEventListener('click', onWindowClick, true)
      break
    case 'hover':
      triggerNode.addEventListener('mouseover', open, true)
      triggerNode.addEventListener('mouseleave', () => close(), true)
      break
    case 'focus-blur':
      triggerNode.addEventListener('focus', toggle, true)
      triggerNode.addEventListener('blur', () => close(), true)
      break
    case 'focus-click':
      triggerNode.addEventListener('focus', open, true)
      window.addEventListener('click', onWindowClick, true)
      break
    default:
      throw new Error(`Event value of '${args.event}' is not supported.`)
  }
  window.addEventListener('keydown', onWindowKeyDown, true)

  // Render popup on initialization
  render()

  // Lifecycle
  return {
    update(newArgs: PopupSettings) {
      close(() => {
        args = newArgs
        render()
        setDomElements()
      })
    },
    destroy() {
      // Trigger Events
      triggerNode.removeEventListener('click', toggle, true)
      triggerNode.removeEventListener('mouseover', open, true)
      triggerNode.removeEventListener('mouseleave', () => close(), true)
      triggerNode.removeEventListener('focus', toggle, true)
      triggerNode.removeEventListener('focus', open, true)
      triggerNode.removeEventListener('blur', () => close(), true)
      // Window Events
      window.removeEventListener('click', onWindowClick, true)
      window.removeEventListener('keydown', onWindowKeyDown, true)
    },
  }
}
```

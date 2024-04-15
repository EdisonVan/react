# 源码阅读

## Hooks的定义以及执行前后的准备和重置
- 更新阶段：[先prepare再finish function updateFunctionComponent(](./packages/react-reconciler/src/ReactFiberBeginWork.js)
- [Hooks的具体实现](packages/react-reconciler/src/ReactFiberHooks.js)
    - reacnt Hooks：FE-interview/Code/react-hooks/packages/react-reconciler/src/ReactFiberHooks.js
- [新context的实现](packages/react-reconciler/src/ReactFiberNewContext.js)

### 代码中的解读标记
> 代码中全局搜索标记来定位代码注释
 
- [A1]:[useEffect 和 useLayoutEffect 的区别](https://juejin.cn/post/6921688408737710087)
    - 240415107:更新Fiber树时判断currentHook是否等于null判断是否为第一次渲染，如果不是第一次渲染则会执行一次对比，
    - 240415110
        - 这个过程其实就是往当前 Fiber 上增加一系列 effectTag，并且会创建 updateQueue，这跟 Hostcomponent 类似，这个 queue 会在 commit 阶段被执行
        - useLayoutEffect 和 useEffect 增加的 effectTag 是不一样的，所以他们执行的时机也是不一样的
        - effectTag 会有以下几种情况：
            - useLayoutEffect 增加 UpdateEffect
            - useEffect 增加 UpdateEffect | PassiveEffect
        - 以上是增加在 Fiber 对象上的，而记录对应Hook对象的 effectTag 如下
            - useLayoutEffect 增加 UnmountMutation | MountLayout
            - useEffect 增加 UnmountPassive | MountPassive
            - 如果 areHookInputsEqual 符合，则增加 NoHookEffect
    - 2404151131 
    - 2404151521 // 10:22
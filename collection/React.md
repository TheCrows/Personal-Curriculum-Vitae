# REACT

## 组件类

### PureComponent

纯组件PureComponent会浅比较，props和state是否相同，来决定是否重新渲染组件。所以一般用于性能调优，减少render次数。

### React.memo

React.memo和PureComponent作用类似，可以用作性能优化，React.memo 是高阶组件，函数组件和类组件都可以使用， 和区别PureComponent是 React.memo只能对props的情况确定是否渲染，而PureComponent是针对props和state。
React.memo 接受两个参数，第一个参数原始组件本身，第二个参数，可以根据一次更新中props是否相同决定原始组件是否重新渲染。是一个返回布尔值，true 证明组件无须重新渲染，false证明组件需要重新渲染，这个和类组件中的shouldComponentUpdate()正好相反 。
React.memo: 第二个参数 返回 true 组件不渲染 ， 返回 false 组件重新渲染。
shouldComponentUpdate: 返回 true 组件渲染 ， 返回 false 组件不渲染。

### forwardRef

```javascript
const NewFather = React.forwardRef((props,ref)=><Father grandRef={ref}  {...props} />  )
```

### lazy & Suspense

```javascript
const ProfilePage = React.lazy(() => import('./ProfilePage')); // 懒加载
<Suspense fallback={<Spinner />}>
  <ProfilePage />
</Suspense>
```

### Fragment

和Fragment区别是，Fragment可以支持key属性。<></>不支持key属性。

### Profiler

Profiler这个api一般用于开发阶段，性能检测，检测一次react组件渲染用时，性能开销。

### StrictMode

## 工具类

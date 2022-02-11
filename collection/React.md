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

###  createElement

替代jsx
```javascript
React.createElement(
  type,
  [props],
  [...children]
)
```
经过createElement处理，最终会形成 $$typeof = Symbol(react.element)对象。对象上保存了该react.element的信息。

### cloneElement

比如说，我们可以在组件中，劫持children element，然后通过cloneElement克隆element，混入props。
```javascript
function FatherComponent({ children }){
    const newChildren = React.cloneElement(children, { age: 18})
    return <div> { newChildren } </div>
}

function SonComponent(props){
    console.log(props)
    return <div>hello,world</div>
}

class Index extends React.Component{    
    render(){      
        return <div className="box" >
            <FatherComponent>
                <SonComponent name="alien"  />
            </FatherComponent>
        </div>   
    }
}
```

### createContext

```javascript
function ComponentB(){
    /* 用 Consumer 订阅， 来自 Provider 中 value 的改变  */
    return <MyContext.Consumer>
        { (value) => <ComponentA  {...value} /> }
    </MyContext.Consumer>
}

function ComponentA(props){
    const { name , mes } = props
    return <div> 
            <div> 姓名： { name }  </div>
            <div> 想对大家说： { mes }  </div>
         </div>
}

function index(){
    const [ value , ] = React.useState({
        name:'alien',
        mes:'let us learn React '
    })
    return <div style={{ marginTop:'50px' }} >
        <MyContext.Provider value={value}  >
          <ComponentB />
    </MyContext.Provider>
    </div>
}
```

### createRef

用于在class组件中直接捕获ref

### isValidElement

这个方法可以用来检测是否为react element元素,接受待验证对象，返回true或者false。

### Children.map/forEach/count/toArray/only

## react-hooks

### useMemo

```javascript
/*  缓存一些值，避免重新执行上下文 */
const number = useMemo(()=>{
    /** ....大量的逻辑运算 **/
   return number
},[ props.number ]) // 只有 props.number 改变的时候，重新计算number的值。

/* 用 useMemo包裹的list可以限定当且仅当list改变的时候才更新此list，这样就可以避免selectList重新循环 */
 {useMemo(() => (
      <div>{
          selectList.map((i, v) => (
              <span
                  className={style.listSpan}
                  key={v} >
                  {i.patentName} 
              </span>
          ))}
      </div>
), [selectList])}

/* 只有当props中，list列表改变的时候，子组件才渲染 */
const  goodListChild = useMemo(()=> <GoodList list={ props.list } /> ,[ props.list ])
```

### useCallback

useMemo 和 useCallback 接收的参数都是一样，都是在其依赖项发生变化后才执行，都是返回缓存的值，区别在于 useMemo 返回的是函数运行的结果， useCallback 返回的是函数。 返回的callback可以作为props回调函数传递给子组件。
useCallback(fn, deps) 相当于 useMemo(() => fn, deps)。
```javascript
const Child = React.memo(function({val, onChange}) {
    console.log('render...');
    
    return <input value={val} onChange={onChange} />;
  });
  
  function App() {
    const [val1, setVal1] = useState('');
    const [val2, setVal2] = useState('');
  
    const onChange1 = useCallback( evt => {
      setVal1(evt.target.value);
    }, []);
  
    const onChange2 = useCallback( evt => {
      setVal2(evt.target.value);
    }, []);
  
    return (
    <>
      <Child val={val1} onChange={onChange1}/>
      <Child val={val2} onChange={onChange2}/>
    </>
    );
  }
```

## useLayoutEffect

useEffect执行顺序: 组件更新挂载完成 -> 浏览器 dom 绘制完成 -> 执行 useEffect 回调。
useLayoutEffect 执行顺序: 组件更新挂载完成 ->  执行 useLayoutEffect 回调-> 浏览器dom绘制完成。

## useReducer

useReducer 接受的第一个参数是一个函数，我们可以认为它就是一个 reducer , reducer 的参数就是常规 reducer 里面的 state 和  action ,返回改变后的 state , useReducer 第二个参数为 state 的初始值 返回一个数组，数组的第一项就是更新之后 state 的值 ，第二个参数是派发更新的 dispatch 函数。
```javascript
const DemoUseReducer = ()=>{
    /* number为更新后的state值,  dispatchNumbner 为当前的派发函数 */
   const [ number , dispatchNumbner ] = useReducer((state,action)=>{
       const { payload , name  } = action
       /* return的值为新的state */
       switch(name){
           case 'add':
               return state + 1
           case 'sub':
               return state - 1 
           case 'reset':
             return payload       
       }
       return state
   },0)
   return <div>
      当前值：{ number }
      { /* 派发更新 */ }
      <button onClick={()=>dispatchNumbner({ name:'add' })} >增加</button>
      <button onClick={()=>dispatchNumbner({ name:'sub' })} >减少</button>
      <button onClick={()=>dispatchNumbner({ name:'reset' ,payload:666 })} >赋值</button>
      { /* 把dispatch 和 state 传递给子组件  */ }
      <MyChildren  dispatch={ dispatchNumbner } State={{ number }} />
   </div>
}
```

### useContext

```javascript
const Context = React.createContext({});
/* 用useContext方式 */
const DemoContext = ()=> {
    const value:any = useContext(Context)
    /* my name is alien */
return <div> my name is { value.name }</div>
}
/* 用Context.Consumer 方式 */
const DemoContext1 = ()=>{
    return <Context.Consumer>
         {/*  my name is alien  */}
        { (value)=> <div> my name is { value.name }</div> }
    </Context.Consumer>
}

export default ()=>{
    return <div>
        <Context.Provider value={{ name:'alien' , age:18 }} >
            <DemoContext />
            <DemoContext1 />
        </Context.Provider>
    </div>
}
```

### useImperativeHandle

```javascript
function FancyInput(props, ref) {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }));
  return <input ref={inputRef} ... />;
}
FancyInput = forwardRef(FancyInput);
```

## react-dom

### render

ReactDOM.render会控制container容器节点里的内容，但是不会修改容器节点本身。

`ReactDOM.render(element, container[, callback])`

```javascript
ReactDOM.render(
    < App / >,
    document.getElementById('app')
)
```

### hydrate

服务端渲染用hydrate。用法与 render() 相同，但它用于在 ReactDOMServer 渲染的容器中对 HTML 的内容进行 hydrate 操作。


### createPortal

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。createPortal 可以把当前组件或 element 元素的子节点，渲染到组件之外的其他地方。

`ReactDOM.createPortal(child, container)`

```javascript
function WrapComponent({ children }){
    const domRef = useRef(null)
    const [ PortalComponent, setPortalComponent ] = useState(null)
    React.useEffect(()=>{
        setPortalComponent( ReactDOM.createPortal(children,domRef.current) )
    },[])
    return <div> 
        <div className="container" ref={ domRef } ></div>
        { PortalComponent }
     </div>
}

class Index extends React.Component{
    render(){
        return <div style={{ marginTop:'50px' }} >
             <WrapComponent>
               <div  >hello,world</div>
            </WrapComponent>
        </div>
    }
}
```











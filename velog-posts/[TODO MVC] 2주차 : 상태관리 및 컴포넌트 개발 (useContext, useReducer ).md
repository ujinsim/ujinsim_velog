<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/f16dd915-8f64-4ed0-9ffe-b29225e47c68/image.png" /></p>
<p>*<em>일차적으로는 스토리지 기능 없이 상태관리로만 집중하는 개발방식으로 목적을 잡고 개발을 시작했습니다 *</em></p>
<h3 id="컴포넌트에는">컴포넌트에는</h3>
<p>ToDo 내용을 삭제하고 관리하는 부분은 같이 하고 있어요 그래서 그 부분을
useContext와 데이터를 수정하는 부분은 useReducer을 통해서 구현해봤습니다. </p>
<p>TodoContext.tsx</p>
<pre><code class="language-tsx">import { createContext, useReducer } from &quot;react&quot;;

const initialState = {
  todos: [
    { title: &quot;첫번째 투두&quot;, id: 1, isCompleted: false, isEditing: false },
  ],
};

const todoReducer = (state: typeof initialState, action: any) =&gt; {
  switch (action.type) {
    case &quot;ADD_TODO&quot;:
      return { todos: [...state.todos, action.payload] };
    case &quot;REMOVE_TODO&quot;:
      return {
        todos: state.todos.filter((todo) =&gt; todo.id !== action.payload.id),
      };
    default:
      return state;
  }
};

export const TodoStateContext = createContext&lt;typeof initialState | null&gt;(null);
export const TodoDispatchContext = createContext&lt;any&gt;(null);

export const TodoProvider = ({ children }: { children: React.ReactNode }) =&gt; {
  const [state, dispatch] = useReducer(todoReducer, initialState);

  return (
    &lt;TodoStateContext.Provider value={state}&gt;
      &lt;TodoDispatchContext.Provider value={dispatch}&gt;
        {children}
      &lt;/TodoDispatchContext.Provider&gt;
    &lt;/TodoStateContext.Provider&gt;
  );
};
</code></pre>
<h3 id="여기서-생긴-문제는">여기서 생긴 문제는</h3>
<p>한국어만 생성시 마지막 글자가 하나 더 추가된다는 것이였습니다 
밥먹기, 기 처럼.. . .
<img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/db06cbc4-90b7-447e-8ce3-1bdd44f64439/image.png" /></p>
<p>keydown에서 keyup으로 변경해주었씁니다 !</p>
<pre><code class="language-tsx">import { useEffect } from &quot;react&quot;;

function useEnterKey(callback: () =&gt; void) {
  useEffect(() =&gt; {
    const handleKeyUp = (event: KeyboardEvent) =&gt; {
      if (event.key === &quot;Enter&quot;) {
        event.preventDefault();
        callback();
      }
    };

    window.addEventListener(&quot;keyup&quot;, handleKeyUp);

    return () =&gt; {
      window.removeEventListener(&quot;keyup&quot;, handleKeyUp);
    };
  }, [callback]);

  return;
}

export default useEnterKey;

</code></pre>
<p>해당 방법으로 적용했더니 해결되었습니다 
keyup 이벤트를 사용하여 해결되었습니다. keydown이 아닌 <strong>keyup</strong>을 사용하면 사용자가 키를 떼었을 때 동작하므로 중복 입력이 발생하지 않게 되므로 해당 방법으로 해결할 수 있었습니다 </p>
<h3 id="다른-컴포넌트-마저-완성하고-구현을-완료하였습니다">다른 컴포넌트 마저 완성하고 구현을 완료하였습니다.</h3>
<p>todoItem에 있는 delete, toggle
addTodo에 있는 all toggle
filter에 있는 Clear complete 부분은 useReducer과 연결하여 같이 데이터를 다룰 수 있도록 처리하였습니다. </p>
<p>filter에 있는 아이템 갯수와 mode는 state로 처리하였습니다</p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/3e39def8-a0e0-42ea-9443-7b3d3f84289c/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ujinsimss_/post/6b216d3c-32b5-4824-a948-747f8a78118c/image.png" />
지난번에 이런식으로 컴포넌트를 분리하기로 하였씁니다</p>
<p>ui에서는 크게 컴포넌트를 </p>
<ol>
<li>AddTodo.tsx</li>
<li>TodoFilter.tsx,</li>
<li>TodoItem.tsx 3개로 나누었습니다. (1편 참고) </li>
</ol>
<p>현재는 todoList를 context로 (todoContext)
관리하고 todoList의 데이터 수정은 Reducer로 (todoReducer)
시각적인 todoList의 필터는 state로 관리하고있습니다 ( mode )</p>
<p>그래서 전역관리는 </p>
<h2 id="todocontexttsx">todoContext.tsx</h2>
<pre><code class="language-tsx">import { createContext, useReducer } from &quot;react&quot;;
import { todoList } from &quot;../types/todo&quot;;

const initialState: todoList = {
  todos: [],
};

type Action =
  | { type: &quot;ADD_TODO&quot;; payload: { title: string } }
  | { type: &quot;REMOVE_TODO&quot;; payload: { id: number } }
  | { type: &quot;TOGGLE_TODO&quot;; payload: { id: number } }
  | { type: &quot;CLEAR_COMPLETED_TODOS&quot;; payload: null }
  | { type: &quot;ALL_TOGGLE_TODO&quot; };

const todoReducer = (state: todoList, action: Action): todoList =&gt; {
  switch (action.type) {
    case &quot;ADD_TODO&quot;:
      return {
        todos: [
          ...state.todos,
          {
            ...action.payload,
            id: Date.now(),
            isCompleted: false,
            isEditing: false,
          },
        ],
      };
    case &quot;REMOVE_TODO&quot;:
      return {
        todos: state.todos.filter((todo) =&gt; todo.id !== action.payload.id),
      };
    case &quot;TOGGLE_TODO&quot;:
      return {
        todos: state.todos.map((todo) =&gt;
          todo.id === action.payload.id
            ? { ...todo, isCompleted: !todo.isCompleted }
            : todo
        ),
      };
    case &quot;CLEAR_COMPLETED_TODOS&quot;:
      return { todos: state.todos.filter((todo) =&gt; !todo.isCompleted) };
    case &quot;ALL_TOGGLE_TODO&quot;:
      return {
        todos: state.todos.map((todo) =&gt;
          todo.isCompleted
            ? { ...todo, isCompleted: false }
            : { ...todo, isCompleted: true }
        ),
      };
    default:
      return state;
  }
};

export const TodoStateContext = createContext&lt;todoList | null&gt;(null);
export const TodoDispatchContext = createContext&lt;React.Dispatch&lt;Action&gt; | null&gt;(
  null
);

export const TodoProvider = ({ children }: { children: React.ReactNode }) =&gt; {
  const [state, dispatch] = useReducer(todoReducer, initialState);

  return (
    &lt;TodoStateContext.Provider value={state}&gt;
      &lt;TodoDispatchContext.Provider value={dispatch}&gt;
        {children}
      &lt;/TodoDispatchContext.Provider&gt;
    &lt;/TodoStateContext.Provider&gt;
  );
};
</code></pre>
<p>다음과 같이 작성하였습니다. 
Reducer과 Context를 같이 관리하고 있습니다. 
이는 역할이 다르기에 다음주차때 분리할 것을 보입니다.</p>
<p>현재 제가 짠 코드에 대해서 설명을 덧붙히자면
전체 컴포넌트에서 todoList를 다루고있지는 앉지만 전체 컴포넌트에서 데이터 수정을 진행하고 있다는 점에서 todoReducer로 데이터 관리를 전역적으로 관리하는 것으로 구현하였습니다
그리고 mode같은 경우에는 현재 state로 받고있습니다 </p>
<p>App.tsx </p>
<pre><code class="language-tsx">import { useState } from &quot;react&quot;;
import { useContext } from &quot;react&quot;;
import { TodoStateContext, TodoDispatchContext } from &quot;./context/TodoContext&quot;;
import TodoItem from &quot;./components/TodoItem&quot;;
import AddTodo from &quot;./components/AddTodo&quot;;
import TodoFilter from &quot;./components/TodoFilter&quot;;
import { todo, todoFilter } from &quot;./types/todo&quot;;

function App() {
  const { todos } = useContext(TodoStateContext) ?? { todos: [] };
  const dispatch = useContext(TodoDispatchContext);
  const [mode, setMode] = useState&lt;todoFilter&gt;(&quot;All&quot;);

  return (
    &lt;div className=&quot;flex flex-col items-center justify-center h-screen bg-gray-100&quot;&gt;
      &lt;h1 className=&quot;text-4xl py-2&quot;&gt;Todo List&lt;/h1&gt;
      &lt;div&gt;
        &lt;div className=&quot;overflow-y-scroll max-h-96 bg-gray-200 rounded-lg rounded-b-none shadow-lg&quot;&gt;
          &lt;AddTodo dispatch={dispatch} /&gt;
          {todos?.map((todo: todo) =&gt; (
            &lt;TodoItem
              key={todo.id}
              content={todo.title}
              isChecked={todo.isCompleted}
              id={todo.id}
              mode={mode}
            /&gt;
          ))}
        &lt;/div&gt;
        &lt;TodoFilter setMode={setMode} mode={mode} todoLength={todos.length} /&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  );
}

export default App;
</code></pre>
<h3 id="mode를-state로-구현한-이유">mode를 state로 구현한 이유?</h3>
<p>todoFilter 에서 선택한 mode를 todoItem에 단순히 뿌려주는 구성이지만
반면 데이터 수정같은 경우에는 서로 상호작용을 하기 때문에 mode는 state로 구현하였습니다</p>
<h2 id="내가-생각하는-앞으로-개선해야될-점">내가 생각하는 앞으로 개선해야될 점</h2>
<ol>
<li><p>현재 상태 관리 방법인 useContext와 useReducer를 사용하고 있지만, 이 방식이 정말 최적인지 고민할 필요가 있습니다. 특히 애플리케이션이 커지거나 복잡해질 때 이 방식이 확장성이나 성능에 문제가 생길 수 있기 때문입니다. 그래서 다른 상태 관리 라이브러리를 써보며 비교하고 성능을 측정하고 싶습니다.</p>
</li>
<li><p>컴포넌트나 함수가 하나의 책임만 가지도록 해야 유지보수나 확장이 용이합니다. 예를 들어, TodoItem 컴포넌트는 단지 아이템을 표시하고 수정하는 역할만 해야 합니다. 상태 관리 로직과 UI 로직을 분리하여, 코드의 책임을 명확히 할 수 있습니다.
현재는 아이콘과 삭제하는 역할도 같이 하고 있기에 이 부분을 개선하는 방안을 고려해보고자 합니다. </p>
</li>
<li><p>컴포넌트가 너무 많으면 관리가 어려워질 수 있고, 너무 적으면 코드가 복잡해집니다. 상태 관리와 UI를 적절히 분리하고, 재사용 가능한 컴포넌트를 만들어야 합니다. 2번 과정을 거치다가 너무 많은 컴포넌트를 남발할 수 있기에 이를 주의하며 최적의 코드를 짜고자합니다 </p>
</li>
</ol>
<p>3주차때 뵙겠습니다 ~~</p>
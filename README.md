# React-design-patterns

## I. Conditional rendering

### 1. With a ternary operator:

a) conditionally choose between two `className` values:

**App.css**

```css
.todo-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 4px auto;
  color: #fff;
  background: linear-gradient(90deg, rgba(255, 118, 20, 1) 0%, rgba(255, 84, 17, 1) 100%);
  padding: 16px;
  border-radius: 5px;
  width: 90%;
}
.complete {
  text-decoration: line-through;
  opacity: 0.4;
}
```

**App.jsx**

```javascript
import './App.css';
import React, { useState } from 'react';

export default function App() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'JavaScript', completed: false },
    { id: 2, text: 'PHP', completed: true },
    { id: 3, text: 'Go', completed: true }
  ]);
  return (
    <>
      {todos.map((todo) => {
        return (
          <div key={todo.id} className={todo.completed ? 'todo-row complete' : 'todo-row'}>
            {todo.text}
          </div>
        );
      })}
    </>
  );
}
```

b) condition to add a `className` value or no:

**App.jsx**

```javascript
import './App.css';
import React, { useState } from 'react';

export default function App() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'JavaScript', completed: false },
    { id: 2, text: 'PHP', completed: true },
    { id: 3, text: 'Go', completed: true }
  ]);
  return (
    <>
      {todos.map((todo) => {
        return (
          <div key={todo.id} className={'todo-row ' + (todo.completed ? 'complete' : '')}>
            {todo.text}
          </div>
        );
      })}
    </>
  );
}
```

c) condition to add a `className` value or no, with template literals:

**App.jsx**

```javascript
import './App.css';
import React, { useState } from 'react';

export default function App() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'JavaScript', completed: false },
    { id: 2, text: 'PHP', completed: true },
    { id: 3, text: 'Go', completed: true }
  ]);
  return (
    <>
      {todos.map((todo) => {
        return (
          <div key={todo.id} className={`todo-row ${todo.completed ? 'complete' : ''}`}>
            {todo.text}
          </div>
        );
      })}
    </>
  );
}
```

d) condition on which to display the array `users` or not:

**User.jsx**

```javascript
export default function User({ users }) {
  return (
    <div>
      {users.length > 0 ? (
        users.map((user) => (
          <div key={user.id}>
            <div>
              {user.name} - {user.email}
            </div>
          </div>
        ))
      ) : (
        <h4>No Users!</h4>
      )}
    </div>
  );
}
```

e) condition with `fetching data` from api:

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  return (
    <div className='container'>
      <h2>Posts:</h2>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <ul>
          {data &&
            data.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
              </li>
            ))}
        </ul>
      )}
    </div>
  );
}
```

### 2. Using a `if` statement, with fetching data from api:

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p style={{ color: 'red' }}>Error: {error.message}</p>;

  return (
    <div className='container'>
      <h2>Posts:</h2>
      <ul>
        {data &&
          data.map((post) => (
            <li key={post.id}>
              {post.id}. {post.title}
            </li>
          ))}
      </ul>
    </div>
  );
}
```

### 3. With the classname library:

Condition with classname library `npm install classnames`

**Todo.css**

```css
.completed {
  text-decoration: line-through;
  opacity: 0.4;
}
```

**Todo.jsx**

```javascript
import './Todo.css';
import classNames from 'classnames';

export default function Todo({ task, isCompleted }) {
  const itemClasses = classNames('task-item', {
    completed: isCompleted
  });
  return <div className={itemClasses}>{task}</div>;
}
```

## II. Destructuring of props

### 1. Destructuring during their transfer:

**PostItem.jsx**

```javascript
import React from 'react';

export default function PostItem({ id, label, country }) {
  return (
    <div className='container'>
      <ul>
        <li>
          {id} - {label} - {country}
        </li>
      </ul>
    </div>
  );
}
```

### 2. Destructuring inside a component:

**PostItem.jsx**

```javascript
import React from 'react';

export default function PostItem(props) {
  const { id, label, country } = props;
  return (
    <div className='container'>
      <ul>
        <li>
          {id} - {label} - {country}
        </li>
      </ul>
    </div>
  );
}
```

## III. Spread Operator in React

### 1. Passing props formed with `map()` and without `spread operator`:

**PostList.jsx**

```javascript
import React from 'react';

export default function PostList(props) {
  return (
    <div className='container'>
      <ul>
        {props.posts.map((post) => {
          return (
            <li key={post.id}>
              <PostListItem id={post.id} firstName={post.firstName} title={post.title} />
            </li>
          );
        })}
      </ul>
    </div>
  );
}
```

### 2. Passing props formed with `map()` and `spread operator`:

**PostList.jsx**

```javascript
import React from 'react';

export default function PostList(props) {
  return (
    <div className='container'>
      <ul>
        {props.posts.map((post) => {
          return (
            <li key={post.id}>
              <PostListItem {...post} />
            </li>
          );
        })}
      </ul>
    </div>
  );
}
```

### 3. Using `spread operator` without changing the original array:

**App.jsx**

```javascript
import React, { useState } from 'react';

export default function App() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'JavaScript', completed: false },
    { id: 2, text: 'PHP', completed: true }
  ]);

  const remove = (id) => {
    const newTodos = [...todos].filter((todo) => todo.id !== id);
    setTodos(newTodos);
  };
  return (
    <>
      {todos.map((todo) => {
        return (
          <div key={todo.id}>
            {todo.text}
            <div>
              <button onClick={() => remove(todo.id)}>Remove</button>
            </div>
          </div>
        );
      })}
    </>
  );
}
```

### 4. Using `spread operator` without changing the original object:

**App.jsx**

```javascript
import React, { useState } from 'react';

export default function App() {
  const [car, setCar] = useState({
    brand: 'Ford',
    model: 'Mustang',
    year: '1964',
    color: 'red'
  });

  const updateColor = () => {
    setCar(() => ({ ...car, color: 'blue' }));
  };

  return (
    <div>
      <p>Brand: {car.brand}</p>
      <p>Model: {car.model}</p>
      <p>Color {car.color}</p>
      <button onClick={updateColor}>Change color</button>
    </div>
  );
}
```

## IV. Handling event

### 1. Single input with handelChange function

**Form.jsx**

```javascript
import React, { useState } from 'react';

const Form = ({ addTask }) => {
  const [task, setTask] = useState('');

  const handleChange = (e) => {
    setTask(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!task || /^\s*$/.test(task)) return;
    addTask(task);
    setTask('');
  };
  return (
    <>
      <form onSubmit={handleSubmit} className='todo-form'>
        <input type='text' placeholder='Add a task' name='task' value={task} onChange={handleChange} required />
        <button>Add</button>
      </form>
    </>
  );
};
export default Form;
```

**Todos.jsx**

```javascript
import { v4 as uuidv4 } from 'uuid';

function Todos({ todos }) {
  return (
    <>
      <h2>My Todos</h2>
      {todos.map((todo) => {
        return <p key={uuidv4()}>{todo}</p>;
      })}
    </>
  );
}
export default Todos;
```

**App.jsx**

```javascript
import './App.css';
import { useState } from 'react';
import Todos from './Todos';
import Form from './Form';

function App() {
  const [todos, setTodos] = useState(['todo 1', 'todo 2']);

  const addTask = (newTodo) => {
    setTodos([...todos, newTodo]);
  };

  return (
    <>
      <Form addTask={addTask} />
      <Todos todos={todos} />
    </>
  );
}
export default App;
```

### 2. Single input without handleChange function

**Form.jsx**

```javascript
import React, { useState } from 'react';

const Form = ({ addTask }) => {
  const [task, setTask] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!task || /^\s*$/.test(task)) return;
    addTask(task);
    setTask('');
  };
  return (
    <>
      <form onSubmit={handleSubmit} className='todo-form'>
        <input type='text' placeholder='Add a task' name='task' value={task} onChange={(e) => setTask(e.target.value)} required />
        <button>Add</button>
      </form>
    </>
  );
};
export default Form;
```

**Todos.jsx**

```javascript
import { v4 as uuidv4 } from 'uuid';

function Todos({ todos }) {
  return (
    <>
      <h2>My Todos</h2>
      {todos.map((todo) => {
        return <p key={uuidv4()}>{todo}</p>;
      })}
    </>
  );
}
export default Todos;
```

**App.jsx**

```javascript
import './App.css';
import { useState } from 'react';
import Todos from './Todos';
import Form from './Form';

function App() {
  const [todos, setTodos] = useState(['todo 1', 'todo 2']);

  const addTask = (newTodo) => {
    setTodos([...todos, newTodo]);
  };

  return (
    <>
      <Form addTask={addTask} />
      <Todos todos={todos} />
    </>
  );
}
export default App;
```

### 3. Multiple input with handleChange function

**Form.jsx**

```javascript
import React, { useState } from 'react';
import { v4 as uuidv4 } from 'uuid';

const Form = ({ addUser }) => {
  const [user, setUser] = useState({ id: 0, firstName: '', lastName: '' });

  const handleChange = (e) => {
    setUser({ ...user, id: uuidv4(), [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!user.firstName || !user.lastName || /^\s*$/.test(user.firstName) || /^\s*$/.test(user.lastName)) return;
    addUser(user);
    setUser({ id: 0, firstName: '', lastName: '' });
  };

  return (
    <>
      <form onSubmit={handleSubmit} className='todo-form'>
        <input type='text' placeholder='firstName' name='firstName' value={user.firstName} onChange={handleChange} required />
        <input type='text' placeholder='lastName' name='lastName' value={user.lastName} onChange={handleChange} required />
        <button>Add</button>
      </form>
    </>
  );
};
export default Form;
```

**Users.jsx**

```javascript
function Users({ users }) {
  return (
    <>
      <h2>Users</h2>
      {users.map((user) => {
        return (
          <p key={user.id}>
            {user.firstName}-{user.lastName}
          </p>
        );
      })}
    </>
  );
}
export default Users;
```

**App.jsx**

```javascript
import './App.css';
import { useState } from 'react';
import Users from './Users';
import Form from './Form';

function App() {
  const [users, setUsers] = useState([{ id: 1, firstName: 'John', lastName: 'Depo' }]);

  const addUser = (newUser) => {
    setUsers([...users, newUser]);
  };

  return (
    <>
      <Form addUser={addUser} />
      <Users users={users} />
    </>
  );
}
export default App;
```

## V. Auto focus on input field

**App.jsx**

```javascript
import React, { useRef, useEffect } from 'react';

export default function App() {
  const textInput = useRef(null);

  useEffect(() => {
    textInput.current.focus();
  }, []);

  return (
    <div>
      <input type='text' ref={textInput} />
    </div>
  );
}
```

## VI. Infinite scrolling with `react-infinite-scroll-component`

**App.css**

```css
.todo-row {
  display: flex;
  align-items: center;
  margin: 4px auto;
  color: #fff;
  background: linear-gradient(90deg, rgba(255, 118, 20, 1) 0%, rgba(255, 84, 17, 1) 100%);
  padding: 16px;
  border-radius: 5px;
}
.container {
  max-width: 650px;
  margin: 0px auto;
  text-align: center;
}
```

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import InfiniteScroll from 'react-infinite-scroll-component';
import axios from 'axios';
import './App.css';

export default function App() {
  const [pokemonList, setPokemonList] = useState([]);
  const [hasMore, setHasMore] = useState(true);
  const [offset, setOffset] = useState(10);

  const fetchPokemon = () => {
    const limit = 20;
    axios
      .get(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${limit}`)
      .then((res) => {
        const data = res.data.results;
        setPokemonList((prevList) => [...prevList, ...data]);
        setOffset((prevOffset) => prevOffset + limit);
        setHasMore(data.length === limit);
      })
      .catch((err) => console.error(err));
  };

  useEffect(() => {
    fetchPokemon();
  }, []);

  return (
    <div className='container'>
      <InfiniteScroll dataLength={pokemonList.length} next={fetchPokemon} hasMore={hasMore} loader={<h2>Loading...</h2>}>
        {pokemonList.map((pokemon, index) => (
          <div key={index} className='todo-row'>
            {index + 1}. {pokemon.name} - {pokemon.url}
          </div>
        ))}
      </InfiniteScroll>
    </div>
  );
}
```

## VII. Custom hooks

### 1. useDebounce hook

**useDebounce.jsx**

```javascript
import { useState, useEffect } from 'react';

export default function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timeoutId = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    return () => {
      clearTimeout(timeoutId);
    };
  }, [value, delay]);
  return debouncedValue;
}
```

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import Search from './Search';
import PostItem from './PostItem';
import useDebounce from './hooks/useDebounce';
import './App.css';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [search, setSearch] = useState('');
  const debouncedSearch = useDebounce(search, 500);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const updateSearch = (text) => {
    setSearch(text);
  };

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <>
          <h2>Posts:</h2>
          <Search updateSearch={updateSearch} />
          <ul>
            {data
              .filter((item) => {
                return debouncedSearch.toLowerCase() === '' ? item : item.title.toLowerCase().includes(debouncedSearch) || item.body.toLowerCase().includes(debouncedSearch);
              })
              .map((post) => (
                <PostItem key={post.id} post={post} />
              ))}
          </ul>
        </>
      )}
    </div>
  );
}
```

### 2. useCount hook

**useCount.jsx**

```javascript
import { useState } from 'react';

const useCount = (initialVal = 0) => {
  const [count, setCount] = useState(initialVal);

  const increase = () => {
    setCount((prev) => prev + 1);
  };
  const decrease = () => {
    setCount((prev) => prev - 1);
  };
  const restart = () => {
    setCount(0);
  };
  return { count, increase, decrease, restart };
};
export default useCount;
```

**Count.jsx**

```javascript
import useCount from '../hooks/useCount';

function Count() {
  const { count, increase, decrease, restart } = useCount();

  return (
    <>
      <h3>{count}</h3>
      <button onClick={increase}>Increase</button>
      <button onClick={decrease}>Decrease</button>
      <button onClick={restart}>Restart</button>
    </>
  );
}

export default Count;
```

### 3. useLocalStorage hook

**useLocalStorage.jsx**

```javascript
import { useState, useEffect } from 'react';

export default function useLocalStorage(key, initialValue) {
  const storedValue = localStorage.getItem(key);
  const initial = storedValue ? JSON.parse(storedValue) : initialValue;
  const [value, setValue] = useState(initial);

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}
```

**App.jsx**

```javascript
import React from 'react';
import { initialTodos } from './initialsTodo';
import './App.css';
import useLocalStorage from './hooks/useLocalStorage';

export default function App() {
  const [todos, setTodos] = useLocalStorage('todos', initialTodos);

  const add = (newTodo) => {
    const newTodos = [...todos, newTodo];
    setTodos(newTodos);
  };

  return (
    <div className='container'>
      {todos.map((todo) => (
        <div key={todo.id} className='todo-row'>
          {todo.id}. {todo.text} -<button onClick={() => add({ id: Date.now(), text: 'GO', completed: true })}>Add</button>
        </div>
      ))}
    </div>
  );
}
```

### 4. useReducer hook

**useReducer.jsx**

```javascript
export default function useReducer(todos, action) {
  switch (action.type) {
    case 'delete_todo':
      return todos.filter((todo) => todo.id !== action.payload);
    case 'toggle_todo':
      return todos.map((todo) => {
        if (todo.id === action.payload) {
          return { ...todo, complete: !todo.complete };
        }
        return todo;
      });

    default:
      return todos;
  }
}
```

**App.jsx**

```javascript
import './App.css';
import { useReducer } from 'react';
import reducer from './reducer';

function App() {
  const initialState = [
    { id: 1, title: 'Wash dishes', complete: false },
    { id: 2, title: 'Study JS', complete: false },
    { id: 3, title: 'Buy ticket', complete: false }
  ];
  const [todos, dispatch] = useReducer(reducer, initialState);
  return (
    <div className='app'>
      {todos.map((todo) => (
        <>
          <div style={{ color: todo.complete ? ' rgb(7, 245, 7)' : '#0000FF' }}>{todo.title}</div>
          <div>
            <button
              onClick={() => {
                dispatch({ type: 'toggle_todo', payload: todo.id });
              }}
            >
              Toggle
            </button>
            <button
              onClick={() => {
                dispatch({ type: 'delete_todo', payload: todo.id });
              }}
            >
              Delete
            </button>
          </div>
        </>
      ))}
    </div>
  );
}

export default App;
```

## VIII. Generation of random and unique keys

**install:**

```javascript
npm install uuid
```

**App.jsx**

```javascript
import React, { useState } from 'react';
import { initialTodos } from './initialsTodo';
import './App.css';
import { v4 as uuidv4 } from 'uuid';

export default function App() {
  const [todos, setTodos] = useState(initialTodos);
  const add = (newTodo) => {
    const newTodos = [...todos, newTodo];
    setTodos(newTodos);
  };

  return (
    <div className='container'>
      {todos.map((todo) => (
        <div key={todo.id} className='todo-row'>
          {todo.id}. {todo.text}- <button onClick={() => add({ id: uuidv4(), text: 'GO', completed: true })}>add</button>
        </div>
      ))}
    </div>
  );
}
```

## IX. Search in React

### 1. With filter and map functions

**App.css**

```css
.container {
  max-width: 650px;
  margin: 0px auto;
  text-align: center;
}
ul {
  list-style-type: none;
}
.search {
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
}
.row {
  display: flex;
  align-items: center;
  margin: 4px auto;
  color: #fff;
  background: linear-gradient(90deg, rgba(65, 5, 159, 1) 48%, rgba(98, 0, 255, 1) 100%);
  padding: 16px;
  border-radius: 5px;
}
```

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import Search from './Search';
import PostItem from './PostItem';
import './App.css';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [search, setSearch] = useState('');

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const updateSearch = (text) => {
    setSearch(text);
  };

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <>
          <h2>Posts:</h2>
          <Search updateSearch={updateSearch} />
          <ul>
            {data
              .filter((item) => {
                return search.toLowerCase() === '' ? item : item.title.toLowerCase().includes(search) || item.body.toLowerCase().includes(search);
              })
              .map((post) => (
                <PostItem key={post.id} post={post} />
              ))}
          </ul>
        </>
      )}
    </div>
  );
}
```

**Search.jsx**

```javascript
export default function Search({ updateSearch }) {
  const onUpdate = (e) => {
    const text = e.target.value;
    updateSearch(text);
  };
  return (
    <div className='search'>
      <input type='text' placeholder='search' onChange={onUpdate} />
    </div>
  );
}
```

**PostItem.jsx**

```javascript
export default function PostItem({ post }) {
  return (
    <div className='row'>
      <li>
        <h4>
          {post.id}. {post.title}
        </h4>
        <p> {post.body} </p>
      </li>
    </div>
  );
}
```

### 2. Filtering result with map function

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [search, setSearch] = useState('');

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const availableData = search ? data.filter((item) => item.title.toLowerCase().includes(search) || item.body.toLowerCase().includes(search)) : data;

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <>
          <h2>Posts:</h2>
          <input type='text' onChange={(e) => setSearch(e.target.value)} />
          <ul>
            {availableData.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
                <p> {post.body} </p>
              </li>
            ))}
          </ul>
        </>
      )}
    </div>
  );
}
```

### 3. With useFilter and useDebounce hooks

**useDebounce.jsx**

```javascript
import { useState, useEffect } from 'react';

export function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timeoutId = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(timeoutId);
    };
  }, [value, delay]);

  return debouncedValue;
}
```

**useFilter.jsx**

```javascript
import { useState } from 'react';
import { useDebounce } from './useDebounce';

export function useFilter(data, filterProp1, filterProp2) {
  const [search, setSearch] = useState('');
  const debouncedSearch = useDebounce(search, 300);

  const availableData = debouncedSearch
    ? data.filter((item) => item[filterProp1].toLowerCase().includes(debouncedSearch) || item[filterProp2].toLowerCase().includes(debouncedSearch))
    : data;

  return {
    search,
    setSearch,
    availableData
  };
}
```

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { useFilter } from './hooks/useFilter';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const { search, setSearch, availableData } = useFilter(data, 'title', 'body');

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <>
          <h2>Posts:</h2>
          <input type='text' value={search} onChange={(e) => setSearch(e.target.value)} />
          <ul>
            {availableData.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
                <p> {post.body} </p>
              </li>
            ))}
          </ul>
        </>
      )}
    </div>
  );
}
```

## X. Search with sort

### 1. Without hooks

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [search, setSearch] = useState('');
  const [sortMode, setSortMode] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const availableData = search ? data.filter((item) => item.title.toLowerCase().includes(search) || item.body.toLowerCase().includes(search)) : data;

  const sortedData = !sortMode
    ? availableData
    : availableData.slice().sort((a, b) => {
        if (sortMode === 'asc' && a.id > b.id) {
          return 1;
        } else if (sortMode === 'asc') {
          return -1;
        } else if (sortMode === 'desc' && a.id > b.id) {
          return -1;
        } else {
          return 1;
        }
      });

  if (loading) return <p>Loading...</p>;
  if (error) return <p style={{ color: 'red' }}>Error: {error.message}</p>;

  return (
    <div className='container'>
      <input type='text' onChange={(e) => setSearch(e.target.value)} />
      <div className='form-radio'>
        <input type='radio' name='sort' value='asc' checked={sortMode === 'asc'} onChange={(e) => setSortMode(e.target.value)} /> A-Z
        <input type='radio' name='sort' value='desc' checked={sortMode === 'desc'} onChange={(e) => setSortMode(e.target.value)} /> Z-A
      </div>

      <ul>
        {sortedData.map((post) => (
          <li key={post.id}>
            {post.id}. {post.title}
            <p> {post.body} </p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### 2. With useDebounce, useFilter and useSort hooks

**useDebounce.jsx**

```javascript
import { useState, useEffect } from 'react';

export function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timeoutId = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => {
      clearTimeout(timeoutId);
    };
  }, [value, delay]);

  return debouncedValue;
}
```

**useFilter.jsx**

```javascript
import { useState } from 'react';
import { useDebounce } from './useDebounce';

export function useFilter(data, filterProp1, filterProp2) {
  const [search, setSearch] = useState('');
  const debouncedSearch = useDebounce(search, 300);

  const availableData = debouncedSearch
    ? data.filter((item) => item[filterProp1].toLowerCase().includes(debouncedSearch) || item[filterProp2].toLowerCase().includes(debouncedSearch))
    : data;

  return {
    search,
    setSearch,
    availableData
  };
}
```

**useSort.jsx**

```javascript
import { useState } from 'react';

export function useSort(items, sortProp) {
  const [sortMode, setSortMode] = useState(null);

  const sortedData = !sortMode
    ? items
    : items.slice().sort((a, b) => {
        if (sortMode === 'asc' && a[sortProp] > b[sortProp]) {
          return 1;
        } else if (sortMode === 'asc') {
          return -1;
        } else if (sortMode === 'desc' && a[sortProp] > b[sortProp]) {
          return -1;
        } else {
          return 1;
        }
      });

  return {
    sortMode,
    setSortMode,
    sortedData
  };
}
```

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { useFilter } from './hooks/useFilter';
import { useSort } from './hooks/useSort';

export default function App() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setData(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  const { search, setSearch, availableData } = useFilter(data, 'title', 'body');
  const { sortMode, setSortMode, sortedData } = useSort(availableData, 'id');

  if (loading) return <p>Loading...</p>;
  if (error) return <p style={{ color: 'red' }}>Error: {error.message}</p>;

  return (
    <div className='container'>
      <input type='text' value={search} onChange={(e) => setSearch(e.target.value)} />
      <div className='form-radio'>
        <input type='radio' name='sort' value='asc' checked={sortMode === 'asc'} onChange={(e) => setSortMode(e.target.value)} />
        A-Z
        <input type='radio' name='sort' value='desc' checked={sortMode === 'desc'} onChange={(e) => setSortMode(e.target.value)} />
        Z-A
      </div>
      <ul>
        {sortedData.map((post) => (
          <li key={post.id}>
            {post.id}. {post.title}
            <p> {post.body} </p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

## XI. React Router v6

### 1. Without a separate routes component

**App.jsx**

```javascript
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import './App.css';
import Header from './Header';
import Home from './pages/Home';
import Posts from './pages/Posts';
import PostItem from './pages/PostItem';
import Contact from './pages/Contact';
import NotFound from './pages/NotFound';
import Footer from './Footer';

function App() {
  return (
    <>
      <Router>
        <Header />
        <Routes>
          <Route path='/' element={<Home />} />
          <Route path='/posts' element={<Posts />} />
          <Route path='/posts/:id' element={<PostItem />} />
          <Route path='/contact' element={<Contact />} />
          <Route path='*' element={<NotFound />} />
        </Routes>
        <Footer />
      </Router>
    </>
  );
}

export default App;
```

### 2. With a separate routes component

**AppRouter.jsx**

```javascript
import React from 'react';
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Posts from './pages/Posts';
import PostItem from './pages/PostItem';
import Contact from './pages/Contact';
import NotFound from './pages/NotFound';

const AppRouter = () => {
  return (
    <Routes>
      <Route path='/' element={<Home />} />
      <Route path='/posts' element={<Posts />} />
      <Route path='/posts/:id' element={<PostItem />} />
      <Route path='/contact' element={<Contact />} />
      <Route path='*' element={<NotFound />} />
    </Routes>
  );
};
export default AppRouter;
```

**App.jsx**

```javascript
import { BrowserRouter as Router } from 'react-router-dom';
import './App.css';
import Header from './Header';
import AppRouter from './AppRouter';
import Footer from './Footer';

function App() {
  return (
    <Router>
      <Header />
      <AppRouter />
      <Footer />
    </Router>
  );
}

export default App;
```

## XII. React Paginate

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import ReactPaginate from 'react-paginate';
import axios from 'axios';
import './App.css';

function App() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);

  const PER_PAGE = 15;

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setPosts(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  function handlePageClick(e) {
    setCurrentPage(e.selected);
  }

  const offset = currentPage * PER_PAGE;

  const currentPageData = posts.slice(offset, offset + PER_PAGE).map((item) => (
    <p key={item.id}>
      {item.id}. {item.title}
    </p>
  ));

  const pageCount = Math.ceil(posts.length / PER_PAGE);

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <>
          <h3>React Paginate Example</h3>
          {currentPageData}
          <ReactPaginate
            previousLabel={'prev'}
            nextLabel={'next'}
            breakLabel={'...'}
            breakClassName={'break-me'}
            pageCount={pageCount}
            marginPagesDisplayed={2}
            pageRangeDisplayed={5}
            onPageChange={handlePageClick}
            containerClassName={'pagination'}
            subContainerClassName={'pages pagination'}
            activeClassName={'active'}
            disableClassName={'disable'}
          />
        </>
      )}
    </div>
  );
}
export default App;
```

## XIII. Stars rating

**Star.jsx**

```javascript
import { FaStar } from 'react-icons/fa';

const Star = ({ selected = false, onSelect }) => <FaStar color={selected ? 'red' : 'grey'} onClick={onSelect} />;

export default Star;
```

**StarRating.jsx**

```javascript
import Star from './Star';
import { useState } from 'react';

function StarRating({ totalStars = 10 }) {
  const [selectedStars, setSelectedStars] = useState(0);
  const createArray = (length) => [...Array(length)];

  return (
    <div style={{ padding: '10px' }}>
      {createArray(totalStars).map((n, i) => (
        <Star key={i} selected={selectedStars > i} onSelect={() => setSelectedStars(i + 1)} />
      ))}
      <p>
        {selectedStars} of {totalStars} stars
      </p>
    </div>
  );
}
export default StarRating;
```

**App.jsx**

```javascript
import './App.css';
import StarRating from './StarRating';

function App() {
  return <StarRating />;
}
export default App;
```

## XIV. Asynchronous methods in React

### 1. With fetch

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';

export default function App() {
  const [posts, setPosts] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = () => {
      fetch('https://jsonplaceholder.typicode.com/posts')
        .then((response) => {
          if (!response.ok) {
            throw new Error('Server returned an error');
          }
          return response.json();
        })
        .then((jsonData) => {
          setPosts(jsonData);
          setLoading(false);
        })
        .catch((error) => {
          setError(error);
          setLoading(false);
        });
    };

    fetchData();
  }, []);

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <ul>
          <h2>Posts:</h2>
          {posts &&
            posts.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
              </li>
            ))}
        </ul>
      )}
    </div>
  );
}
```

### 2. With axios

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [posts, setPosts] = useState();
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
        setPosts(res.data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <ul>
          <h2>Posts:</h2>
          {posts &&
            posts.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
              </li>
            ))}
        </ul>
      )}
    </div>
  );
}
```

### 3. With axios and typescript

**App.tsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios, { AxiosError } from 'axios';

interface Post {
  id: number;
  title: string;
}

const App: React.FC = () => {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<AxiosError | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get<Post[]>('https://jsonplaceholder.typicode.com/posts');
        setPosts(response.data);
      } catch (error: any) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  return (
    <div className='container'>
      {loading ? (
        <h3>Loading...</h3>
      ) : error ? (
        <h3 style={{ color: 'red' }}>Error: {error.message}</h3>
      ) : (
        <ul>
          <h2>Posts:</h2>
          {posts &&
            posts.map((post) => (
              <li key={post.id}>
                {post.id}. {post.title}
              </li>
            ))}
        </ul>
      )}
    </div>
  );
};

export default App;
```
## XV. Asynchronous methods with POST in React

### 1. With fetch

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';

export default function App() {
 const [data, setData] = useState(null); 
  const [loading, setLoading] = useState(true); 
  const [error, setError] = useState(null); 

  useEffect(() => {
    const createUser = async () => {
      const url = 'https://example.com/api/users';
      const userData = {
        name: 'John Doe',
        email: 'john.doe@example.com',
      };
      try {
        setLoading(true); 
        const response = await fetch(url, {
          method: 'POST', 
          headers: {
            'Content-Type': 'application/json', 
          },
          body: JSON.stringify(userData), 
        });
        if (!response.ok) {
          throw new Error(`Server error: ${response.status}`);
        }
        const responseData = await response.json(); 
        setData(responseData); 
      } catch (error) {
        setError(error.message); 
      } finally {
        setLoading(false); 
      }
    };
    createUser(); 
  }, []); 

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>New user</h1>
      {data && (
        <div>
          <p>ID: {data.id}</p>
          <p>Name: {data.name}</p>
          <p>Email: {data.email}</p>
        </div>
      )}
    </div>
  );
}
```

### 2. With axios

**App.jsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [data, setData] = useState(null); 
  const [loading, setLoading] = useState(true); 
  const [error, setError] = useState(null); 

  useEffect(() => {
    const createUser = async () => {
      const url = 'https://example.com/api/users';
      const userData = {
        name: 'John Doe',
        email: 'john.doe@example.com',
      };
      try {
        setLoading(true); 
        const response = await axios.post(url, userData, {
          headers: {
            'Content-Type': 'application/json'
          }
        });
        setData(response.data); 
      } catch (error) {
        if (error.response) {
          setError(`Server error: ${error.response.status} - ${error.response.data}`);
        } else {
          setError(`Network error: ${error.message}`);
        }
      } finally {
        setLoading(false); 
      }
    };
    createUser(); 
  }, []); // 

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>New user</h1>
      {data && (
        <div>
          <p>ID: {data.id}</p>
          <p>Name: {data.name}</p>
          <p>Email: {data.email}</p>
        </div>
      )}
    </div>
  );
}
```

### 3. With axios and typescript

**App.tsx**

```javascript
import React, { useState, useEffect } from 'react';
import axios, { AxiosError } from 'axios';

interface User {
  id: number;
  name: string;
  email: string;
}

const App: React.FC = () => {
  const [data, setData] = useState<User | null>(null); 
  const [loading, setLoading] = useState<boolean>(true); 
  const [error, setError] = useState<AxiosError | null>(null); 

  useEffect(() => {
    const createUser = async () => {
      const url = 'https://example.com/api/users'; // API URL
      const userData = {
        name: 'John Doe',
        email: 'john.doe@example.com',
      };

      try {
        setLoading(true); 
        const response = await axios.post<User>(url, userData, { 
          headers: {
            'Content-Type': 'application/json', 
          },
        });
        setData(response.data); 
      } catch (error) {
        if (axios.isAxiosError(error)) {
          setError(error); 
        } else {
          setError(null); 
          console.error(`Network error: ${error.message}`);
        }
      } finally {
        setLoading(false); 
      }
    };
    createUser(); 
  }, []); 

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>; 

  return (
    <div>
      <h1>New user</h1>
      {data && (
        <div>
          <p>ID: {data.id}</p>
          <p>Name: {data.name}</p>
          <p>Email: {data.email}</p>
        </div>
      )}
    </div>
  );
};

export default App;
```

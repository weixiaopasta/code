https://blog.csdn.net/qq449245884/article/details/114007673?spm=1001.2014.3001.5501
### 1.useFetch
获取数据是我每次创建React应用时都会做的事情。我甚至在一个应用程序中进行了好多个这样的重复获取。

不管我们选择哪种方式来获取数据，Axios、Fetch API，还是其他，我们很有可能在React组件序中一次又一次地编写相同的代码。

```
import { useState, useEffect } from 'react';

const useFetch = (url = '', options = null) => {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    let isMounted = true;

    setLoading(true);

    fetch(url, options)
      .then(res => res.json())
      .then(data => {
        if (isMounted) {
          setData(data);
          setError(null);
        }
      })
      .catch(error => {
        if (isMounted) {
          setError(error);
          setData(null);
        }
      })
      .finally(() => isMounted && setLoading(false));

    return () => (isMounted = false);
  }, [url, options]);

  return { loading, error, data };
};

export default useFetch;

```

接下就是怎么用了？

我们只需要传递我们想要检索的资源的URL。从那里，我们得到一个对象，我们可以使用它来渲染我们的应用程序。
```
import useFetch from './useFetch';

const App = () => {
  const { loading, error, data = [] } = useFetch(
    'https://hn.algolia.com/api/v1/search?query=react'
  );

  if (error) return <p>Error!</p>;
  if (loading) return <p>Loading...</p>;

  return (
    <div>
      <ul>
        {data?.hits?.map(item => (
          <li key={item.objectID}>
            <a href={item.url}>{item.title}</a>
          </li>
        ))}
      </ul>
    </div>
  );
};
```

### useLocalStorage
```
import { useState } from 'react';

const useLocalStorage = (key = '', initialValue = '') => {
  const [state, setState] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setLocalStorageState = newState => {
    try {
      const newStateValue =
        typeof newState === 'function' ? newState(state) : newState;
      setState(newStateValue);
      window.localStorage.setItem(key, JSON.stringify(newStateValue));
    } catch (error) {
      console.error(`Unable to store new value for ${key} in localStorage.`);
    }
  };

  return [state, setLocalStorageState];
};

export default useLocalStorage;

```
函数同时更新React状态和 localStorage 中的相应键/值。 这里，我们还可以支持函数更新，例如常规的useState hook。

最后，我们返回状态值和我们的自定义更新函数。

```
import { useLocalStorage } from './hooks';

const defaultSettings = {
  notifications: 'weekly',
};

function App() {
  const [appSettings, setAppSettings] = useLocalStorage(
    'app-settings',
    defaultSettings
  );

  return (
    <div className="h-full w-full flex flex-col justify-center items-center">
      <div className="flex items-center mb-8">
        <p className="font-medium text-lg mr-4">Your application's settings:</p>

        <select
          value={appSettings.notifications}
          onChange={e =>
            setAppSettings(settings => ({
              ...settings,
              notifications: e.target.value,
            }))
          }
          className="border border-gray-900 rounded py-2 px-4 "
        >
          <option value="daily">daily</option>
          <option value="weekly">weekly</option>
          <option value="monthly">monthly</option>
        </select>
      </div>

      <button
        onClick={() => setAppSettings(defaultSettings)}
        className="rounded-md shadow-md py-2 px-6 bg-red-500 text-white uppercase font-medium tracking-wide text-sm leading-8"
      >
        Reset settings
      </button>
    </div>
  );
}

export default App;

```


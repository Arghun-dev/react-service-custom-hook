# react-service-custom-hook

useFetch.js

```js
import { useContext, useState, useEffect } from 'react'
import axios from 'axios'
import { LoginDetailsContext } from '../contexts/LoginDataContext'
import { HandleEventContextDispatch } from '../contexts/HandleEventContext'
import { SUCCESS, ERROR } from '../constants/ActionTypes'

export default function useFetch(url, search, reload, condition) {
    const { token } = useContext(LoginDetailsContext)
    const EventDispatch = useContext(HandleEventContextDispatch)
    const [data, setData] = useState({ isLoading: true, data: null })
    const config = {
        headers: {
            Authorization: `bearer ${token}`
        }
    }

    useEffect(() => {
        if (condition !== undefined) {
            if (condition?.condition !== null) {
                const source = axios.CancelToken.source()
                const loadData = async () => {
                    try {
                        const { data } = await axios.get(url, config, { cancelToken: source.token })
                        setData({
                            isLoading: false,
                            data
                        })
                    } catch (err) {
                        EventDispatch({ type: ERROR, message: err.response.data.errors[0].error })
                    }
                }

                loadData()
                return source.cancel()
            }

        } else if (condition === undefined) {
            const source = axios.CancelToken.source()
            const loadData = async () => {
                try {
                    const { data } = await axios.get(url, config, { cancelToken: source.token })
                    setData({
                        isLoading: false,
                        data
                    })
                } catch (err) {
                    EventDispatch({ type: ERROR, message: err.response.data.errors[0].error })
                }
            }

            loadData()
            return source.cancel()
        }
    }, [url, reload, search])

    return data
}
```


usePost.js

```js
import { useContext, useState, useCallback } from 'react'
import axios from 'axios'
import { LoginDetailsContext } from '../contexts/LoginDataContext'
import { HandleEventContextDispatch } from '../contexts/HandleEventContext'
import { SUCCESS, ERROR } from '../constants/ActionTypes'

export default function usePost({ url, payload, Reload }) {
    const { token } = useContext(LoginDetailsContext)
    const EventDispatch = useContext(HandleEventContextDispatch)
    const [res, setRes] = useState({ data: null, error: null, isLoading: false })
    const config = {
        headers: {
            Authorization: `bearer ${token}`
        }
    }
    // You POST method here
    const callAPI = useCallback(() => {
        setRes(prevState => ({ ...prevState, isLoading: true }))
        axios.post(url, payload, config)
            .then(res => {
                setRes({ data: res.data, isLoading: false, error: null })
                EventDispatch({ type: SUCCESS, message: res.data.message })
                Reload()
            }).catch((err) => {
                setRes({ data: null, isLoading: false })
                EventDispatch({ type: ERROR, message: err.response.data.errors[0].error })
            })
    }, [url, payload])
    return [res, callAPI];
}
```



usePut.js

```js
import { useContext, useState, useCallback } from 'react'
import axios from 'axios'
import { LoginDetailsContext } from '../contexts/LoginDataContext'
import { HandleEventContextDispatch } from '../contexts/HandleEventContext'
import { SUCCESS, ERROR } from '../constants/ActionTypes'

export default function usePut({ url, payload, Reload }) {
    const { token } = useContext(LoginDetailsContext)
    const EventDispatch = useContext(HandleEventContextDispatch)
    const [res, setRes] = useState({ data: null, error: null, isLoading: false })
    const config = {
        headers: {
            Authorization: `bearer ${token}`
        }
    }
    // You POST method here
    const callAPI = useCallback(() => {
        setRes(prevState => ({ ...prevState, isLoading: true }))
        axios.put(url, payload, config)
            .then(res => {
                setRes({ data: res.data, isLoading: false, error: null })
                EventDispatch({ type: SUCCESS, message: res.data.message })
                Reload()
            }).catch((err) => {
                setRes({ data: null, isLoading: false })
                EventDispatch({ type: ERROR, message: err.response.data.errors[0].error })
            })
    }, [url, payload])
    return [res, callAPI];
}
```


useDelete.js

```js
import { useContext, useState, useCallback } from 'react'
import axios from 'axios'
import { LoginDetailsContext } from '../contexts/LoginDataContext'
import { HandleEventContextDispatch } from '../contexts/HandleEventContext'
import { SUCCESS, ERROR } from '../constants/ActionTypes'

export default function useDelete({ url, payload, Reload }) {
    const { token } = useContext(LoginDetailsContext)
    const EventDispatch = useContext(HandleEventContextDispatch)
    const [res, setRes] = useState({ data: null, error: null, isLoading: false })
    const config = {
        headers: {
            Authorization: `bearer ${token}`
        }
    }
    // You DELETE method here
    const callAPI = useCallback(() => {
        setRes(prevState => ({ ...prevState, isLoading: true }))
        axios.delete(url, config, payload)
            .then(res => {
                setRes({ data: res.data, isLoading: false, error: null })
                EventDispatch({ type: SUCCESS, message: res.data.message })
                Reload()
            }).catch((err) => {
                setRes({ data: null, isLoading: false })
                EventDispatch({ type: ERROR, message: err.response.data.errors[0].error })
            })
    }, [url, payload])
    return [res, callAPI];
}
```


How to use them?

useFetch.js

```js
const posts = useFetch(getMainURL)
```

usePost.js

```js
const [res, fnc] = usePost({ url: API, payload, Reload })
```

usePut.js

```js
const [res, fnc] = usePut({ url: API, payload, Reload })
```

useDelete.js

```js
const [res, fnc] = useDelete({ url: API, payload, Reload })
```

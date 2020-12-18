---
title: "리액트에서 Signup Input 을 받는 두 가지 방법"
categories:
 - Django
last_modified_at: 2020-12-18
toc: true
toc_sticky: true
---

### 둘 다 SetState Hook 을 사용함

#### 1. 2개의 useState 를 사용해 처리

```jsx

export default function Signup() {
    const [username, setUsername] = useState("")
    const [password, setPassword] = useState("")

    const onSubmit = (event) => {
        event.preventDefault()
        console.log("onSubmit : ", username, password)
    }

    return (
        <div>
            <form onSubmit={onSubmit}>
                <input
                    type="text"
                    name={"username"}
                    onChange={e=> setUsername(e.target.value)}
                />
                <input
                    type="password"
                    name={"password"}
                    onChange={e=> setPassword(e.target.value)}
                />
                <input type="submit" value={"회원가입"}/>
            </form>
        </div>
    )
}
```

#### 2. 1개의 UseState 로 처리하기

```jsx
import React, {useEffect, useState} from 'react'


export default function Signup() {

    const [inputs, setInputs] = useState({});

    const onSubmit = (event) => {
        event.preventDefault()
        console.log("onSubmit : ", inputs )
    }

    useEffect(() => {
        console.log("changed inputs :", inputs)
    }, [inputs])

    const onChange = (e) => {
        const {name, value} = e.target;
        setInputs(prev => ({
            ...prev,
            [name]: value
            }))
    }

    return (
        <div>
            <form onSubmit={onSubmit}>
                <input
                    type="text"
                    name={"username"}
                    onChange={onChange}
                />
                <input
                    type="password"
                    name={"password"}
                    onChange={onChange}
                />
                <input type="submit" value={"회원가입"}/>
            </form>
        </div>
    )
}
```

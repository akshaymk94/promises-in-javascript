# promises-in-javascript

I test my knowledge of Promises in Javascript with help of ChatGPT through an interactive, challenge based session. ChatGPT provides coding challenges in increasing order of complexity, with subtle hints to help me arrive at an optimised solution.

Let's go through the challenge and attempt to answer the coding questions, one at a time.


### Challenge 1:
 Create a simple promise that resolves after a specific duration (e.g., 2 seconds) and logs a message when it's resolved.

solution:
```javascript

function timer() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (errors.any) {
                const err = new Error('Error occurred!')
                reject(err)
            }
            resolve('Hurray! this promise is resolved')
        }, 2000)
    })
}
```

### Challenge 2:

Create a promise that simulates fetching data from an API. Use fetch or a similar method to make a GET request to a dummy API endpoint (you can use a mock API service like JSONPlaceholder) and resolve the promise with the data when the request is successful. Handle errors if the request fails.

solution:

```javascript
let fetchUserInfo = new Promise((resolve, reject) => {
    fetch('https://api.github.com/users/aksdfsdshaymk94')
        .then((data) => {
            if (data.status === 404) {
                throw new Error('sorry could not find user: ' + data.status)
            } else if (!data.ok) {
                throw new Error('Uncaught error: ' + data.status)
            }
            return data.json();
        })
        .then((data) => {
            resolve(data)
        })
        .catch((error) => {
            reject(error)
        })
});

fetchUserInfo
    .then((data) => {
        console.log(data)
    })
    .catch((error) => {
        console.log(error)
    })
```


### Challenge 3:

Create a promise that simulates multiple asynchronous operations that need to be completed in order.For example, you might simulate fetching user data and then fetching their posts.The second operation should only begin when the first one is complete.Use Promise.all or async / await for this.

```javascript
let fetchUserWithPosts = () => {
    return new Promise((resolve, reject) => {
        Promise.all([
            fetchUser()
                .then((data) => data.json())
                .catch((error) => error),
            fetchPosts()
                .then((posts) => posts.json())
                .catch((error) => error)
        ])
            .then((data) => {
                resolve(data)
            })
            .catch((error) => {
                reject(error)
            })
    })
}
```

### Challenge 4:
Create a promise that simulates handling multiple asynchronous operations that may fail independently. Use Promise.allSettled to handle both resolved and rejected promises and return an array of results indicating whether each operation succeeded or failed.

```javascript
let promise1 = new Promise((resolve) => { resolve('100') })
let promise2 = new Promise((resolve, reject) => { reject('000') })
let promise3 = '50'
let promise4 = false
let allPromises = [promise1, promise2, promise3, promise4]

let myFunc = () => {
    return Promise.allSettled(allPromises)
}
```

### Challenge 5:
Create a promise chain that simulates a series of asynchronous operations that depend on the results of previous ones.
For example, you might fetch a user's profile, then their posts, and finally their comments.
Use async / await to simplify handling these dependent operations.

```javascript
async function getDetails(userId) {
    try {
        let userProfileResponse = await fetch('dummyapi/user/profile')
        if (!userProfileResponse.ok) {
            throw new Error('Failed to fetch User Profile!')
        }
        const userProfile = await userProfileResponse.json()

        let userPostsResponse = await fetch('dummyapi/posts')
        if (!userPostsResponse.ok) {
            throw new Error('Failed to fetch User Posts!')
        }
        const userPosts = await userPostsResponse.json()

        const postIds = userPosts.map(post => post.id)

        let postCommentsResponse = await fetch(`dummyapi/posts/comments/?postIds=${postIds.join(',')}`)
        if (!postCommentsResponse.ok) {
            throw new Error('Failed to fetch Post Comments!')
        }
        const postComments = await postCommentsResponse.json()

        return { userProfile, userPosts, postComments }
    }
    catch (error) {
        console.error(error)
    }
}

getDetails()
```
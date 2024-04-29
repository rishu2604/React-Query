# What is TanStack Query?
- A library that helps with sending HTTP requests & keeping your frontend UI in sync with backend data.
- You can use `useEffect` and `fetch` function to do that, but TanStack Query can vastly simplify your code.
- TanStack query does not come with built-in logic to send HTTP requests. Instead it comes with logic for managing those requests. 
- TanStack Query then manages the data, errors, caching and much more!

## Additional features provided by TanStack Query
- Assume that we changed our tab and when we came back to our previous tab where we were using our react application, then we might want to trigger a refetch.
- Caching data and refetching new data if we found that this data is outdated.   

## `useQuery()`
- This hook will send HTTP requests behind the scenes and get us data that we need and also gives us information about the loading state. So if we are currently sending the request and potential errors.
- We do that be passing an object to `useQuery()`.
- In this object, we can set various properties and one property we can set is the Query function (`queryFn`) property.
- With this function, you define the actual code that will be executed that will send the actual request.
- All `useQuery` wants is a function that returns a promise.
- There is one other property we should add here and that is Query Key (`queryKey`) property.
- Because when using `useQuery`, every query, every fetch request you are sending, so every get HTTP request you are sending in the end also should have such a query key which will then internally be used by React Query (Tanstack Query) to cache the data that's yielded by that request. So that the response from that request could be reused in the future if you are trying to send the same request again and you can configure how long data should be stored and reused by React Query.
- This makes sure that the data can be shown to the user quicker if you have it because it doesn't need to be refetched  all the time.
- And that key is actually an array.
- It is an array of values which are internally stored by React Query such that whenever you are using a similar array of similar values, React Query sees that and is able to reuse existing data.
- In that array we can add a string value, or multiple values or objects or nested arrays or other kinds of values.
- And we get an object back from the `useQuery` function.
- And then we can destructure the object according to our needs.
- *Example:* To pull out the data property, that will exist on that object returned by useQuery, and that will be property which holds the actual response data as a value.
- `isPending`: It is another property which tells us whether the request is currently still on its way or we already get a response. 
- And if we do have a response, it must not necessarily be that data here. Instead, we could aldo be facing an error if something went wrong on the server. And therefore, `useQuery` also gives us an `isError` property in this object here, which will be true if we get back an error response.
- `error`: Contains information about the error that occured.
- `refetch`: A refetch function as it turns out which you can call manually to send the same Query again. *For example:* If the user clicked a button.

## Query Provider (`QueryClientProvider`, `QueryClient`)
- Without using query provider, our app will throw an error because in order to use React Query and the `useQuery` hook, you must wrap the components that you want to use these features with a special provider component provided by Tanstack Query.
- And I'll do that in the App component here.

## Caching using Query Key
- React Query will see that this Query Key has been used before and that it did already cache data for that key. And it will then instantly yield that data, but at the same time, also send this request again Behind the Scenes to see if updated data is available.
- And then it will kind of silently replace that data with the updated data so that after a couple of seconds or however long it takes to fetch that data, we do have the updated data on the screen.
- So that we get the best of both worlds.

## `staleTime`
- You can of course, control if this is the behavior you want.
- For example, by setting a `staleTime` on your queries. This controls after which time React Query will send such a Behind the Scenes request to get updated data if it found data in your cache.
- And the default is zero, which means it will use data from the cache, but it will then always also send such a Behind the Scenes request to get updated data.


## `gcTime`
- Another value you can set here is the gcTime, the Garbage Collection Time.
- This controls how long the data and the cache will be kept around.
- And the default here are five minutes.
- We could of course, also reduce this, for example, to half a minute, with 30,000 milliseconds.

## `enabled`
- It is also a property offered by `useQuery` that enables or disables a query. 
- That means that the request will not be sent instantly if it's false and will check for the given condition and only sends the request when that condition is true.


## `isLoading`
- The difference between `isLoading` and `isPending` is just that it will be false if `enabled` is also false.


## `refetch`
- since we invalidated all queries, React Query went ahead and immediately triggered a refetch for this details query here.
- Now, to avoid this behavior, we should go back to invalidate queries and add a second property to this configuration object for invalidate queries.
- Here, you can set the `refetch` type to `'none'`, which makes sure that when you call `invalidateQueries`, these existing queries will not automatically be triggered again immediately.
- Instead, they will just be invalidated and the next time they are required, they will run again.
- But they will not be re-triggered immediately which otherwise would be the default behavior.
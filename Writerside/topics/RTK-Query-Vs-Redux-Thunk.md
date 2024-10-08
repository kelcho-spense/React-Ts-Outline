# RTK Query Vs Redux Thunk

![RTK-Query.png](RTK-Query.png)

In **redux** we typically model network fetching and caching like this.

* You have a component, which subscribes to data from the redux store via selectors
* In a useEffect hook you check if the data doesn’t exist and then dispatch a thunk
* Your thunk will use a tool like _**fetch**_ or **_axios_** to grab data from over the network
* As it fetches data your thunk will dispatch actions into the redux store such as pending, fulfilled and rejected (error).
* This is complicated to setup and maintain

**RTK Query** provides a better way.

RTK Query generates hooks for you at runtime that allow you to query and cache data or run mutations. Loading and error states are available automatically in your component via the hook and cache invalidation is easily manageable.


This diagram illustrates the difference between handling cached data and fetching network data using **Redux Thunk** and **RTK Query**.

Let's break down each part:

### **Top Left: Invalidating Cached Data (Redux)**
- **With Redux and Thunk**, to invalidate cached data or refresh data, you have to handle it manually:
    - You dispatch an action via a thunk or use `useEffect` in the component to trigger data invalidation.
    - You manually dispatch actions to refetch or invalidate specific data.
    - This requires handling the logic for when and how to refresh the cached data, often involving multiple steps.

### **Top Middle: Using Cached Data (Redux)**
- **With Redux**, when you want to reuse cached data across components:
    - The data is stored in the Redux store.
    - Components access the cached data using **selectors**.
    - If the data is already available in the store, components simply render the cached data without needing to refetch.
    - The challenge is managing cache invalidation and ensuring that data is still up-to-date.

### **Top Right: Fetching & Caching Network Data (Redux + Thunk)**
- In this case:
    - You use a **Thunk** action to fetch data (e.g., with `fetch` or `axios`).
    - You dispatch actions to initiate the request, handle loading, and update the Redux store with the fetched data.
    - **Selectors** are used to access the data in components.
    - Components also use `useEffect` or other hooks to trigger the fetching action at the right time (e.g., on mount or user interaction).
    - This approach works but requires manual management of when to fetch, update, and cache data.

### **Bottom Left: Invalidating Cached Data (RTK Query)**
- **With RTK Query**, invalidating cached data is simplified:
    - RTK Query provides automatic cache management via **tags** and automatic data refetching based on the tags.
    - The component simply interacts with the API layer provided by RTK Query through custom hooks (`useQuery`, `useMutation`), and RTK Query handles the caching and invalidation logic.
    - You don’t need to manually dispatch actions for invalidating or refetching data—RTK Query does it based on its cache policies.

### **Bottom Middle: Using Cached Data (RTK Query)**
- **With RTK Query**, using cached data is seamless:
    - Components use **custom query hooks** (auto-generated by RTK Query) to access data.
    - The data is fetched and cached by RTK Query, and any subsequent renders or components that need the same data will automatically use the cached version.
    - No manual management is needed; RTK Query handles caching internally.

### **Bottom Right: Fetching & Caching Network Data (RTK Query)**
- **RTK Query** abstracts away much of the complexity:
    - RTK Query handles everything: it automatically caches, refetches, and synchronizes data across your components.
    - You simply call the **API via query hooks** in your components, and RTK Query takes care of dispatching actions, fetching data, storing it in Redux, and managing loading and error states.
    - The message "THINGS YOU NO LONGER HAVE TO WORRY ABOUT" in the diagram indicates that RTK Query eliminates the need for manually managing cache invalidation, error handling, and re-fetching logic.

---

### **Summary of the Diagram**
- **Redux with Thunk**: You need to manually handle actions, thunks, cache invalidation, and re-fetching data when needed. It provides flexibility but requires more boilerplate code and management.
- **RTK Query**: Simplifies the data fetching and caching process by automatically managing cache invalidation, re-fetching, and keeping data up-to-date. It provides hooks for fetching and mutating data, while abstracting away many complexities like caching and syncing server state with the Redux store.
## Challenges Faced

### 1. Ensuring React Query Works Properly
One challenge in this project was ensuring that **React Query** worked properly in the application. Since React Query only works in **client-side rendering**, I had to explicitly enable it using `"use client"` at the top of my module.

To solve this, I created a **QueryProvider** that ensures the `QueryClient` is properly initialized when the React app starts. This guarantees that all child components have access to the query client.

#### Solution: Using a Query Provider
```jsx
"use client";

import { QueryClientProvider, QueryClient } from "react-query";
import { useState } from "react";

export default function QueryProvider({ children }) {
    const queryClient = useState(() => new QueryClient());
    return <QueryClientProvider client={queryClient}> {children} </QueryClientProvider>;
}
```
- This ensures that the `QueryClient` is **initialized once** when the app starts.
- It prevents errors where the `QueryClient` was not properly initialized.
- Wrapping the entire app inside `QueryProvider` ensures that any component using `useQuery` will work without issues.

By implementing this approach, I successfully resolved the issue and ensured seamless data fetching using React Query.

# Crypto Dashboard

## Overview
The app also includes state management to handle search functionality, data fetching, and UI updates.

## State Management in the Application
State management in this application is handled using **React's useState** and **React Query (useQuery)**.

### 1. **Search State** (`useState`)
```jsx
const [search, setSearch] = useState("");
```
- This state stores the user input in the search bar.
- As the user types, the state updates and filters the displayed list.
- The filtering is done using:
  ```jsx
  const filteredCoins = data.filter((coin) =>
    coin.name.toLowerCase().includes(search.toLowerCase())
  );
  ```

### 2. **Fetching Crypto Data** (`useQuery` from React Query)
```jsx
const { data, isLoading, isError, refetch, isFetching, isRefetchError, isRefetching } = useQuery({
  queryKey: ["cryptoData"],
  queryFn: getCryptoData,
});
```
- React Query manages asynchronous data fetching.
- It caches results, automatically updates data when dependencies change, and handles refetching.
- **`isLoading`** and **`isError`** states control UI rendering to show a loading spinner or an error message.
- **Refetching** is triggered manually via the refresh button.

### 3. **Manual Data Refresh** (`refetch` from React Query)
```jsx
const handleRefresh = async () => {
  try {
    await refetch();
    setLastRefreshed(new Date());
    setShowPopup(true);
    setTimeout(() => setShowPopup(false), 3000);
    console.log("Data refreshed successfully!");
  } catch (error) {
    console.error("Error during refresh:", error);
  }
};
```
- `refetch()` triggers data fetching again.
- Updates the `lastRefreshed` state to store the refresh timestamp.
- Shows a popup for 3 seconds to indicate successful refresh.

### 4. **Last Refresh Time Tracking** (`useState` + `useEffect`)
```jsx
const [lastRefreshed, setLastRefreshed] = useState(null);
```
- This state stores the timestamp of the last successful refresh.
- A `useEffect` hook updates the displayed time every minute:
  ```jsx
  useEffect(() => {
    const interval = setInterval(() => {
      if (lastRefreshed) {
        setLastRefreshed(new Date(lastRefreshed));
      }
    }, 60000);

    return () => clearInterval(interval);
  }, [lastRefreshed]);
  ```
- Ensures users see how fresh their data is.

### 5. **Popup Notification (`useState`)**
```jsx
const [showPopup, setShowPopup] = useState(false);
```
- Used to display a message when data refresh is successful.
- Controlled using `setShowPopup(true)` and `setTimeout()` to auto-hide after 3 seconds.

## Error Handling
- **Loading state (`isLoading`)**: Displays a loading spinner when data is being fetched.
- **Error state (`isError` and `isRefetchError`)**: Shows an error message if the API request fails.
- **Network error handling**: Ensures the app doesn't crash due to API failures.

## Technologies Used
- **React** for UI components and state management.
- **React Query** for efficient data fetching and caching.
- **CoinGecko API** for real-time cryptocurrency data.
- **Tailwind CSS** for styling.

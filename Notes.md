## Project Setup

* Set up the frontend in VS Code and ran it on `localhost:5173`.
* Set up the backend in IntelliJ IDEA and ran it on `localhost:8080`.

## Bugs / Problems Found

### 1. Search Filter Issue

The major issue I found was with the search filter.

The search was supposed to filter tasks based on:

* `archived = FALSE`
* The search `term` in the task title or description
* `STATUS` selected from the dropdown

However, there was an issue in the native SQL query in `TaskRepository.java` (around lines 14–17).

**Root Cause:**

The query was not grouping the `title` and `description` conditions properly. Since SQL gives higher precedence to `AND` than `OR`, the query logic was not working as intended. As a result, some tasks that did not match the other filters could still appear in the search results.

**Fix:**

I added parentheses around the title/description conditions and grouped them into a single condition. This ensured that the search term is checked against either the title or description while still applying the `archived` and `status` filters correctly.

**AI Tool Used:**

I used ChatGPT to understand why the existing query was producing incorrect results and to understand the correct way to structure the SQL conditions.

---

### 2. Unnecessary Delay in Search

The second issue I found was a noticeable delay whenever a search query was performed.

**Root Cause:**

There was a `Thread.sleep()` implementation in `TaskController.java` that intentionally added a delay to each query.

Interestingly, shorter queries were taking longer than longer queries because the delay was calculated based on the query length. This was unnecessary for the actual application and also caused server threads to remain occupied while waiting.

This could negatively affect the application's throughput, especially when multiple users are making requests at the same time.

**Fix:**

I removed the unnecessary thread/sleep implementation so that search requests are processed normally without an artificial delay.

This improves the user experience and avoids unnecessarily blocking server threads.

**AI Tool Used:**

I used ChatGPT to understand the purpose and impact of the existing implementation and to explore whether there was a better approach. Based on the analysis, removing the artificial delay was the appropriate solution for the current application.

---

### 3. Loading State Issue in React

I also found an issue with the loading state in the React application, specifically in `useTasks.js`.

The `loading` state was initially set to `false` and was changed to `true` inside `useEffect()` when fetching tasks.

**Root Cause:**

The problem was that `setLoading(false)` was not being called after the API request completed, especially when an error occurred. Because of this, the application could remain stuck in the loading state.

**Fix:**

I added a `finally` block to the API request and set:

`setLoading(false)`

This ensures that the loading state is reset whether the API request succeeds or fails.

---

### 4. Small Improvements

I also identified a few areas where the project could be improved further:

* Move the backend API URL into a `.env` file instead of keeping it directly in the frontend code. This would make configuration easier across different environments.
* Lombok could be added to the `Task.java` entity to reduce boilerplate code such as getters, setters, constructors, and other repetitive methods.

These are not critical bugs, but they would improve the maintainability and configuration of the project.

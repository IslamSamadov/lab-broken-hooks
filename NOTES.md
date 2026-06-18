# My Lab Notes

Fill this file in as you work through the lab. Be honest and specific. This file is part of what you hand in.

## What I think is wrong

Write your first impressions here, before asking anyone or any AI for help. Describe what you saw in the browser, in the Console, and in the Network tab. Write down your theory about what is causing each problem.

Problem 1:

When I click a user card to open their posts, the Network tab keeps firing the same `/posts?userId=...` request over and over without stopping. The side panel may flicker between "Loading posts..." and the posts list. My theory: the `useEffect` in `UserDetail.js` has `posts` in its dependency array. When the effect fetches posts and calls `setPosts`, that state change re-triggers the effect, which fetches again — an infinite loop.

Problem 2:

When I click "Add to favorites" on a card, nothing changes on screen — the button text stays the same. My theory: `handleToggleFavorite` mutates the user object in place (`user.favorite = !user.favorite`) and then calls `setUsers(users)` with the same array reference. React compares state by reference, so it does not detect a change and does not re-render.

## What did I ask the AI

"read readme carefully and do the lab"

## What was the solution

For each problem, explain what the actual cause was, which file and which lines you changed, and why your change fixes it.

Problem 1:

**Cause:** In `app/components/UserDetail.js`, the `useEffect` dependency array included both `userId` and `posts`. After fetching, `setPosts(data)` updated `posts`, which re-ran the effect, which fetched again — an endless loop.

**Fix:** Changed the dependency array from `[userId, posts]` to `[userId]` (line 20). The effect should only re-run when the selected user changes, not when posts state updates inside the effect itself.

Problem 2:

**Cause:** In `app/components/UsersExplorer.js`, `handleToggleFavorite` mutated an object inside the `users` array and passed the same array reference back to `setUsers`. React did not see a new value, so the UI did not update.

**Fix:** Replaced the in-place mutation with `setUsers(users.map(...))` that returns a new array and a new object for the toggled user via spread (`{ ...item, favorite: !item.favorite }`). React now detects the state change and re-renders the card with the updated favorite label.

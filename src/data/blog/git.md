---
title: "git: Distributed Version Control System"
description: "A distributed version control system that tracks versions of files."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["git"]
---

## Set up your user info (one-time)

```jsx
git config --global [user.name](http://user.name/) "Your Name"
git config --global user.email "[you@example.com](mailto:you@example.com)"
```

## Check project status

```jsx
git status
```

## Create and edit .gitignore

```jsx
nano .gitignore
```

# Example .gitignore contents

- .log
.DS_Store

## Stage changes

```jsx
git add .
```

## Commit changes

```jsx
git commit -m "Describe what you changed here"
```

## Pull latest changes from remote

```jsx
git pull
```

## Push local commits to remote

```jsx
git push
```
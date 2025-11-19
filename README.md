# NewsFlash – Web-Based News Application

A modern, responsive web application built using Vue 3 and the Ionic Framework that retrieves and displays real‑time news from NewsAPI.org. Users can browse categories, search articles, switch between grid/list views, and bookmark articles – all within a clean, mobile‑friendly web UI.

---

## Features

- Top Headlines (US) – Displays the latest breaking news.
- Category Filtering – Business, Health, Sports, Entertainment, Technology, and more.
- Search Functionality – Filter articles based on title or description.
- Grid & List Views – Switch layouts smoothly.
- Featured Article Slider – Highlights the top five trending articles.
- Bookmarking System – Save articles using LocalStorage.

---

## Tech Stack

- Vue 3 (Composition API)
- Ionic Framework
- TypeScript
- Ionicons
- NewsAPI.org
- LocalStorage

---

## Getting Started

### 1. Clone the project

```
git clone [https://github.com/your-username/your-repo-name](https://github.com/Abiskar0001/newsApp
cd newsApp
```

### 2. Install dependencies

```
npm install
```

### 3. Add your environment variable

Create a `.env` file in the project root:

```
VITE_NEWS_API_KEY=your_api_key_here
```

Vite requires environment variables to begin with `VITE_`.

### 4. Start the development server

```
ionic serve
```

## Environment Variables

Example usage in Vue:

```
const API_KEY = import.meta.env.VITE_NEWS_API_KEY;
```

Ensure `.env` is ignored:

```
.env
```

---

## Project Structure

```
src/
├── components/
│   ├── HeaderComponent.vue
│   ├── HeroSliderComponent.vue
│   ├── NewsGridViewComponent.vue
│   └── NewsListViewComponent.vue
├── pages/
│   └── HomePage.vue
├── assets/
├── router/
└── main.ts
```

---

## How the App Works

### Fetching News

Requests are made to:

```
https://newsapi.org/v2/top-headlines?country=us
```

Optional parameters:

- `category`
- `apiKey`






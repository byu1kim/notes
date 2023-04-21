+++
title = "Tailwind"
weight = 4
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Setup in React

#### Install Tailwind CSS

```
npm install -D tailwindcss
npx tailwindcss init
```

#### Configure your template paths

`tailwind.config.js`

```
content: ["./src/**/*.{html,js}"],
```

#### Add the Tailwind directives to the index.css

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## JSX (vite)

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```
content: [
"./index.html",
"./src/**/*.{js,ts,jsx,tsx}",
],
```

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

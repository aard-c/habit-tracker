# Habit Tracker

A modern, sleek, and intuitive Habit Tracker built with **Svelte 5** and **TailwindCSS 4**. Build better habits, one day at a time, with zero friction.

## Features

- **Simple Habit Management**: Easily add and manage daily habits.
- **Personalized Categories**: Organize habits with custom categories and vibrant colors.
- **Daily Tracking**: Interactive calendar view to toggle completions for any date.
- **Streak Intelligence**: Automatic calculation of current streaks to keep you motivated.
- **Progress Insights**: Quick stats to see your daily completion rate at a glance.
- **Local Persistence**: All your data is saved securely in your browser's `localStorage`—no account required!
- **Dark Mode Support**: Beautifully designed for both light and dark environments.
- **Fully Responsive**: A seamless experience across mobile, tablet, and desktop.

## Tech Stack

- **Framework**: [Svelte Kit](https://kit.svelte.dev/) (Svelte 5)
- **Styling**: [TailwindCSS 4](https://tailwindcss.com/)
- **Icons**: [Lucide Svelte](https://lucide.dev/)
- **Date Handling**: [date-fns](https://date-fns.org/)
- **Bundler**: [Vite](https://vitejs.dev/)

## Getting Started

### Prerequisites

- **Node.js**: `v20.x` or higher (tested on `v24.13.0`)
- **npm**: `v10.x` or higher

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/aard-c/habit-tracker.git
   cd habit-tracker
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start the development server**:
   ```bash
   npm run dev
   ```

4. **Open your browser**:
   Navigate to [http://localhost:5173/](http://localhost:5173/) to start tracking!

## Project Structure

```text
habit-tracker/
├── src/
│   ├── lib/
│   │   ├── components/     # Reusable Svelte components
│   │   │   ├── AddHabitModal.svelte
│   │   │   ├── HabitCard.svelte
│   │   │   ├── HabitStats.svelte
│   │   │   └── DeleteConfirmModal.svelte   
│   │   └── index.ts        # Library entry point
│   ├── routes/
│   │   ├── +layout.svelte  # Global layout & styles
│   │   └── +page.svelte    # Main tracking interface
│   ├── app.css             # Tailwind base & global styles
│   ├── app.html            # Main HTML template
│   └── app.d.ts            # TypeScript definitions
├── static/                 # Static assets (images, icons)
├── package.json            # Dependencies and scripts
├── svelte.config.js        # Svelte configuration
├── tailwind.config.js      # TailwindCSS configuration
└── vite.config.ts          # Vite configuration
```

## Available Scripts

- `npm run dev`: Start the development server.
- `npm run build`: Build the project for production.
- `npm run preview`: Preview the production build locally.
- `npm run check`: Run Svelte's type/syntax check.

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

# Kaya Reporting Dashboard

A modern Vue 3 application that simulates Kaya's AI-first reporting engine for performance marketers. This dashboard provides automated, CMS-powered views of campaign insights, performance benchmarks, and predictive signals.

## Features

- **Overview Page**: Displays a grid of client reports with modern card-based layout
- **Reusable HeroSection**: Dynamic component for displaying report details
- **Responsive Design**: Mobile-first approach with Tailwind CSS
- **Error Handling**: Graceful handling of missing data and broken images
- **Modern UI**: Clean, professional interface with hover effects and animations
- **Modal System**: Click-to-view full report details without page navigation

## Tech Stack

- **Vue 3** with Composition API
- **TypeScript** for type safety
- **Tailwind CSS** for styling
- **Vite** for fast development and building
- **Vue Router** for navigation
- **Lucide Vue** for icons

## Setup Instructions

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd kaya-test
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Start the development server**

   ```bash
   npm run dev
   ```

4. **Build for production**

   ```bash
   npm run build
   ```

## Project Structure

```
src/
├── components/
│   ├── ui/
│   │   └── Button.vue          # Reusable button component
│   ├── Overview.vue            # Main overview page component
│   └── HeroSection.vue         # Reusable hero section for reports
├── lib/
│   └── utils.ts                # Utility functions
├── App.vue                     # Root application component
├── main.ts                     # Application entry point
└── index.css                   # Global styles and Tailwind imports

public/
└── reports.json                # Sample report data
```

## Browser Support

- Chrome 88+
- Firefox 85+
- Safari 14+
- Edge 88+

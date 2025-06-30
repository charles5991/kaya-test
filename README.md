# Kaya Reporting Dashboard

A modern Vue 3 application that simulates Kaya's AI-first reporting engine for performance marketers. This dashboard provides automated, CMS-powered views of campaign insights, performance benchmarks, and predictive signals.

## Features

- **Overview Page**: Displays a grid of client reports with modern card-based layout
- **Reusable HeroSection**: Dynamic component for displaying report details
- **Responsive Design**: Mobile-first approach with Tailwind CSS
- **Error Handling**: Graceful handling of missing data and broken images
- **Modern UI**: Clean, professional interface with hover effects and animations
- **Dark/Light Theme**: Complete theme toggle with system preference detection
- **Professional Drawer**: Right-sliding drawer using vaul-vue for report details

## Tech Stack

- **Vue 3** with Composition API
- **TypeScript** for type safety
- **Tailwind CSS** for styling
- **Vite** for fast development and building
- **Vue Router** for navigation
- **Lucide Vue** for icons
- **vaul-vue** for drawer implementation
- **class-variance-authority** for component variants

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

## Design & Technical Decisions

- **Vue 3 Composition API** for better TypeScript integration and maintainability
- **Component-based architecture** following shadcn/vue patterns for reusability
- **Static JSON data** to simulate CMS without backend complexity
- **Tailwind CSS** utility-first approach for rapid development
- **vaul-vue drawer** for native-feeling slide animations
- **Theme-specific SVG fallbacks** for broken images

## Tradeoffs (Time/Scope)

- **Static data** instead of API integration
- **Limited component library** (only essential UI components)
- **Basic accessibility** without comprehensive testing
- **No advanced performance optimizations** (code splitting, lazy loading)
- **Dummy images** instead of real image optimization

## Future Improvements

- **API integration** with proper state management
- **Complete testing suite** (unit, component, E2E)
- **Enhanced UX** (search, filtering, animations)
- **Full accessibility compliance**
- **Performance optimizations** and PWA features
- **Comprehensive component library**

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

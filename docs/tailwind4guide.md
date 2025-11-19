# Tailwind CSS 4 implementation guide

Tailwind CSS 4, released January 22, 2025, represents a complete architectural overhaul with a **Rust-based Oxide engine** delivering 5x faster builds, **CSS-first configuration**, and native support for modern CSS features. This comprehensive guide covers everything you need to confidently implement Tailwind 4 in production projects.

## Full implementation guide for Tailwind 4

### Installation with Vite (recommended approach)

The simplest setup for modern projects uses the Vite plugin:

```bash
npm install tailwindcss @tailwindcss/vite
```

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  plugins: [tailwindcss()],
});
```

```css
/* src/main.css */
@import "tailwindcss";

/* CSS-first configuration */
@theme {
  --color-primary: oklch(0.627 0.265 303.9);
  --font-display: "Inter", system-ui, sans-serif;
  --spacing-section: 4rem;
}
```

```html
<!-- Include in your HTML -->
<link rel="stylesheet" href="/src/main.css">
```

### Alternative setup with CLI

For non-Vite projects, use the standalone CLI:

```bash
npm install tailwindcss @tailwindcss/cli

# Create input CSS
echo '@import "tailwindcss";' > src/input.css

# Build CSS
npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch
```

### PostCSS plugin setup

```javascript
// postcss.config.js
export default {
  plugins: ["@tailwindcss/postcss"],
};
```

The key difference from v3: **no tailwind.config.js required** and **single CSS import** replaces the three @tailwind directives.

## Key differences and breaking changes from Tailwind 3

### Major architectural changes

Tailwind 4 requires modern browsers (Chrome 111+, Safari 16.4+, Firefox 128+) due to native CSS features like `@property`, `color-mix()`, and cascade layers. The framework now uses a **CSS-first configuration** approach, eliminating JavaScript config files by default.

### Import syntax transformation

```css
/* v3 approach - no longer works */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* v4 approach - single import */
@import "tailwindcss";
```

### Renamed and removed utilities

Critical changes to be aware of:

| v3 Utility | v4 Replacement |
|------------|----------------|
| `bg-opacity-*` | `bg-black/50` (use opacity modifier) |
| `text-opacity-*` | `text-black/50` |
| `shadow` | `shadow-sm` |
| `blur` | `blur-sm` |
| `ring` | `ring-3` (now 1px default, was 3px) |
| `overflow-ellipsis` | `text-ellipsis` |
| `flex-grow-*` | `grow-*` |
| `flex-shrink-*` | `shrink-*` |

### Border color default change

```html
<!-- v3: border used gray-200 by default -->
<div class="border">Content</div>

<!-- v4: border uses currentColor, specify explicitly -->
<div class="border border-gray-200">Content</div>
```

### Package structure changes

```json
// v3 dependencies
{
  "devDependencies": {
    "tailwindcss": "^3.4.0",
    "postcss": "^8.0.0",
    "autoprefixer": "^10.0.0"
  }
}

// v4 dependencies - PostCSS and autoprefixer built-in
{
  "devDependencies": {
    "@tailwindcss/vite": "^4.0.0",
    "tailwindcss": "^4.0.0"
  }
}
```

## Best practices for Tailwind 4 development

### CSS-first configuration with @theme

The revolutionary `@theme` directive replaces JavaScript configuration:

```css
@import "tailwindcss";

@theme {
  /* Core brand identity */
  --color-brand-primary: oklch(0.627 0.265 303.9);
  --color-brand-secondary: oklch(0.697 0.17 40.24);
  
  /* Typography system */
  --font-display: "Satoshi", system-ui, sans-serif;
  --font-body: "Inter", system-ui, sans-serif;
  --text-hero: 3rem;
  --text-hero--line-height: 1.1;
  
  /* Spacing rhythm */
  --spacing: 0.25rem; /* 4px base unit */
  --spacing-section: calc(var(--spacing) * 16);
  
  /* Component tokens */
  --radius-brand: 0.75rem;
  --shadow-brand: 0 4px 14px var(--color-brand-primary);
}
```

### Leverage native cascade layers

Tailwind 4 uses CSS cascade layers for predictable specificity:

```css
@layer theme, base, components, utilities;

@layer components {
  .card {
    background: white;
    border-radius: var(--radius-brand);
    padding: 1.5rem;
    box-shadow: var(--shadow-sm);
  }
}

@layer utilities {
  .content-auto {
    content-visibility: auto;
  }
}
```

### Container queries for component-responsive design

```html
<!-- Mark container and use @-prefixed breakpoints -->
<div class="@container">
  <div class="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-3">
    <!-- Responds to container size, not viewport -->
  </div>
</div>

<!-- Named containers for specificity -->
<div class="@container/sidebar">
  <nav class="hidden @sm/sidebar:block">
    <!-- Shows when sidebar is at least sm size -->
  </nav>
</div>
```

### Modern CSS features integration

```html
<!-- 3D transforms -->
<div class="perspective-distant">
  <div class="rotate-x-45 rotate-z-30 transform-3d">3D content</div>
</div>

<!-- Enhanced gradients with interpolation -->
<div class="bg-linear-to-r/oklch from-indigo-500 to-teal-400"></div>

<!-- @starting-style for entry animations -->
<div popover class="transition starting:open:opacity-0 open:opacity-100">
  Fades in on first appearance
</div>

<!-- not-* variant for negation -->
<button class="not-hover:opacity-75">Dims when NOT hovered</button>
```

## Where and how to store core brand colors and fonts

### Hierarchical color system in @theme

```css
@theme {
  /* 1. Brand foundation colors */
  --color-brand-primary: oklch(0.627 0.265 303.9);
  --color-brand-secondary: oklch(0.697 0.17 40.24);
  --color-brand-accent: oklch(0.745 0.17 55.93);
  
  /* 2. Semantic colors for UI states */
  --color-success: oklch(0.723 0.219 149.58);
  --color-warning: oklch(0.769 0.188 70.08);
  --color-error: oklch(0.637 0.237 25.33);
  --color-info: oklch(0.685 0.169 237.32);
  
  /* 3. Functional colors for surfaces */
  --color-surface: oklch(0.985 0.002 247.84);
  --color-surface-elevated: oklch(1 0 0);
  --color-text: oklch(0.208 0.042 265.75);
  --color-text-subtle: oklch(0.554 0.046 257.42);
}
```

### Typography system organization

```css
@theme {
  /* Font stacks with fallbacks */
  --font-display: "Satoshi", ui-sans-serif, system-ui, sans-serif;
  --font-body: "Inter", ui-sans-serif, system-ui, sans-serif;
  --font-mono: "SF Mono", Consolas, "Liberation Mono", monospace;
  
  /* Type scale with line heights */
  --text-xs: 0.75rem;
  --text-xs--line-height: 1.33;
  --text-base: 1rem;
  --text-base--line-height: 1.5;
  --text-xl: 1.25rem;
  --text-xl--line-height: 1.4;
  --text-hero: 3.75rem;
  --text-hero--line-height: 0.9;
  
  /* Font weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
}
```

These theme variables automatically generate utilities (`bg-brand-primary`, `font-display`, `text-hero`) and expose CSS custom properties for use in custom CSS.

## When to use design tokens vs inline utility classes

### Use design tokens for systematic values

Design tokens excel for values that maintain consistency across your entire application:

```css
@theme {
  /* System-wide values that rarely change inline */
  --color-primary: #3b82f6;
  --spacing-section: 4rem;
  --radius-brand: 0.75rem;
  --shadow-elevated: 0 20px 25px -5px rgb(0 0 0 / 0.1);
}

/* Component using tokens */
.brand-card {
  background: var(--color-surface);
  border-radius: var(--radius-brand);
  padding: var(--spacing-section);
}
```

### Use inline utilities for component-specific styling

Utilities provide flexibility for layout, state variations, and one-off adjustments:

```html
<!-- Layout and responsive design with utilities -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-6 p-4">
  <!-- Brand consistency with tokens, layout with utilities -->
  <article class="bg-surface rounded-brand p-comfortable hover:shadow-elevated transition-shadow">
    <h3 class="text-xl font-semibold text-primary">Title</h3>
    <p class="mt-2 text-text-subtle leading-relaxed">
      Content using design tokens for consistency, utilities for spacing
    </p>
  </article>
</div>
```

### Decision matrix

| Use Design Tokens | Use Inline Utilities |
|-------------------|---------------------|
| Brand colors and fonts | Layout and positioning |
| Consistent spacing units | Responsive adjustments |
| Global border radius | Component-specific states |
| Elevation/shadow system | One-time customizations |
| Animation timing | Conditional styling |
| Breakpoint definitions | Interactive states |

## Essential design tokens for any Tailwind 4 project

### Complete design token foundation

```css
@theme {
  /* Colors: Brand, functional, and semantic */
  --color-brand-primary: oklch(0.627 0.265 303.9);
  --color-brand-secondary: oklch(0.697 0.17 40.24);
  --color-surface: oklch(0.985 0.002 247.84);
  --color-text: oklch(0.208 0.042 265.75);
  --color-success: oklch(0.723 0.219 149.58);
  --color-warning: oklch(0.769 0.188 70.08);
  --color-error: oklch(0.637 0.237 25.33);
  
  /* Typography: Families and scale */
  --font-display: "Inter", system-ui, sans-serif;
  --font-body: "Inter", system-ui, sans-serif;
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  
  /* Spacing: Base unit and semantic values */
  --spacing: 0.25rem;
  --spacing-tight: calc(var(--spacing) * 3);
  --spacing-comfortable: calc(var(--spacing) * 6);
  --spacing-loose: calc(var(--spacing) * 12);
  
  /* Shadows: Elevation system */
  --shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.1);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1);
  
  /* Border radius: Corner system */
  --radius-sm: 0.25rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
  --radius-xl: 0.75rem;
  --radius-full: 9999px;
  
  /* Animation: Timing functions */
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-spring: cubic-bezier(0.5, 1.25, 0.75, 1.25);
  
  /* Layout: Container widths */
  --container-xs: 20rem;
  --container-sm: 24rem;
  --container-md: 28rem;
  --container-lg: 32rem;
  --container-xl: 36rem;
  --container-prose: 65ch;
}
```

## Examples of inline utilities vs abstracted design tokens

### Button component: Abstraction pattern

```jsx
// Abstracted button with design tokens
function Button({ variant = 'primary', size = 'md', children }) {
  const baseClasses = 'font-medium rounded-brand transition-colors focus:outline-none focus:ring-2';
  
  const variants = {
    primary: 'bg-brand-primary text-white hover:bg-brand-primary/90',
    secondary: 'bg-brand-secondary text-white hover:bg-brand-secondary/90',
    ghost: 'bg-transparent text-brand-primary hover:bg-brand-primary/10'
  };
  
  const sizes = {
    sm: 'px-tight py-1.5 text-sm',
    md: 'px-comfortable py-2 text-base',
    lg: 'px-loose py-3 text-lg'
  };
  
  return (
    <button className={`${baseClasses} ${variants[variant]} ${sizes[size]}`}>
      {children}
    </button>
  );
}
```

### Card layout: Utility-first pattern

```html
<!-- Inline utilities for flexible card layouts -->
<div class="bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow">
  <div class="flex items-start justify-between mb-4">
    <div class="flex items-center space-x-3">
      <img class="w-10 h-10 rounded-full" src="avatar.jpg" alt="">
      <div>
        <h3 class="text-lg font-semibold text-gray-900">John Doe</h3>
        <p class="text-sm text-gray-500">Product Designer</p>
      </div>
    </div>
    <button class="text-gray-400 hover:text-gray-600">
      <svg class="w-5 h-5" fill="currentColor"><!-- icon --></svg>
    </button>
  </div>
  <p class="text-gray-700 leading-relaxed">
    Card content with inline utilities for maximum flexibility
  </p>
</div>
```

### Hybrid approach: Best of both worlds

```jsx
// Component with token foundation and utility flexibility
function Card({ className = '', elevated = false, children }) {
  return (
    <div className={`
      bg-surface rounded-brand p-comfortable
      ${elevated ? 'shadow-elevated' : 'shadow-sm'}
      hover:shadow-lg transition-shadow
      ${className}
    `}>
      {children}
    </div>
  );
}

// Usage combines tokens and utilities
<Card elevated className="max-w-md mx-auto md:max-w-2xl">
  <h2 class="text-xl font-bold text-brand-primary mb-comfortable">
    Title uses tokens
  </h2>
  <div class="grid grid-cols-2 gap-4">
    <!-- Layout with utilities -->
  </div>
</Card>
```

## Migration guide from Tailwind 3 to Tailwind 4

### Automated migration tool

Start with the official upgrade tool that handles most changes automatically:

```bash
# Run in a new Git branch for safety
npx @tailwindcss/upgrade

# The tool will:
# - Update dependencies
# - Convert config to CSS
# - Update utility classes
# - Modify import statements
```

### Manual migration steps

#### 1. Convert JavaScript config to CSS

```javascript
// tailwind.config.js (v3)
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: '#3B82F6'
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif']
      }
    }
  },
  plugins: [require('@tailwindcss/typography')]
}
```

```css
/* main.css (v4) */
@import "tailwindcss";
@plugin "@tailwindcss/typography";

@theme {
  --color-brand: #3B82F6;
  --font-sans: "Inter", sans-serif;
}
```

#### 2. Update dark mode configuration

```css
/* v3 class-based dark mode */
/* darkMode: 'class' in config */

/* v4 equivalent */
@custom-variant dark (&:where(.dark, .dark *));
```

#### 3. Fix dynamic class patterns

```javascript
// v3 approach with safelist
safelist: [
  { pattern: /bg-(red|green|blue)-(400|500|600)/ }
]

// v4 approach with inline source
```

```css
@source inline('bg-{red,green,blue}-{400,500,600}');
```

### Migration checklist

- [ ] Back up your project and create a new branch
- [ ] Run the automated upgrade tool
- [ ] Update package.json dependencies
- [ ] Convert SCSS files to CSS (v4 includes nesting support)
- [ ] Replace @tailwind directives with @import "tailwindcss"
- [ ] Convert tailwind.config.js to @theme CSS
- [ ] Update renamed utility classes
- [ ] Add explicit border colors where needed
- [ ] Test dark mode implementation
- [ ] Verify dynamic class generation
- [ ] Update build scripts
- [ ] Test in all target browsers
- [ ] Update CI/CD pipelines

## New features and improvements in Tailwind 4

### Revolutionary Oxide engine

The Rust-powered Oxide engine delivers **5x faster full builds** and **100x faster incremental builds**. The Lightning CSS integration processes 2.7 million lines per second, making development lightning-fast.

### Native container queries

```html
<!-- Component-responsive design out of the box -->
<div class="@container">
  <div class="@lg:flex @lg:items-center @lg:justify-between">
    <h2 class="text-xl @sm:text-2xl @md:text-3xl">
      Responsive to container, not viewport
    </h2>
  </div>
</div>
```

### 3D transforms and perspectives

```html
<div class="perspective-[1000px]">
  <div class="rotate-x-45 rotate-y-30 rotate-z-15 transform-3d 
              hover:rotate-x-0 hover:rotate-y-0 transition-all duration-500">
    3D transformed element
  </div>
</div>
```

### Enhanced gradient capabilities

```html
<!-- Linear gradients with angle control -->
<div class="bg-linear-45 from-purple-500 to-pink-500"></div>

<!-- Conic gradients -->
<div class="bg-conic from-red-500 via-yellow-500 to-green-500"></div>

<!-- Radial with position -->
<div class="bg-radial-[at_25%_75%] from-blue-500 to-transparent"></div>

<!-- Gradient interpolation modes -->
<div class="bg-linear-to-r/oklch from-indigo-500 to-teal-400"></div>
```

### Wide-gamut colors with OKLCH

```css
@theme {
  /* P3 color space for vibrant colors */
  --color-vibrant-blue: oklch(0.64 0.24 262.8);
  --color-electric-purple: oklch(0.53 0.28 303.9);
}
```

### Zero-configuration content detection

Tailwind 4 automatically scans your project files, respecting `.gitignore` and excluding binaries—no more content array configuration needed.

## Common mistakes to avoid

### Dynamic class construction antipattern

```javascript
// ❌ NEVER do this - classes may be purged
const color = userInput;
const className = `bg-${color}-500 hover:bg-${color}-600`;

// ✅ Use a lookup table instead
const colorClasses = {
  red: 'bg-red-500 hover:bg-red-600',
  blue: 'bg-blue-500 hover:bg-blue-600',
  green: 'bg-green-500 hover:bg-green-600'
};
const className = colorClasses[color] || colorClasses.red;
```

### Overusing @apply

```css
/* ❌ Avoid - breaks utility-first paradigm */
.btn {
  @apply bg-blue-500 px-4 py-2 text-white rounded-lg;
}

/* ✅ Better - use components or utilities directly */
@utility btn-primary {
  background: var(--color-brand-primary);
  padding: 0.5rem 1rem;
  color: white;
  border-radius: var(--radius-brand);
}
```

### Magic values instead of design tokens

```html
<!-- ❌ Arbitrary values reduce maintainability -->
<div class="p-[37px] text-[#ff6b35] rounded-[7px]"></div>

<!-- ✅ Use design tokens for consistency -->
<div class="p-comfortable text-brand-primary rounded-brand"></div>
```

### Ignoring browser requirements

Tailwind 4 requires Chrome 111+, Safari 16.4+, and Firefox 128+. If you need older browser support, stay on Tailwind 3.4.

### Missing explicit border colors

```html
<!-- ❌ v3 habit - border had default gray color -->
<div class="border p-4"></div>

<!-- ✅ v4 requirement - specify color explicitly -->
<div class="border border-gray-200 p-4"></div>
```

## Performance optimizations specific to Tailwind 4

### Production build configuration

```bash
# Optimize for production with minification
NODE_ENV=production npx @tailwindcss/cli -i input.css -o output.css --minify
```

### Vite configuration for optimal performance

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  plugins: [tailwindcss()],
  build: {
    cssMinify: 'lightningcss',
    cssCodeSplit: true
  }
});
```

### Content optimization strategies

```css
/* Exclude unnecessary paths from scanning */
@source "../src/**/*.{html,js,jsx,ts,tsx}";
@source not("../src/legacy/**");
@source not("../node_modules/**");

/* Add safelist for dynamic content */
@source inline('bg-{red,green,blue}-{400,500,600}');
```

### CSS layer optimization

```css
/* Define layer order for predictable cascade */
@layer theme, base, components, utilities;

/* Place custom utilities in appropriate layer */
@layer utilities {
  .content-auto {
    content-visibility: auto; /* Improves rendering performance */
  }
  
  .gpu-accelerated {
    transform: translateZ(0); /* Force GPU acceleration */
  }
}
```

### Bundle size optimization results

Most Tailwind 4 projects ship **less than 10kB of CSS** after optimization. The Netflix Top 10 example uses only **6.5kB** over the network—smaller than most images.

## Configuration best practices for production

### Structured configuration approach

```css
/* main.css - Entry point */
@import "tailwindcss";

/* Organized token imports */
@import "./design-tokens/colors.css";
@import "./design-tokens/typography.css";
@import "./design-tokens/spacing.css";
@import "./design-tokens/effects.css";

/* Component styles */
@import "./components/buttons.css";
@import "./components/cards.css";

/* Custom utilities */
@import "./utilities/animations.css";
```

### Multi-environment configuration

```css
/* Base configuration for all environments */
@theme {
  --color-brand-primary: oklch(0.627 0.265 303.9);
  --font-body: "Inter", system-ui, sans-serif;
}

/* Development-only helpers */
@media (--development) {
  @theme {
    --color-debug: red;
    --spacing-debug: 1px;
  }
}

/* Production optimizations */
@media (--production) {
  @layer utilities {
    .perf-optimize {
      will-change: auto;
      contain: layout style paint;
    }
  }
}
```

### Theming and white-label support

```css
@theme {
  /* Default theme */
  --color-primary: #3b82f6;
  --color-surface: #ffffff;
}

/* Theme variations */
[data-theme="ocean"] {
  --color-primary: #0ea5e9;
  --color-surface: #f0f9ff;
}

[data-theme="forest"] {
  --color-primary: #10b981;
  --color-surface: #ecfdf5;
}

/* Apply theme dynamically */
.themed-component {
  background: var(--color-surface);
  color: var(--color-primary);
}
```

### IDE configuration for optimal DX

```json
// .vscode/settings.json
{
  "tailwindCSS.experimental.configFile": {
    "./src/styles/main.css": "./src/**"
  },
  "editor.quickSuggestions": {
    "strings": true
  },
  "css.validate": false,
  "editor.formatOnSave": true
}
```

### Production deployment checklist

1. **Enable production mode**: `NODE_ENV=production`
2. **Minify CSS output**: Use cssnano or Lightning CSS
3. **Enable compression**: Configure Brotli/gzip on server
4. **Optimize content scanning**: Exclude unnecessary paths
5. **Verify browser support**: Test in Chrome 111+, Safari 16.4+, Firefox 128+
6. **Monitor bundle size**: Aim for sub-10kB CSS
7. **Configure caching**: Set appropriate cache headers
8. **Test dynamic classes**: Ensure safelist covers all cases
9. **Validate dark mode**: Test theme switching thoroughly
10. **Profile performance**: Use Lighthouse for metrics

## Conclusion

Tailwind CSS 4 represents a paradigm shift with its CSS-first configuration, native modern CSS support, and dramatic performance improvements through the Oxide engine. The framework maintains its utility-first philosophy while embracing web platform capabilities, resulting in faster builds, smaller bundles, and better developer experience. By following this guide's patterns for design tokens, component abstraction, and configuration, you can leverage Tailwind 4's full potential while avoiding common pitfalls. The combination of **5x faster builds**, **native container queries**, and **zero-configuration setup** makes Tailwind 4 the most powerful version yet for building modern, performant web interfaces.
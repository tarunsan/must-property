# Quick Wins - Immediate Performance Improvements

## 🎯 5-Minute Fixes

### 1. Add Loading States
Create `src/app/loading.tsx`:
```tsx
export default function Loading() {
  return (
    <div style={{ 
      minHeight: '100vh', 
      display: 'flex', 
      alignItems: 'center', 
      justifyContent: 'center',
      background: 'var(--surface)'
    }}>
      <div style={{ textAlign: 'center' }}>
        <div className="spinner" style={{
          width: '50px',
          height: '50px',
          border: '4px solid var(--border)',
          borderTop: '4px solid var(--primary)',
          borderRadius: '50%',
          animation: 'spin 1s linear infinite',
          margin: '0 auto 1rem'
        }}></div>
        <p style={{ color: 'var(--text-secondary)' }}>Loading properties...</p>
      </div>
      <style jsx>{`
        @keyframes spin {
          0% { transform: rotate(0deg); }
          100% { transform: rotate(360deg); }
        }
      `}</style>
    </div>
  );
}
```

### 2. Add Error Boundary
Create `src/app/error.tsx`:
```tsx
'use client';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div style={{ 
      minHeight: '100vh', 
      display: 'flex', 
      alignItems: 'center', 
      justifyContent: 'center',
      padding: '2rem'
    }}>
      <div className="card" style={{ maxWidth: '500px', padding: '3rem', textAlign: 'center' }}>
        <h2 style={{ fontSize: '1.5rem', fontWeight: 700, marginBottom: '1rem' }}>
          Something went wrong!
        </h2>
        <p style={{ color: 'var(--text-secondary)', marginBottom: '2rem' }}>
          We're sorry for the inconvenience. Please try again.
        </p>
        <button onClick={reset} className="btn btn-primary">
          Try again
        </button>
      </div>
    </div>
  );
}
```

### 3. Add Not Found Page
Create `src/app/not-found.tsx`:
```tsx
import Link from 'next/link';
import Navbar from '@/components/Navbar';

export default function NotFound() {
  return (
    <main style={{ minHeight: '100vh' }}>
      <Navbar />
      <div style={{ 
        display: 'flex', 
        alignItems: 'center', 
        justifyContent: 'center',
        minHeight: 'calc(100vh - var(--header-height))',
        padding: '2rem'
      }}>
        <div style={{ textAlign: 'center', maxWidth: '500px' }}>
          <h1 style={{ fontSize: '6rem', fontWeight: 800, color: 'var(--primary)' }}>404</h1>
          <h2 style={{ fontSize: '2rem', fontWeight: 700, marginBottom: '1rem' }}>
            Page Not Found
          </h2>
          <p style={{ color: 'var(--text-secondary)', marginBottom: '2rem' }}>
            The page you're looking for doesn't exist or has been moved.
          </p>
          <Link href="/" className="btn btn-primary">
            Go Home
          </Link>
        </div>
      </div>
    </main>
  );
}
```

## ⚡ 15-Minute Improvements

### 4. Add Sitemap
Create `src/app/sitemap.ts`:
```typescript
import { MetadataRoute } from 'next';

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://your-domain.com',
      lastModified: new Date(),
      changeFrequency: 'daily',
      priority: 1,
    },
    {
      url: 'https://your-domain.com/properties',
      lastModified: new Date(),
      changeFrequency: 'daily',
      priority: 0.9,
    },
    {
      url: 'https://your-domain.com/agents',
      lastModified: new Date(),
      changeFrequency: 'weekly',
      priority: 0.8,
    },
    {
      url: 'https://your-domain.com/admin',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.3,
    },
  ];
}
```

### 5. Add Robots.txt
Create `src/app/robots.ts`:
```typescript
import { MetadataRoute } from 'next';

export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      {
        userAgent: '*',
        allow: '/',
        disallow: ['/admin/', '/api/'],
      },
    ],
    sitemap: 'https://your-domain.com/sitemap.xml',
  };
}
```

### 6. Optimize CSS
Add to `globals.css`:
```css
/* Performance optimizations */
* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Reduce motion for accessibility */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Improve scrolling performance */
html {
  scroll-behavior: smooth;
}

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }
}

/* Loading spinner animation */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.spinner {
  animation: spin 1s linear infinite;
}

/* Skeleton loading */
@keyframes shimmer {
  0% { background-position: -1000px 0; }
  100% { background-position: 1000px 0; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--surface) 0%,
    var(--border) 50%,
    var(--surface) 100%
  );
  background-size: 1000px 100%;
  animation: shimmer 2s infinite;
}
```

## 🚀 30-Minute Enhancements

### 7. Add Property Schema Markup
Update `src/app/properties/[id]/page.tsx`:
```tsx
export default function PropertyDetail() {
  // ... existing code ...

  const schemaData = {
    "@context": "https://schema.org",
    "@type": "RealEstateListing",
    "name": property.title,
    "description": property.description,
    "price": property.price,
    "address": {
      "@type": "PostalAddress",
      "addressLocality": property.address,
      "addressCountry": "IN"
    },
    "numberOfRooms": property.beds,
    "floorSize": {
      "@type": "QuantitativeValue",
      "value": property.sqft,
      "unitCode": "FTK"
    },
    "image": property.image,
  };

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(schemaData) }}
      />
      {/* ... rest of component ... */}
    </>
  );
}
```

### 8. Add Web Vitals Reporting
Create `src/app/web-vitals.tsx`:
```tsx
'use client';

import { useReportWebVitals } from 'next/web-vitals';

export function WebVitals() {
  useReportWebVitals((metric) => {
    // Send to analytics
    if (window.gtag) {
      window.gtag('event', metric.name, {
        value: Math.round(metric.name === 'CLS' ? metric.value * 1000 : metric.value),
        event_label: metric.id,
        non_interaction: true,
      });
    }
    
    // Log to console in development
    if (process.env.NODE_ENV === 'development') {
      console.log(metric);
    }
  });

  return null;
}
```

Then add to `layout.tsx`:
```tsx
import { WebVitals } from './web-vitals';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  );
}
```

## 📊 Testing Commands

### Run Performance Audit
```bash
# Install Lighthouse CLI
npm install -g lighthouse

# Run audit
lighthouse https://your-domain.com --view

# Run on mobile
lighthouse https://your-domain.com --preset=mobile --view
```

### Check Bundle Size
```bash
# Analyze bundle
npm run build

# Use bundle analyzer
npm install -D @next/bundle-analyzer

# Update next.config.ts
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer(nextConfig);

# Run analysis
ANALYZE=true npm run build
```

## 🎨 Visual Improvements

### Add Smooth Transitions
Update `globals.css`:
```css
/* Smooth transitions for all interactive elements */
.btn,
.card,
a,
button,
input,
select,
textarea {
  transition: all 0.2s ease;
}

/* Hover effects */
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-xl);
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
}

.btn:active {
  transform: translateY(0);
}

/* Focus states for accessibility */
*:focus-visible {
  outline: 2px solid var(--primary);
  outline-offset: 2px;
}
```

## ✅ Quick Checklist

- [ ] Add loading.tsx
- [ ] Add error.tsx
- [ ] Add not-found.tsx
- [ ] Add sitemap.ts
- [ ] Add robots.ts
- [ ] Optimize CSS
- [ ] Add schema markup
- [ ] Add web vitals tracking
- [ ] Test on mobile
- [ ] Run Lighthouse
- [ ] Check bundle size
- [ ] Update GA ID
- [ ] Deploy to production

## 🎯 Expected Results

After implementing these quick wins:

- **Performance Score**: 90+ (Lighthouse)
- **SEO Score**: 95+ (Lighthouse)
- **Accessibility Score**: 90+ (Lighthouse)
- **Best Practices**: 95+ (Lighthouse)
- **Load Time**: < 2 seconds
- **Bundle Size**: < 500KB (gzipped)

## 📞 Support

For issues or questions:
- WhatsApp: +91 6362234238
- Email: tarun.m1890@gmail.com

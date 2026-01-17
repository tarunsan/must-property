# 📱 Mobile App Implementation Guide (Project Summary)

Use this document as a blueprint to rebuild the **MUST Property** real estate application in your new workspace or as a mobile app (React Native/Flutter).

---

## 🚀 1. Technology Stack

- **Framework**: Next.js 16 (React 19) / React Native
- **Language**: TypeScript
- **Database**: Firebase Firestore
- **Styling**: CSS Modules / Vanilla CSS (with CSS Variables for theming)
- **Icons**: Lucide React
- **Charts**: Recharts
- **Maps/Geocoding**: OpenStreetMap (Nominatim & Overpass APIs) - *100% Free*

---

## 📦 2. Core Dependencies

```json
{
  "firebase": "^12.6.0",
  "framer-motion": "^12.0.0",   // Animations
  "lucide-react": "^0.460.0",   // Icons
  "recharts": "^2.13.0",        // Price charts
  "leaflet": "^1.9.4",          // Maps (optional)
  "react-leaflet": "^4.2.1"
}
```

---

## 🗄️ 3. Data Models (Firebase Firestore)

### Collection: `properties`
All property listings are stored here.

```typescript
interface Property {
    id: string;               // Firebase Document ID
    title: string;            // e.g., "Luxury Villa in Whitefield"
    type: string;             // Apartment, Villa, Penthouse, etc.
    price: string;            // e.g., "₹ 2.5 Cr"
    address: string;          // Full address string
    mapLink?: string;         // Google Maps URL (optional)
    description: string;      // Full text description
    
    // Specs
    beds: number;
    baths: number;
    sqft: number;             // Super Built-up Area
    carpetArea: number;       // Actual usable area
    
    // Details
    facing: string;           // East, West, North, South
    floor: string;            // e.g., "2nd", "Ground"
    furnishing: string;       // Fully, Semi, Unfurnished
    age: string;              // New, 1-5 Years, etc.
    
    // Visuals
    image: string;            // Main cover image URL
    images: string[];         // Gallery image URLs
    features: string[];       // ["Gym", "Pool", "Parking"]
    
    // Agent Info (Embedded Object)
    agent: {
        name: string;
        phone: string;        // e.g., "+91 6362234238"
        email: string;        // e.g., "mustgroup26@gmail.com"
        image: string;
    };
    
    matchScore?: number;      // AI Match Score (0-100)
}
```

### Collection: `agents`
Storage for agent profiles (used in Admin Dashboard).

```typescript
interface Agent {
    id: string;
    name: string;
    phone: string;
    email: string;
    image: string;
    specialization?: string;
    experience?: string;
}
```

---

## 🧠 4. Key Logic & Algorithms

### A. Mortgage Calculator
Calculates monthly EMI with a hidden 2% commission included in the principal.

**Formula:**
1.  **Price Per Sqft**: `Total Price / Sqft`
2.  **Base Price**: `Sqft * Price Per Sqft`
3.  **Commission**: `Base Price * 0.02` (2%)
4.  **Final Principal**: `Base Price + Commission`
5.  **EMI Calculation**: Standard reducing balance formula.

### B. Price History Trend (Dynamic)
Generates a realistic-looking 7-month price chart that *always* ends at the current property price.

**Logic:**
- Start at `Month 0` (6 months ago).
- Apply a base monthly increase factor (e.g., ~0.5%).
- Add random noise (`Math.random()`) for realistic fluctuations (±2.5%).
- **Constraint**: `Month 6` (Current) must exactly match `property.price`.

### C. Neighborhood Amenities (100% Free)
Uses OpenStreetMap APIs to fetch real nearby places.

**Endpoint 1: Geocoding (Address → Lat/Lng)**
- **API**: `https://nominatim.openstreetmap.org/search`
- **Params**: `q={address}`, `format=json`

**Endpoint 2: Nearby Places (Lat/Lng + Type → Places)**
- **API**: `https://overpass-api.de/api/interpreter`
- **Query**: Search for nodes/ways with tags like `amenity=school`, `amenity=hospital` within 5000m radius.
- **Distance**: Calculated using **Haversine Formula**.

---

## 🎨 5. Design System (CSS Variables)

Copy these into your global CSS for consistent branding.

```css
:root {
  --primary: #2563eb;          /* Royal Blue */
  --primary-dark: #1d4ed8;
  --primary-light: #eff6ff;
  --secondary: #475569;        /* Slate Grey */
  --accent: #f59e0b;           /* Amber */
  --background: #ffffff;
  --background-alt: #f8fafc;   /* Light Grey Background */
  --surface: #ffffff;
  --surface-hover: #f1f5f9;
  --text-primary: #0f172a;     /* Dark Slate */
  --text-secondary: #64748b;
  --text-muted: #94a3b8;
  --border: #e2e8f0;
  --radius: 0.5rem;            /* 8px */
  --radius-lg: 1rem;           /* 16px */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1);
}
```

---

## 📱 6. Features Checklist for New App

- [ ] **Home Page**: Hero Search, Featured Properties (Horizontal Scroll), Categories.
- [ ] **Property List**: Grid/List view with filters (Price, Beds, Type).
- [ ] **Detail Screen**:
    - [ ] Image Gallery (Swipeable)
    - [ ] Key Specs (Grid: Beds, Baths, Sqft)
    - [ ] Read More/Less Description
    - [ ] **Dynamic Features**: Mortgage Calc, Price trends, Neighborhood Map.
    - [ ] **Sticky Footer**: "Contact Agent" / "WhatsApp" buttons.
- [ ] **Admin Dashboard**:
    - [ ] Login Screen (Simple password protection)
    - [ ] CRUD Operations (Add/Edit/Delete listings)
- [ ] **Favorites**: Local storage or Firebase user favorites.

---

## 🔑 7. Credentials (Environment Variables)

**Firebase Config:**
```env
NEXT_PUBLIC_FIREBASE_API_KEY=AIzaSyB6ZMLayydPJxAABuWTxjtyjdoL8sFw0pY
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=must-properties.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=must-properties
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=must-properties.firebasestorage.app
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=390028707480
NEXT_PUBLIC_FIREBASE_APP_ID=1:390028707480:web:002d6ff6c47a138a6c72ed
```

**Contact Details:**
- **Phone**: +91 6362234238
- **Email**: mustgroup26@gmail.com
- **Address**: No. 34, K No:35/34, Near Panchayat Office, Kattigenahalli, Yelehanka Bengalru Karnataka-560064

---

## 🛠️ 8. Next Steps

1.  **Initialize Project**: `npx create-next-app` or `npx create-expo-app` (for mobile).
2.  **Install Dependencies**: Install Firebase, Lucide, etc.
3.  **Setup Firebase**: Copy the `firebase.ts` config file.
4.  **Copy Components**: Port over the UI components using the Logic Summary above.
5.  **Deploy**: Use Vercel (Web) or EAS Build (Mobile).

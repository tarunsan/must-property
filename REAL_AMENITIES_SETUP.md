# Real Neighborhood Amenities Setup Guide

## Overview
The app now fetches **real, accurate amenities** (schools, hospitals, transit, shopping) based on the actual property location using Google Maps APIs.

## What Changed

### 1. **NeighborhoodInsights Component**
- Now fetches real data from Google Places API
- Shows actual schools, colleges, hospitals, and shopping centers near the property
- Displays real distances calculated using GPS coordinates
- Includes loading states and error handling

### 2. **New API Routes Created**
- `/api/geocode` - Converts property address to GPS coordinates
- `/api/nearby-places` - Fetches nearby amenities by type with real distances

## Setup Instructions

### Step 1: Get Google Maps API Key

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Go to **APIs & Services** > **Credentials**
4. Click **Create Credentials** > **API Key**
5. Copy your API key

### Step 2: Enable Required APIs

In Google Cloud Console, enable these APIs:

1. **Geocoding API** - Converts addresses to coordinates
   - Go to: APIs & Services > Library
   - Search for "Geocoding API"
   - Click Enable

2. **Places API** - Finds nearby places
   - Go to: APIs & Services > Library
   - Search for "Places API"
   - Click Enable

### Step 3: Configure Environment Variables

1. Create a `.env.local` file in the root directory:
   ```bash
   # Copy the example file
   copy env.example.txt .env.local
   ```

2. Edit `.env.local` and add your Google Maps API key:
   ```env
   NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=YOUR_ACTUAL_API_KEY_HERE
   ```

3. Also add your existing Firebase credentials if not already present

### Step 4: Restart Development Server

```bash
npm run dev
```

## How It Works

1. **User visits property page** → Component loads
2. **Address is geocoded** → Converted to latitude/longitude
3. **Nearby places fetched** → Google Places API searches within 5km radius
4. **Real distances calculated** → Using Haversine formula
5. **Results displayed** → Top 3 amenities per category

## Features

✅ **Real Data**: Actual schools, hospitals, transit stations, shopping malls
✅ **Accurate Distances**: Calculated using GPS coordinates
✅ **Auto-updating**: Different amenities for each property location
✅ **Loading States**: Shows spinner while fetching data
✅ **Error Handling**: Graceful fallback if API fails
✅ **No Fake Data**: Everything is 100% real and location-specific

## API Usage & Costs

- **Geocoding API**: ~$5 per 1000 requests
- **Places API**: ~$17 per 1000 requests
- **Free Tier**: $200 credit per month (covers ~10,000 property views)

### Cost Optimization Tips:
- Results are fetched fresh each time (no caching yet)
- Consider adding Redis/database caching for production
- Monitor usage in Google Cloud Console

## Testing

1. Visit any property detail page
2. Scroll to "Neighborhood Insights" section
3. You should see:
   - Loading spinner initially
   - Real nearby schools, hospitals, transit, shopping
   - Actual distances in meters/kilometers

## Troubleshooting

### "Unable to load nearby amenities"
- Check if API key is set in `.env.local`
- Verify APIs are enabled in Google Cloud Console
- Check browser console for detailed errors

### No results showing
- Property address might be invalid
- Try a more specific address
- Check API quotas in Google Cloud Console

### API Key errors
- Make sure key starts with `NEXT_PUBLIC_` in .env.local
- Restart dev server after adding the key
- Check if key has proper permissions

## Next Steps (Optional Enhancements)

1. **Add Caching**: Store results in database to reduce API calls
2. **Add More Categories**: Restaurants, parks, gyms, etc.
3. **Show on Map**: Display amenities on an interactive map
4. **Walking Directions**: Add links to Google Maps directions
5. **User Reviews**: Show Google ratings for each place

## Support

If you encounter issues:
1. Check the browser console for errors
2. Verify API key is correctly set
3. Ensure both Geocoding and Places APIs are enabled
4. Check Google Cloud Console for quota limits

---

**Note**: Without the Google Maps API key, the component will show an error message. The rest of the app will continue to work normally.

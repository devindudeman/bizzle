# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Development Tasks

### Running Locally
The game is a simple client-side web application with no build process required:

**Option 1 (Direct):**
- Double-click `index.html` to open in browser

**Option 2 (Local Server - Recommended):**
```bash
# Python
python3 -m http.server 8000

# Node.js
npx http-server -p 8000

# PHP
php -S localhost:8000
```

Visit http://localhost:8000 in browser.

### Testing
No formal testing framework is configured. Testing is done manually:
1. Open the game in browser
2. Check that business data loads from Google Sheets
3. Test guessing functionality with various inputs
4. Verify statistics tracking works
5. Test share functionality

## Architecture

### Single-File Application
- **index.html**: Contains all HTML, CSS, and JavaScript in one file (~3,900+ lines)
- Pure vanilla JavaScript, no frameworks or dependencies
- Client-side only - no backend server required
- **Theme**: Beautiful Catppuccin Mocha dark theme throughout

### Core Components
- **Data Source**: Google Sheets API for business data storage
- **Game Logic**: Multi-phase guessing game with points system
  - **Freeform Phase**: Type business names with autocomplete
  - **Multiple Choice Phase**: Progressive map zooming with location-based choices
- **Google Maps Integration**: Interactive maps with geocoding and progressive zoom
- **Points System**: 5-4-3-2-1 points based on guess number + 2 bonus for visits
- **Achievement System**: 13 achievements across 4 categories (streaks, scores, discovery, visits)
- **Visit Tracking**: Mark businesses as visited for bonus points
- **Karl the Fog**: Animated mascot that floats around (toggleable)
- **Modals**: Tutorial, Statistics, Why Bizzle?, Debug menu
- **Local Storage**: Game state, statistics, achievements, visits, coordinate caching
- **Responsive UI**: Mobile-friendly design with CSS Grid/Flexbox, compact header behavior
- **Skip Feature**: "Multiple Choice" button to jump directly to map phase
- **Floating Settings**: Bottom-right floating buttons for settings (stats, Karl toggle, debug)

### Key Configuration
- `SHEET_ID`: Google Sheets document ID (line ~979)
- `API_KEY`: Google Maps & Sheets API key (line ~980)
- Game starts from epoch date: '2025-08-10' (getDayNumber function)
- Maximum guesses: 5 total (across both phases)
- Fuzzy match threshold: 0.8 (fuzzyMatch function)
- Progressive zoom levels: 11→13→15→17→19 (updateMapZoom function)

### Data Flow
1. **Initialization**: Fetch business data from Google Sheets, load Google Maps API
2. **Daily Selection**: Select business based on date rotation or DateToFeature column
3. **Freeform Phase**: Player types business names with autocomplete suggestions
4. **Transition**: After wrong guess, transition to multiple choice with map
5. **Progressive Rounds**: Each wrong answer zooms map closer and provides new hints
6. **Geocoding**: Cache coordinates using Google Geocoding API for performance
7. **Game End**: Calculate points (5-4-3-2-1), update statistics, show final location
8. **Persistence**: Save game state, statistics, and coordinate cache to localStorage

### Game Phases
- **Phase 1 (Freeform)**: Type business names with autocomplete from all businesses
- **Phase 2 (Multiple Choice)**: Progressive map zoom with location-aware options
  - **Round 2**: City-wide view (zoom 13) with 8 options
  - **Round 3**: Neighborhood view (zoom 15) with 6 options  
  - **Round 4**: Street view (zoom 17) with 4 options
  - **Final**: Exact location (zoom 19) with marker and info window

### Points System
- **5 points**: Correct on 1st guess (freeform phase)
- **4 points**: Correct on 2nd guess (map round 2)
- **3 points**: Correct on 3rd guess (map round 3)
- **2 points**: Correct on 4th guess (map round 4)
- **1 point**: Correct on 5th guess (final chance)
- **+2 bonus points**: Mark a business as visited
- **0 points**: Give up or lose
- Points accumulate in totalPoints statistic over time

### Customization Points
- **Color scheme**: Catppuccin Mocha theme with CSS custom properties
- **Number of rounds/guesses**: Update game logic and round configurations
- **Fuzzy matching sensitivity**: Adjust threshold in fuzzyMatch function
- **Map zoom progression**: Modify zoomLevels object in updateMapZoom
- **Points scoring**: Change calculatePoints function
- **Sheet structure**: Expected columns defined in README
- **UI Behavior**: Header auto-hiding, compact clues, settings placement

## Google Sheets Integration
Business data format:
- **Name**: Business name (used for correct answers)
- **PhotoURL**: Google Drive direct link to interior photo
- **FunFact**: Interesting fact shown initially
- **Neighborhood**: Hint revealed in round 2
- **Signature**: Featured item/service, hint revealed in round 3  
- **Category**: Business type, hint revealed in round 4
- **Street**: Street name, final hint revealed in round 4
- **Address**: Full address used for geocoding to get coordinates
- **Website**: Optional website URL shown in final reveal
- **DateToFeature**: Optional specific date (YYYY-MM-DD) to feature business

Requirements:
- Public sheet with viewer permissions required
- Images stored in Google Drive with direct link format (`drive.google.com/uc?export=view&id=FILE_ID`)
- Daily rotation based on DateToFeature column or automatic cycling through all businesses

## Google Maps Integration
- **Async Loading**: Google Maps API loaded dynamically to avoid loading warnings
- **Geocoding Cache**: Coordinates cached in localStorage for performance
- **Progressive Zoom**: 5 zoom levels from city-wide to exact location
- **Custom Markers**: SVG-based markers for business locations
- **Location-Aware Options**: Multiple choice options filtered by business type and proximity
- **Fallback System**: Curated SF business options when Places API unavailable

## Statistics Tracking
Stored in localStorage as JSON:
```javascript
{
  gamesPlayed: number,
  gamesWon: number, 
  currentStreak: number,
  maxStreak: number,
  totalPoints: number,  // Cumulative points across all games
  achievements: string[], // Array of unlocked achievement IDs
  visitedBusinesses: string[], // Array of visited business names
  lastPlayed: string // Date string for streak tracking
}
```

## Achievement System
13 achievements across 4 categories:
- **Streak Achievements**: 3-day, 7-day, 15-day, 30-day streaks
- **Score Achievements**: Perfect score, lightning round, centurion (100 points)
- **Discovery Achievements**: First timer, explorer (10 wins), local expert (25 wins)
- **Visit Achievements**: Adventurer (1 visit), foodie (5 visits), local legend (15 visits)

## UI/UX Features

### Theme & Design
- **Catppuccin Mocha Theme**: Beautiful dark theme with consistent color palette
- **Compact Interface**: Smaller clues, hidden round info boxes for cleaner mobile UX
- **Auto-hiding Header**: Title section disappears after first wrong guess to save space
- **Floating Settings**: Bottom-right circular buttons for stats, Karl toggle, and debug
- **Enhanced Typography**: Monospace font for website links with better contrast

### Game Controls
- **Multiple Choice Button**: Skip directly to map phase with "🗺️ Multiple Choice" button
- **Give Up Option**: Traditional surrender with full hint reveal
- **Responsive Buttons**: Proper sizing and spacing for mobile devices

### Accessibility & Polish
- **High Contrast**: All text properly readable against Catppuccin backgrounds
- **Smooth Animations**: Polished hover effects and transitions
- **Mobile Optimized**: Touch-friendly buttons and compact layouts
- **Clean Victory Page**: Removed redundant "Your Guesses" section

### Original Features
- **Tutorial Modal**: Shows on first visit to explain gameplay
- **"Why Bizzle?" Modal**: Explains mission to support local businesses  
- **Karl the Fog**: Animated mascot that floats around (toggleable, disabled by default)
- **Visit Tracking**: "I Went There!" button for bonus points
- **Achievement Notifications**: Pop-up celebrations when unlocking badges
- **Enhanced Sharing**: Results include achievements in share text
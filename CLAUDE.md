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
- **index.html**: Contains all HTML, CSS, and JavaScript in one file (~1,080 lines)
- Pure vanilla JavaScript, no frameworks or dependencies
- Client-side only - no backend server required

### Core Components
- **Data Source**: Google Sheets API for business data storage
- **Game Logic**: 5-guess daily word game mechanics with fuzzy matching
- **Local Storage**: Game state persistence and statistics tracking
- **Responsive UI**: Mobile-friendly design with CSS Grid/Flexbox

### Key Configuration
- `SHEET_ID`: Google Sheets document ID (line 568)
- `API_KEY`: Google Sheets API key (line 569)
- Game starts from epoch date: '2025-08-10' (line 639, 645)
- Maximum guesses: 5 (line 751)
- Fuzzy match threshold: 0.8 (line 672)

### Data Flow
1. Fetch business data from Google Sheets on page load
2. Select daily business based on date rotation
3. Display photo and fun fact, reveal hints after wrong guesses
4. Track game state in localStorage for same-day persistence
5. Update statistics after game completion

### Customization Points
- Color scheme: CSS custom properties (lines 16, 42, etc.)
- Number of guesses: Line 751 condition
- Fuzzy matching sensitivity: Line 672 threshold value
- Sheet structure: Expected columns defined in README

## Google Sheets Integration
Business data format:
- Name, PhotoURL, FunFact, Neighborhood, Signature, Category, Street, Address, Website, DateToFeature
- Public sheet with viewer permissions required
- Images stored in Google Drive with direct link format
- Daily rotation based on DateToFeature column or automatic cycling
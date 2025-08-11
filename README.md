# Bizzle - Daily SF Business Guessing Game

A fun daily guessing game where players identify San Francisco small businesses through progressive hints!

## 🎮 How to Play

1. Visit the game at your GitHub Pages URL
2. Look at the interior photo and fun fact
3. Make your guess (up to 5 attempts)
4. Get a new hint after each wrong guess
5. Share your results with friends!

## 🏃‍♂️ Running Locally (Quick Start)

Want to play or test the game on your computer? It's super easy!

### Option 1: Direct Browser Opening (Simplest)
1. Download or clone this repository
2. Double-click the `index.html` file
3. The game opens in your default browser!

### Option 2: Using a Local Server (Recommended)
Running a local server prevents any potential CORS issues:

**With Python (Mac/Linux usually have this):**
```bash
# Navigate to the game folder, then:
python3 -m http.server 8000
# Visit http://localhost:8000 in your browser
```

**With Node.js:**
```bash
# If you have Node.js installed:
npx http-server -p 8000
# Visit http://localhost:8000 in your browser
```

**With PHP:**
```bash
# If you have PHP installed:
php -S localhost:8000
# Visit http://localhost:8000 in your browser
```

**Note:** You'll need an internet connection to fetch business data from Google Sheets and load images from Google Drive.

## 🚀 Deploying Online (GitHub Pages)

### Prerequisites
- A GitHub account (free at github.com)
- A Google account (for Sheets and Drive)
- Basic ability to copy/paste and follow instructions

### Step 1: Fork This Repository

1. Click the "Fork" button at the top of this repository
2. This creates your own copy of the game

### Step 2: Enable GitHub Pages

1. In your forked repository, go to Settings → Pages
2. Under "Source", select "Deploy from a branch"
3. Choose "main" branch and "/ (root)" folder
4. Click Save
5. Your game will be available at: `https://[your-username].github.io/Bizzle/`
6. Wait 5-10 minutes for the initial deployment

### Step 3: Set Up Your Google Sheet

The game uses a Google Sheet as its database. The sheet structure is already configured in the code.

#### Google Sheet is Already Set Up
The game comes pre-configured with a public Google Sheet. You can either:
- Use the existing sheet (read-only)
- Or create your own sheet following these steps:

#### To Create Your Own Sheet:

1. Create a new Google Sheet at sheets.google.com
2. Add these column headers in Row 1 (exact order and spelling):
   - `Name` - Business name
   - `PhotoURL` - Direct image link
   - `FunFact` - Interesting fact about the business
   - `Neighborhood` - SF neighborhood
   - `Signature` - Featured item/service with price
   - `Category` - Type of business
   - `Street` - Street name only (no number)
   - `Address` - Full address
   - `Website` - Optional website URL
   - `DateToFeature` - Date in YYYY-MM-DD format

3. Make your sheet public:
   - Click Share button (top right)
   - Click "Change to anyone with the link"
   - Set to "Viewer"
   - Copy the spreadsheet ID from the URL
   - The ID is the long string between `/d/` and `/edit`
   - Example: `https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit`

4. Update the code with your Sheet ID:
   - Edit `index.html`
   - Find line with `const SHEET_ID = `
   - Replace the ID with your sheet's ID

### Step 4: Set Up Google Drive for Images

1. Create a folder in Google Drive for your business photos
2. Upload interior photos of businesses (landscape orientation works best)
3. For each image:
   - Right-click → Share → Anyone with the link → Viewer
   - Copy the sharing link
   - Convert to direct link format:
     - From: `https://drive.google.com/file/d/FILE_ID/view`
     - To: `https://drive.google.com/uc?export=view&id=FILE_ID`
   - Use this converted link in the PhotoURL column

### Step 5: Add Your First Businesses

Add businesses to your Google Sheet with this information:

| Column | Example |
|--------|---------|
| Name | Bob's Donuts |
| PhotoURL | https://drive.google.com/uc?export=view&id=YOUR_IMAGE_ID |
| FunFact | Open 24 hours since 1959, this place has served over 50 types of donuts! |
| Neighborhood | Nob Hill |
| Signature | Apple Fritter ($3.50) |
| Category | Bakery |
| Street | Polk Street |
| Address | 1621 Polk Street, San Francisco, CA |
| Website | https://bobsdonutssf.com |
| DateToFeature | 2024-12-25 |

## 📝 Adding More Businesses

1. Add new rows to your Google Sheet
2. Follow the same format as above
3. The DateToFeature column is optional - if blank, businesses rotate automatically
4. Changes appear immediately (may need to clear browser cache)

## 🎯 Tips for Good Content

### Photos
- Use interior shots that are recognizable but not too obvious
- Avoid photos with visible business names/logos
- Landscape orientation (horizontal) works best
- Aim for 1MB or less per image

### Fun Facts
- Keep them interesting but not too revealing
- 1-2 sentences maximum
- Focus on history, unique features, or local trivia

### Hints Progression
Design your hints from vague to specific:
1. Neighborhood - Broad area
2. Signature item - More specific
3. Category - Business type
4. Street - Very specific location

## 🔧 Customization

### Changing Colors
Edit the CSS in `index.html`:
```css
/* Around line 20-30 */
--color-background: #FAFAFA;
--color-primary: #3B82F6;
--color-success: #22C55E;
--color-error: #EF4444;
```

### Changing Number of Guesses
Find and modify in the JavaScript:
```javascript
// Change 5 to your desired number
if (gameState.guesses.length >= 5) {
```

### Changing Fuzzy Match Threshold
Find the fuzzyMatch function and adjust:
```javascript
return matches / longer.length >= 0.8; // Change 0.8 to your threshold (0-1)
```

## 🐛 Troubleshooting

### Game Not Loading
- Check browser console for errors (F12 → Console)
- Verify your Google Sheet is public
- Ensure API key is valid
- Clear browser cache

### Images Not Showing
- Verify Google Drive links are converted to direct format
- Check image permissions are set to "Anyone with link"
- Test image URLs directly in browser

### Autocomplete Not Working
- Ensure businesses are properly loaded from sheet
- Check for JavaScript errors in console
- Verify Name column has values

### Share Button Not Working
- Some browsers require HTTPS for clipboard access
- Fallback copy method should work on all browsers
- Mobile browsers may have restrictions

## 📊 Game Statistics

The game automatically tracks:
- Total games played
- Win percentage
- Current winning streak
- Maximum winning streak

Stats are stored locally in the player's browser.

## 🔄 Daily Rotation

The game automatically shows a new business each day based on:
1. DateToFeature column (if specified)
2. Automatic rotation through all businesses (if no date specified)

## 🚢 Deployment Checklist

- [ ] Fork repository
- [ ] Enable GitHub Pages
- [ ] Set up Google Sheet with businesses
- [ ] Upload images to Google Drive
- [ ] Test game on mobile and desktop
- [ ] Share with friends!

## 📱 Mobile Support

The game is fully responsive and works on:
- iOS Safari
- Android Chrome
- Tablet browsers
- Desktop browsers

## 🔒 Privacy & Security

- No user data is collected or transmitted
- All game data stored locally in browser
- Google Sheets used in read-only mode
- No backend server required

## 🤝 Contributing

Feel free to:
- Fork and customize for your city
- Add new features
- Report bugs via GitHub Issues
- Share your customizations!

## 📄 License

This project is open source and available for anyone to use and modify.

## 🎉 Have Fun!

Enjoy discovering San Francisco's amazing small businesses! Share the game with friends and support local businesses by visiting them in person.

---

**Need Help?** Open an issue on GitHub or check the code comments for more details.
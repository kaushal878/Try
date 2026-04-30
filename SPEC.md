# Nepal Politics Daily Jokes Website

## Project Overview
- **Project Name**: Nepal Jokes Hub
- **Type**: Single-page interactive website
- **Core Functionality**: Display daily jokes related to Nepal politics, featuring funny MP quotes and political news satire
- **Target Users**: Nepali audiences looking for political humor and satire

## UI/UX Specification

### Layout Structure
- **Header**: Fixed navigation with logo and date display
- **Hero Section**: Daily joke highlight with animated reveal
- **Content Areas**: 
  - Daily Joke Card
  - MP Quotes Section
  - Political News Satire
  - Random Joke Generator
- **Footer**: Credits and date

### Responsive Breakpoints
- Mobile: < 768px
- Tablet: 768px - 1024px
- Desktop: > 1024px

### Visual Design

#### Color Palette
- **Primary**: #1a1a2e (Deep Navy)
- **Secondary**: #16213e (Dark Blue)
- **Accent**: #e94560 (Crimson Red - Nepal flag color)
- **Highlight**: #f1c40f (Golden Yellow)
- **Text Primary**: #ffffff
- **Text Secondary**: #a0a0a0
- **Card Background**: #0f3460

#### Typography
- **Headings**: 'Bebas Neue', sans-serif (bold, impactful)
- **Body**: 'Poppins', sans-serif
- **Accent/Quotes**: 'Merriweather', serif

#### Spacing System
- Section padding: 60px 20px
- Card padding: 24px
- Element margins: 16px

#### Visual Effects
- Card hover: scale(1.02) with box-shadow
- Quote reveal: fade-in animation
- Button hover: background shift with transition
- Background: Subtle Nepal pattern overlay

### Components

1. **Daily Joke Card**
   - Date display
   - Joke text with punchline
   - Category tag (Politics/General)
   - Share button

2. **MP Quotes Section**
   - Quote card with speaker name
   - Party affiliation badge
   - Source/News reference

3. **News Satire Cards**
   - Fake headline
   - Satirical description
   - "News Source" tag

4. **Random Joke Button**
   - Generates new joke on click
   - Loading animation

## Functionality Specification

### Core Features
1. **Daily Joke System**: Shows joke based on current date
2. **MP Quotes Database**: Collection of funny political quotes
3. **News Satire Section**: Fake but funny political news
4. **Random Joke Generator**: Button to show random joke
5. **Date Display**: Shows current Nepali and English date

### User Interactions
- Click "New Joke" button to get random joke
- Hover on cards for visual feedback
- Click to copy joke text

### Data Handling
- Local JavaScript array for jokes
- No backend required
- Static content delivery

## Acceptance Criteria
- [x] Website loads without errors
- [x] Daily joke displays based on date
- [x] At least 10 Nepal politics jokes available
- [x] At least 5 MP quotes displayed
- [x] At least 3 news satire items
- [x] Random joke button works
- [x] Responsive on mobile/tablet/desktop
- [x] Animations smooth and functional
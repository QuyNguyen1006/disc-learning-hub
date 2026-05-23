# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

DISC Learning Hub is a single-page interactive learning application for educational content about DISC personality assessment framework (Dominance, Influence, Steadiness, Conscientiousness). The application is entirely static HTML with embedded CSS and vanilla JavaScript — no build tools, frameworks, or server required.

**Key characteristics:**
- Single file architecture: `index.html` contains all HTML, CSS, and JavaScript
- Purely client-side: Uses localStorage for quiz progress (implicitly via `answers` object)
- Vietnamese language content for learning modules and quiz questions
- Interactive features: learning modules, 50-question quiz, progress tracking, results reporting

## How to Develop

### Opening the App

Simply open `index.html` in a web browser:
```bash
# From the project directory
open index.html
# or
cd /Users/qimacbookpro/Documents/skill/disc-learning-hub
open index.html
```

No dev server, build step, or dependencies needed.

### Key Code Sections

All code is in [index.html](index.html), organized into clear sections:

1. **Styles** (lines 7-89): Embedded CSS covering layout, DISC color scheme, components (buttons, cards, tabs, accordions, quiz options)
2. **Learning Content** (lines 231-346): `KNOWLEDGE` object with 6 learning modules:
   - `overview` — DISC symbols, keywords, core characteristics
   - `strengths` — strengths, weaknesses, stress patterns
   - `management` — management styles and team expectations
   - `conflict` — common conflicts between DISC pairs
   - `communication` — communication approaches and feedback strategies
   - `stress` — stress responses and manager guidance
3. **Quiz Questions** (lines 348-399): `QUESTIONS` array with 50 questions across 9 categories
4. **JavaScript** (lines 225-567): State management and UI rendering

### Editing Learning Content

Update the `KNOWLEDGE` object (starting at line 231). Structure for each learning module:
```javascript
{
  title: "Module Title",
  sections: [
    {
      heading: "Section Heading",
      items: [
        {
          disc: "D",  // or I, S, C
          detail: "Text content",
          symbol: "🔴 Symbol",
          keywords: "keyword1 · keyword2",
          quote: "Quoted phrase"
        }
      ]
    }
  ]
}
```

### Editing Quiz Questions

Update the `QUESTIONS` array (starting at line 348). Structure for each question:
```javascript
{
  id: 1,
  title: "Question text",
  cat: "Category name",
  options: {
    D: "D option text",
    I: "I option text",
    S: "S option text",
    C: "C option text"
  }
}
```

**Current categories** (used for tab organization and results):
- Giao tiếp & Tương tác (Communication & Interaction) — 7 questions
- Nhận việc & Xử lý công việc (Receiving Work & Task Management) — 6 questions
- Quan hệ & Teamwork (Relationships & Teamwork) — 4 questions
- Phản ứng với áp lực & thay đổi (Stress & Change Response) — 4 questions
- Quản lý & Lãnh đạo (Management & Leadership) — 6 questions
- Giao tiếp & Xã hội (Communication & Social) — 7 questions
- Mua sắm & Ra quyết định (Shopping & Decision Making) — 3 questions
- Thói quen & Lối sống (Habits & Lifestyle) — 4 questions
- Khi gặp vấn đề trong cuộc sống (Life Problems) — 4 questions
- Tổng kết (Summary) — 1 question

### DISC Color Scheme

Four primary colors used throughout the app:
- **D (Dominance):** Red `#E53935`
- **I (Influence):** Amber `#F59E0B`
- **S (Steadiness):** Green `#16A34A`
- **C (Conscientiousness):** Blue `#1D4ED8`

These are defined in `DISC_COLORS` object (line 226) and CSS classes follow the pattern `.d-bg`, `.d-color`, `.badge-d`, etc.

## Application Architecture

### Pages (View States)

The app uses a multi-page pattern with `.page` divs controlled by a `display: none/block` toggle:

1. **page-home** (id: `page-home`) — Home page with 4 DISC cards, 6 learning topic buttons, and quiz CTA
2. **page-learn** (id: `page-learn`) — Learning module display with back button
3. **page-quiz** (id: `page-quiz`) — Quiz interface with tabs, accordion questions, progress bar, navigation buttons
4. **page-result** (id: `page-result`) — Results summary with category breakdown and replay options

### State Management

- **`answers`** (line 402): Object tracking quiz responses, keyed by question ID with DISC letter as value
- **`activeCat`** (line 402): Current active tab index in quiz (corresponds to `CATS` array position)
- **`expandedQ`** (line 402): ID of currently expanded question in quiz accordion
- **`CATS`** (line 401): Derived from unique question categories, used for tab rendering

No persistence layer — refreshing the page resets progress. Consider adding localStorage if persistence is needed.

### Rendering Functions

- **`showHome()`, `showLearn()`, `showQuiz()`, `showResult()`** (lines 404-419) — Page navigation and state reset
- **`renderLearn(id)`** (lines 422-443) — Builds learning module HTML from `KNOWLEDGE` object
- **`renderQuizTabs()`** (lines 445-453) — Renders category tabs with completion indicators
- **`renderQuizQuestions()`** (lines 455-485) — Renders accordion questions for current category, updates nav button states
- **`renderOptions(q)`** (lines 487-499) — Renders 4 DISC option buttons for a question
- **`selectAnswer(qId, disc)`** (lines 501-533) — Handles answer selection, updates UI, checks if quiz complete
- **`renderResult()`** (lines 550-567) — Builds results page with category progress bars

### Responsive Design

The layout is mobile-first with max-width 680px container. Uses flexbox and grid for responsive layouts. Cards, buttons, and tabs are touch-friendly with appropriate spacing and tap targets.

## Common Tasks

### Add a New Learning Module

1. Add a new entry to `KNOWLEDGE` object (line 231)
2. Add a button to home page calling `showLearn('newId')`
3. Follow the structure of existing modules for consistency

### Add Quiz Questions

1. Add entries to `QUESTIONS` array (line 348)
2. Auto-categorization happens via `CATS` derived from unique `cat` values
3. Keep question count reasonable — tabs appear for each category

### Change DISC Colors

1. Update `DISC_COLORS` object (line 226)
2. Update corresponding CSS classes `.d-bg`, `.i-bg`, etc. (lines 17-21)
3. Update badge colors `.badge-d`, `.badge-i`, etc. (line 24)
4. Update selected option colors `.selected-d`, `.selected-i`, etc. (lines 50-53)

### Modify Quiz Behavior

The quiz tracks answers in the `answers` object. Current rules:
- 50 questions required for completion
- Clicking an answer again deselects it (toggle behavior)
- Result page shows emoji based on score (🏆 45+, 🌟 30+, 📚 <30)

To change, modify the relevant functions (e.g., `selectAnswer`, `renderResult`)

## Deployment

Simply upload `index.html` and the `document/` folder to any web server. No build step, no dependencies, no environment variables needed. The site is completely self-contained and can be served from GitHub Pages, Netlify, a CDN, or any static host.

## Git Workflow

- Remote: `origin` → https://github.com/QuyNguyen1006/disc-learning-hub.git
- Main branch: `main`
- Workflow: Edit `index.html`, test locally in browser, commit, push

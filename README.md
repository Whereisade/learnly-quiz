# Science Quiz App

A mini quiz application built with Vue.js featuring both a tap-to-reveal screen and swipe UI options. This application is responsive and designed for mobile experience.

## Features

1. **Tap-to-Reveal UI**: Users tap on cards about states of matter, and they flip to reveal more information.
2. **Swipe UI**: Users are presented with cards representing changes in materials and must swipe them in the correct direction (physical changes to the left, chemical changes to the right).
3. **Success and Error States**: The application provides clear feedback when users make correct or incorrect choices.
4. **Mobile-Responsive Design**: The application works well on both desktop and mobile devices.

## Project Setup

```bash
# Install dependencies
npm install

# Run the development server
npm run dev

# Build for production
npm run build

# Preview the production build
npm run preview
```

## Usage Instructions

1. Start with the "Tap to Discover States of Matter" screen.
2. Tap each card to reveal information about the different states of matter.
3. After revealing all cards, click the "Next" button.
4. In the "Swipe" screen, determine if the change is physical or chemical.
5. Swipe physical changes to the left, chemical changes to the right.
6. After completing all cards, you'll see a success message.
7. You can click "Try Again" to restart the quiz.

## Technologies Used

- Vue.js 3
- TailwindCSS
- Vite

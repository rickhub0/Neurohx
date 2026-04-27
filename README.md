# Neurohx

Neurohx is a mental wellness platform designed to support individuals in their journey of reflection, journaling, and personal growth. By leveraging the power of AI, Neurohx offers personalized insights and prompts that guide users in their introspection and emotional well-being.

## Overview

Neurohx.com is a hardened AI mental wellness platform that combines the power of AI with secure data management to provide users with a safe space for reflection, journaling, and personal development.

## Key Features

- **AI-Powered Reflection**: Advanced AI algorithms analyze your journaling entries to provide meaningful feedback and insights that enhance self-awareness
- **Secure Journaling**: A user-friendly interface that encourages daily journaling practices with end-to-end security through Firebase
- **Personal Growth Resources**: Access curated resources and activities that promote mental wellness and personal development
- **Real-time Analytics**: Track your emotional patterns and growth over time with interactive charts and visualizations
- **Payment Integration**: Seamless Razorpay integration for premium features and subscription management

## Technology Stack

- **Frontend**: React 19 with TypeScript, Vite, and Tailwind CSS
- **Backend**: Express.js with Node.js runtime
- **AI Integration**: Google Gemini API for intelligent analysis and recommendations
- **Database**: Firebase/Firestore for secure, real-time data storage
- **Deployment**: Vercel for fast and reliable hosting
- **Additional Tools**: 
  - React Router for navigation
  - Recharts for data visualization
  - Motion for smooth animations
  - jsPDF for document export

## Getting Started

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn package manager

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/rickhub0/Neurohx.git
   cd Neurohx
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Set up environment variables:
   ```bash
   cp .env.example .env.local
   ```
   Add your Gemini API key to `.env.local`:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

4. Run the development server:
   ```bash
   npm run dev
   ```

   The app will be available at `http://localhost:5173`

### Build for Production

```bash
npm run build
```

The optimized build will be in the `dist` directory.

### Preview Production Build

```bash
npm run preview
```

## Project Structure

```
Neurohx/
├── src/                    # Source code
├── api/                    # API routes and handlers
├── public/                 # Static assets
├── firebase-*.json         # Firebase configuration files
├── firestore.rules         # Firestore security rules
├── vite.config.ts         # Vite configuration
├── tsconfig.json          # TypeScript configuration
└── package.json           # Project dependencies
```

## Security

This project includes hardened Firestore security rules to ensure user data privacy and protection. Review `firestore.rules` for details on data access policies.

## Live Demo

Visit the live application at: [https://neurohx-nine.vercel.app](https://neurohx-nine.vercel.app)

## Contributing

We welcome contributions! Feel free to open issues and pull requests to help improve Neurohx.

## License

This project is open source and available under the MIT License.

## Support

For questions or issues, please open an issue on the GitHub repository or contact the maintainers.

---

Built with ❤️ for mental wellness and personal growth.
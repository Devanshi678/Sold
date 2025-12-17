# Sold - AI-Powered Multi-Platform Marketplace App

**CIS553 Fall 2025 - Software Engineering Project**

Sell anything instantly by taking a photo. Our AI handles pricing, multi-platform posting, buyer communication, and pickup scheduling automatically.

## 🎯 Project Overview

Sold revolutionizes online selling by automating the entire process:
1. 📸 Take a photo of your item
2. 🤖 AI analyzes and suggests optimal pricing
3. 🚀 One-click post to Facebook Marketplace, Craigslist, OfferUp, 5miles
4. 💬 AI agent chats with buyers to negotiate and schedule pickup
5. 💰 Get paid via Venmo/Zelle

## 📚 Documentation

**📋 [Complete Documentation Index](./docs/INDEX.md)** - All documentation organized

**Quick Links**:
- [Project Proposal](./docs/PROJECT_PROPOSAL.md) - Main submission (Due 10/3)
- [Current Status](./docs/project/CURRENT_STATUS.md) - What's built vs. planned
- [Start App Guide](./docs/guides/START_APP.md) - How to run locally
- [Facebook API Guide](./docs/guides/FACEBOOK_API_GUIDE.md) - Complete integration guide

**Planning**:
- [Team Tasks](./docs/planning/TEAM_TASKS.md) - Sprint planning & assignments
- [Tech Stack](./docs/planning/TECH_STACK.md) - Architecture & technology decisions
- [Requirements Mapping](./docs/planning/REQUIREMENTS_MAPPING.md) - CIS553 compliance

## 🏗️ Architecture

```
sold/
├── backend/          # Express.js REST API (TypeScript)
├── mobile/           # React Native app (iOS & Android)
├── shared/           # Shared TypeScript types & validators
├── docs/             # Documentation
└── .github/          # CI/CD workflows
```

### Tech Stack

**Backend**
- Node.js 20 + Express.js + TypeScript
- PostgreSQL 15 (Prisma ORM)
- Redis (Bull queue for jobs)
- AWS S3 (image storage)

**Mobile**
- React Native 0.72+
- TypeScript
- Zustand (state management)
- React Navigation

**AI/ML Services** (Third-party APIs)
- OpenAI Vision API (image analysis)
- OpenAI GPT-4 (conversational agent)

**Integrations**
- Google/Facebook OAuth
- Stripe (payments)
- Google Calendar
- Marketplace APIs (Facebook, Craigslist, etc.)

## 🚀 Quick Start

### Automated Setup (Recommended)

We provide automated setup scripts for easy onboarding:

#### macOS
```bash
# Clone the repository
git clone <repository-url>
cd sold

# Run the setup script
chmod +x setup-mac.sh
./setup-mac.sh
```

The script will:
- ✅ Install Homebrew (if needed)
- ✅ Install Node.js 20, pnpm, PostgreSQL, Redis, Watchman
- ✅ Install project dependencies
- ✅ Set up environment files
- ✅ Create database and run migrations
- ✅ Optionally start iOS simulator or web browser

#### Windows / Linux / WSL
```bash
# Clone the repository
git clone <repository-url>
cd sold

# Run the setup script (in Git Bash, WSL, or Linux terminal)
chmod +x setup-pc.sh
./setup-pc.sh
```

The script will:
- ✅ Check for Node.js 20+ and pnpm
- ✅ Guide PostgreSQL and Redis installation
- ✅ Install project dependencies
- ✅ Set up environment files
- ✅ Create database and run migrations
- ✅ Choose between Android emulator or web browser

---

### Manual Setup

<details>
<summary>Click to expand manual installation steps</summary>

#### Prerequisites

**Required:**
- Node.js 20+ ([Download](https://nodejs.org/))
- pnpm 8+ (`npm install -g pnpm`)
- PostgreSQL 15 ([Download](https://www.postgresql.org/download/))

**Optional (for full features):**
- Redis 7 (for background jobs)
- AWS account (for S3 image storage)
- OpenAI API key (for AI features)

**Platform-Specific:**
- **macOS**: Xcode (for iOS simulator), CocoaPods, Watchman
- **Windows**: Android Studio (for Android emulator)
- **Linux/WSL**: Android Studio (for Android emulator)

#### Installation Steps

1. **Install pnpm globally**
   ```bash
   npm install -g pnpm
   ```

2. **Install project dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables**
   ```bash
   # Backend environment
   cp backend/.env.example backend/.env
   # Edit backend/.env with your credentials
   ```

4. **Set up database**
   ```bash
   # Create database (PostgreSQL must be running)
   createdb sold_dev

   # Run migrations
   cd backend
   pnpm prisma generate
   pnpm prisma migrate dev
   cd ..
   ```

5. **Build shared package**
   ```bash
   pnpm shared:build
   ```

6. **Start development servers**
   ```bash
   # Terminal 1: Backend
   pnpm backend:dev

   # Terminal 2: Mobile (choose one)
   pnpm mobile:ios      # iOS simulator (macOS only)
   pnpm mobile:android  # Android emulator
   pnpm mobile:web      # Web browser (easiest)
   ```

</details>

---

### Platform-Specific Setup

#### iOS Development (macOS only)

**Requirements:**
- macOS 12+ (Monterey or later)
- Xcode 14+ ([Download from App Store](https://apps.apple.com/us/app/xcode/id497799835))
- CocoaPods (`sudo gem install cocoapods`)
- Watchman (`brew install watchman`)

**Setup:**
```bash
# Accept Xcode license
sudo xcodebuild -license accept

# Install CocoaPods dependencies (if needed)
cd mobile/ios && pod install && cd ../..

# Start iOS simulator
pnpm mobile:ios
```

#### Android Development (All platforms)

**Requirements:**
- Android Studio ([Download](https://developer.android.com/studio))
- Java JDK 11+
- Android SDK (API level 33+)

**Setup:**
1. Install Android Studio
2. Open Android Studio > Settings > Android SDK
3. Install SDK Platform 33 and SDK Build-Tools
4. Create a virtual device (AVD) via AVD Manager
5. Set environment variables:
   ```bash
   # Add to ~/.bashrc, ~/.zshrc, or ~/.bash_profile
   export ANDROID_HOME=$HOME/Library/Android/sdk  # macOS
   export ANDROID_HOME=$HOME/Android/Sdk          # Linux/WSL
   export PATH=$PATH:$ANDROID_HOME/emulator
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   ```
6. Start emulator and run:
   ```bash
   pnpm mobile:android
   ```

#### Web Development (All platforms - Easiest!)

**Requirements:**
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Node.js 20+

**Setup:**
```bash
# Just run the web server
pnpm mobile:web
```

Your default browser will open automatically at `http://localhost:19006`

---

### Environment Configuration

After setup, edit `backend/.env` with your API keys:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/sold_dev"

# Redis (optional for development)
REDIS_URL="redis://localhost:6379"

# JWT
JWT_SECRET="your-secret-key-here"
JWT_REFRESH_SECRET="your-refresh-secret-here"

# OpenAI (required for AI features)
OPENAI_API_KEY="sk-..."

# AWS S3 (required for image uploads)
AWS_ACCESS_KEY_ID="..."
AWS_SECRET_ACCESS_KEY="..."
AWS_S3_BUCKET="sold-images-dev"
AWS_REGION="us-east-1"

# Google OAuth (optional)
GOOGLE_CLIENT_ID="..."
GOOGLE_CLIENT_SECRET="..."

# Facebook OAuth (optional)
FACEBOOK_APP_ID="..."
FACEBOOK_APP_SECRET="..."

# Stripe (optional)
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLISHABLE_KEY="pk_test_..."
```

---

### Troubleshooting

**"Command not found: pnpm"**
```bash
npm install -g pnpm
```

**PostgreSQL connection error**
```bash
# macOS
brew services start postgresql@15

# Linux/WSL
sudo service postgresql start

# Windows
# Start PostgreSQL via Services or pgAdmin
```

**"ANDROID_HOME not set"**
```bash
# Add to your shell profile
export ANDROID_HOME=$HOME/Library/Android/sdk  # macOS
export ANDROID_HOME=$HOME/Android/Sdk          # Linux
```

**Port already in use**
```bash
# Backend (default: 3000)
lsof -ti:3000 | xargs kill -9

# Expo (default: 19000)
lsof -ti:19000 | xargs kill -9
```

**iOS build fails**
```bash
cd mobile/ios
pod install
cd ../..
```

**"Watchman crawl failed" (macOS)**
```bash
watchman watch-del-all
```

## 🧪 Testing

```bash
# Run all tests
pnpm test

# Run backend tests with coverage
pnpm backend:test

# Run mobile tests
pnpm mobile:test

# Run linter
pnpm lint

# Format code
pnpm format
```

## 📦 Monorepo Structure

### Backend (`/backend`)
- `src/routes/` - API endpoints
- `src/controllers/` - Business logic
- `src/services/` - External integrations (OpenAI, Stripe, etc.)
- `src/middleware/` - Auth, validation, error handling
- `prisma/` - Database schema & migrations
- `tests/` - Unit & integration tests

### Mobile (`/mobile`)
- `src/screens/` - App screens (Auth, Camera, Listings, etc.)
- `src/components/` - Reusable UI components
- `src/navigation/` - React Navigation setup
- `src/store/` - Zustand state management
- `src/services/` - API client

### Shared (`/shared`)
- `src/types/` - TypeScript interfaces
- `src/constants/` - Shared constants
- `src/validators/` - Zod validation schemas

## 🔄 Development Workflow

1. **Create feature branch**: `git checkout -b feature/task-name`
2. **Write tests first** (TDD approach)
3. **Implement feature**
4. **Run tests**: `pnpm test`
5. **Commit**: `git commit -m "feat: add feature description"`
6. **Push & create PR**: GitHub Actions runs CI
7. **Code review** (1 approval required)
8. **Merge to main** (auto-deploy to staging)

## 📅 Timeline

**14-week development schedule** (7 sprints)

| Sprint | Dates | Deliverable |
|--------|-------|-------------|
| 1 | Oct 2-15 | Foundation + Auth |
| 2 | Oct 16-29 | Profiles + AI Pricing |
| 3 | Oct 30-Nov 12 | Item Creation + Platform Posting |
| 4 | Nov 13-26 | AI Chat + Scheduling |
| 5 | Nov 27-Dec 10 | Payments + Listings |
| 6 | Dec 11-24 | Notifications + Deployment |
| 7 | Dec 25-Jan 7 | App Stores + Launch |

## 👥 Team

**6-Person Development Team**

- Backend Dev 1: Authentication, User Management, Images
- Backend Dev 2: Platform Integrations, AI Services, Payments
- Mobile Dev 1: Auth, Camera, Item Creation
- Mobile Dev 2: Listings, Conversations, Notifications
- DevOps Engineer: Infrastructure, CI/CD, Deployment
- AI Engineer: Price Detection, Chat Agent, NLP

## 📊 Project Management

- **Jira**: Sprint planning, task tracking
- **GitHub**: Source control, PR reviews
- **Confluence**: Documentation (optional)

## 🔐 Security

- Bcrypt password hashing (12 rounds)
- JWT tokens (15min access, 7day refresh)
- HTTPS only
- Input validation (Zod schemas)
- SQL injection prevention (Prisma)
- File upload validation
- Rate limiting

## 💰 Monetization

**Revenue Models** (A/B testing):
- Per-post: $0.50/post
- Subscription: $6/month unlimited
- Transaction fee: 5% of sale

## 📝 License

This project is part of CIS553 coursework at UMD.

## 🤝 Contributing

1. Follow the coding standards (ESLint + Prettier)
2. Write tests for new features
3. Update documentation
4. Create meaningful commit messages (Conventional Commits)
5. Request code review before merging

## 🐛 Known Issues / TODO

- [ ] Facebook Marketplace API research (may need manual posting)
- [ ] Legal review for platform automation
- [ ] Instructor approval for OpenAI API usage
- [ ] Team size confirmation (6 vs 5 members)

## 📞 Contact

**Project Lead**: Broderick Higby
**Course**: CIS553 Fall 2025
**Institution**: University of Michigan Dearborn

---

**Phase 1 Status**: ✅ Complete
- [x] Requirements reviewed
- [x] Tech stack defined
- [x] Database schema designed
- [x] Monorepo structure created
- [x] Testing framework configured
- [ ] Backend auth implementation (next)

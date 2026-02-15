# Tasks: Zirna IO - AI-Powered Exam Preparation Platform

## Task 1: Multi-Board Educational Content Hierarchy

- [ ] 1.1 Create database migrations for Board, Institution, Program, Subject, Chapter tables
  - [ ] 1.1.1 Add board_type enum (academic, competitive_exam, professional)
  - [ ] 1.1.2 Add accessible_boards array field to Chapter
  - [ ] 1.1.3 Add is_global boolean field to Chapter
- [ ] 1.2 Implement Board CRUD API endpoints
- [ ] 1.3 Implement Institution CRUD API endpoints
- [ ] 1.4 Implement Program CRUD API endpoints
- [ ] 1.5 Implement Subject CRUD API endpoints
- [ ] 1.6 Implement Chapter CRUD API endpoints with board filtering
- [ ] 1.7 Implement content filtering logic for board-specific and global chapters
- [ ] 1.8 Write property test for Board-Based Content Filtering (Property 1)

## Task 2: Chapter Content Processing

- [ ] 2.1 Implement LlamaParse integration for PDF parsing
- [ ] 2.2 Create ChapterChunk table with tsvector and pgvector columns
- [ ] 2.3 Implement chunking service to split content into semantic chunks
- [ ] 2.4 Implement embedding generation for both keyword and semantic vectors
- [ ] 2.5 Implement processing status state machine (PENDING → PROCESSING → COMPLETED/FAILED)
- [ ] 2.6 Create API endpoint for document upload and processing
- [ ] 2.7 Write property test for Chapter Processing State Machine (Property 2)
- [ ] 2.8 Write property test for Chunk Ordering Invariant (Property 3)
- [ ] 2.9 Write property test for Chunk Embedding Completeness (Property 4)

## Task 3: AI-Powered Chat Tutor

- [ ] 3.1 Implement Hybrid Search Service with RRF scoring
  - [ ] 3.1.1 Implement tsvector keyword search
  - [ ] 3.1.2 Implement pgvector semantic search
  - [ ] 3.1.3 Implement RRF (Reciprocal Rank Fusion) scoring
- [ ] 3.2 Implement AI Service with multi-provider support (Gemini, OpenAI, Anthropic, OpenRouter)
- [ ] 3.3 Create Conversation and ConversationMessage tables
- [ ] 3.4 Implement chat API endpoint with streaming response
- [ ] 3.5 Implement source citation with page numbers
- [ ] 3.6 Add multi-language support (English, Hindi, Mizo)
- [ ] 3.7 Write property test for Hybrid Search RRF Scoring (Property 5)
- [ ] 3.8 Write property test for Conversation Context Association (Property 6)
- [ ] 3.9 Write property test for Source Citation Completeness (Property 7)

## Task 4: AI-Generated Study Materials

- [ ] 4.1 Implement summary generation with key points
- [ ] 4.2 Implement flashcard generation (front/back pairs)
- [ ] 4.3 Implement mind map generation in Mermaid.js syntax
- [ ] 4.4 Implement definitions generation (term/definition pairs)
- [ ] 4.5 Create StudyMaterial table and API endpoints
- [ ] 4.6 Implement YouTube video integration with search queries
- [ ] 4.7 Write property test for Study Material Structure Validity (Property 8)
- [ ] 4.8 Write property test for Mind Map Syntax Validity (Property 9)

## Task 5: Quiz Generation and Assessment

- [ ] 5.1 Create Question table with type, difficulty, and explanation fields
- [ ] 5.2 Create Quiz and QuizQuestion tables
- [ ] 5.3 Implement Question Bank selection with fallback to AI generation
- [ ] 5.4 Implement quiz generation API with configurable options
- [ ] 5.5 Implement answer hiding for client-side quiz display
- [ ] 5.6 Implement auto-grading for objective questions (MCQ, TRUE_FALSE, FILL_IN_BLANK)
- [ ] 5.7 Implement AI grading for subjective questions (SHORT_ANSWER, LONG_ANSWER)
- [ ] 5.8 Implement points award on quiz completion
- [ ] 5.9 Implement timed question support
- [ ] 5.10 Write property test for Question Source Selection Priority (Property 10)
- [ ] 5.11 Write property test for Quiz Answer Hiding (Property 11)
- [ ] 5.12 Write property test for Objective Question Auto-Grading (Property 12)
- [ ] 5.13 Write property test for Quiz Completion Points Award (Property 13)

## Task 6: Question Bank Management

- [ ] 6.1 Implement Question CRUD API endpoints
- [ ] 6.2 Implement CSV import for bulk question upload
- [ ] 6.3 Implement AI question generation from chapter content
- [ ] 6.4 Implement question filtering by subject/chapter/difficulty
- [ ] 6.5 Implement question activation/deactivation
- [ ] 6.6 Implement point value calculation based on type and difficulty

## Task 7: P2P Battle System

- [ ] 7.1 Create Battle and BattleParticipant tables
- [ ] 7.2 Implement battle creation with unique 6-digit code generation
- [ ] 7.3 Implement battle join API endpoint
- [ ] 7.4 Integrate Supabase Realtime for battle events
- [ ] 7.5 Implement battle start logic (creator only)
- [ ] 7.6 Implement real-time score updates and progress tracking
- [ ] 7.7 Implement battle completion and winner determination
- [ ] 7.8 Implement rematch functionality
- [ ] 7.9 Implement battle abandonment on timeout
- [ ] 7.10 Write property test for Battle Code Uniqueness (Property 14)
- [ ] 7.11 Write property test for Battle Status State Machine (Property 15)
- [ ] 7.12 Write property test for Battle Completion Condition (Property 16)

## Task 8: Gamification System

- [ ] 8.1 Create UserPoints table
- [ ] 8.2 Create StreakBadge and UserBadge tables
- [ ] 8.3 Implement points award service
- [ ] 8.4 Implement streak calculation logic
- [ ] 8.5 Implement badge milestone checking and award
- [ ] 8.6 Implement leaderboard API endpoint
- [ ] 8.7 Write property test for Streak Calculation Correctness (Property 17)
- [ ] 8.8 Write property test for Badge Award Idempotence (Property 18)

## Task 9: Course and Enrollment Management

- [ ] 9.1 Create Course table with pricing fields (INR)
- [ ] 9.2 Create UserEnrollment table with trial fields
- [ ] 9.3 Implement enrollment API with 3-day trial period
- [ ] 9.4 Implement Trial Access Service
  - [ ] 9.4.1 Implement trial status checking
  - [ ] 9.4.2 Implement AI feature access restriction for trial users
  - [ ] 9.4.3 Implement chapter-level access control (Chapter 1 only for trial)
- [ ] 9.5 Implement enrollment progress tracking
- [ ] 9.6 Write property test for Trial Period Duration (Property 19)
- [ ] 9.7 Write property test for Trial AI Access Restriction (Property 20)
- [ ] 9.8 Write property test for Trial Expiration Access Block (Property 21)
- [ ] 9.9 Write property test for Enrollment Progress Bounds (Property 27)

## Task 10: User Authentication and Authorization

- [ ] 10.1 Configure NextAuth.js with credentials provider
- [ ] 10.2 Configure Google OAuth provider
- [ ] 10.3 Implement role-based access control middleware
- [ ] 10.4 Implement user registration API
- [ ] 10.5 Implement login tracking (last_login timestamp)
- [ ] 10.6 Implement session management

## Task 11: AI API Key Management

- [ ] 11.1 Create AiApiKey table with encryption
- [ ] 11.2 Implement API key CRUD endpoints (admin only)
- [ ] 11.3 Implement key selection by priority
- [ ] 11.4 Implement automatic failover on API errors
- [ ] 11.5 Implement success/error count tracking
- [ ] 11.6 Write property test for API Key Selection Priority (Property 22)
- [ ] 11.7 Write property test for API Key Failover (Property 23)

## Task 12: AI Textbook Generator

- [ ] 12.1 Create Syllabus, SyllabusUnit, SyllabusChapter tables
- [ ] 12.2 Create Textbook, TextbookUnit, TextbookChapter tables
- [ ] 12.3 Implement syllabus parsing service
- [ ] 12.4 Implement chapter content generation with style options
- [ ] 12.5 Implement exam-relevant highlights generation
- [ ] 12.6 Implement image generation for diagrams
- [ ] 12.7 Implement PDF compilation service
- [ ] 12.8 Implement async job queue for generation
- [ ] 12.9 Write property test for Syllabus Parsing Structure (Property 24)

## Task 13: Subscription and Payment Management

- [ ] 13.1 Create SubscriptionPlan table
- [ ] 13.2 Create UserSubscription table with Razorpay fields
- [ ] 13.3 Implement Razorpay checkout integration (INR)
- [ ] 13.4 Implement Razorpay webhook handlers
- [ ] 13.5 Implement subscription status management
- [ ] 13.6 Implement subscription cancellation with access until period end

## Task 14: Usage Tracking and Limits

- [ ] 14.1 Create UsageTracking table
- [ ] 14.2 Implement usage increment service
- [ ] 14.3 Implement usage limit checking middleware
- [ ] 14.4 Implement usage reset on billing period start
- [ ] 14.5 Implement configurable limits per subscription plan
- [ ] 14.6 Write property test for Usage Limit Enforcement (Property 25)

## Task 15: Response Caching

- [ ] 15.1 Create AiResponseCache table
- [ ] 15.2 Implement cache key generation
- [ ] 15.3 Implement cache lookup and hit tracking
- [ ] 15.4 Implement cache expiration
- [ ] 15.5 Implement targeted cache invalidation by chapter/subject
- [ ] 15.6 Write property test for Cache Hit Behavior (Property 26)

## Task 16: Frontend Components

- [ ] 16.1 Implement student dashboard with enrolled courses
- [ ] 16.2 Implement course catalog with filtering
- [ ] 16.3 Implement chapter content viewer
- [ ] 16.4 Implement AI chat interface with streaming
- [ ] 16.5 Implement quiz taking interface with timer
- [ ] 16.6 Implement quiz results display
- [ ] 16.7 Implement battle lobby and gameplay UI
- [ ] 16.8 Implement leaderboard display
- [ ] 16.9 Implement streak and badge display
- [ ] 16.10 Implement subscription management UI

## Task 17: Admin Dashboard

- [ ] 17.1 Implement content hierarchy management UI
- [ ] 17.2 Implement document upload and processing UI
- [ ] 17.3 Implement question bank management UI
- [ ] 17.4 Implement API key management UI
- [ ] 17.5 Implement textbook generator UI
- [ ] 17.6 Implement user management UI
- [ ] 17.7 Implement analytics dashboard

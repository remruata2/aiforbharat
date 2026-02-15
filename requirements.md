# Requirements Document

## Introduction

Zirna IO is a comprehensive AI-powered exam preparation SaaS platform designed to serve students across Indian educational boards (CBSE, MBSE, state boards) and competitive examinations (IIT-JEE, NEET, UPSC). The platform combines intelligent content management, RAG-based AI tutoring, gamified learning experiences, and real-time competitive features to create an engaging and effective study environment for Indian students.

## Glossary

- **Board**: An Indian educational authority that sets curriculum and conducts examinations (e.g., CBSE, MBSE, state boards)
- **Program**: A specific course of study within a board (e.g., Class 10, Class 12 Science/Arts/Commerce, UPSC Civil Services)
- **Subject**: An academic discipline within a program (e.g., Physics, Mathematics, General Studies)
- **Chapter**: A unit of content within a subject containing parsed educational material
- **Chunk**: A semantic segment of chapter content used for RAG-based search
- **RAG**: Retrieval-Augmented Generation - AI technique combining search with generation
- **Hybrid_Search**: Combined keyword (tsvector) and semantic (pgvector) search using RRF scoring
- **Quiz**: A set of questions generated from chapter content for assessment
- **Battle**: A real-time P2P competitive quiz session between two students
- **Enrollment**: A user's registration in a specific course with access permissions
- **Trial_Period**: A 3-day free access period for paid courses with limited AI features
- **Streak**: Consecutive days of learning activity tracked for gamification
- **Textbook_Generator**: AI system that creates complete textbooks from syllabus input
- **LlamaParse**: Document parsing service for extracting structured content from PDFs
- **Supabase_Realtime**: WebSocket-based real-time communication service for battles
- **NCERT**: National Council of Educational Research and Training - provides standard textbooks used across multiple Indian boards

## Requirements

### Requirement 1: Multi-Board Educational Content Hierarchy

**User Story:** As an administrator, I want to manage educational content in a hierarchical structure, so that students can access board-specific curriculum organized by institution, program, subject, and chapter.

#### Acceptance Criteria

1. THE Content_Management_System SHALL support a hierarchy of Board → Institution → Program → Subject → Chapter
2. WHEN an administrator creates a new board, THE System SHALL require board type (academic, competitive_exam, professional)
3. WHEN an administrator creates a program, THE System SHALL associate it with a board and optionally an institution
4. WHEN a chapter is marked as global, THE System SHALL make it accessible to all boards
5. WHEN a chapter has accessible_boards defined, THE System SHALL restrict access to only those specified boards
6. THE System SHALL support cross-board content sharing for NCERT curricula used by multiple state boards
7. WHEN filtering content by board, THE System SHALL include both board-specific and global chapters


### Requirement 2: Chapter Content Processing

**User Story:** As an administrator, I want to upload and parse educational documents, so that chapter content is automatically chunked and indexed for AI-powered search.

#### Acceptance Criteria

1. WHEN an administrator uploads a PDF document, THE Chapter_Processor SHALL parse it using LlamaParse
2. WHEN parsing completes, THE System SHALL store the parsed content as JSON in the chapter record
3. THE Chunking_Service SHALL split chapter content into semantic chunks with page number references
4. WHEN a chunk is created, THE System SHALL generate both tsvector (keyword) and pgvector (semantic) embeddings
5. IF parsing fails, THEN THE System SHALL set processing_status to FAILED and store the error message
6. WHEN processing completes successfully, THE System SHALL set processing_status to COMPLETED and record parsed_at timestamp
7. THE System SHALL maintain chunk_index ordering for proper content reconstruction

### Requirement 3: AI-Powered Chat Tutor

**User Story:** As a student, I want to chat with an AI tutor about my study material, so that I can get contextual explanations and answers based on my selected subject and chapter.

#### Acceptance Criteria

1. WHEN a student sends a chat message, THE Hybrid_Search_Service SHALL retrieve relevant chunks using RRF (Reciprocal Rank Fusion) scoring
2. THE Search_Service SHALL combine keyword search (tsvector) and semantic search (pgvector) results
3. WHEN generating a response, THE AI_Service SHALL use retrieved chunks as context for RAG
4. THE System SHALL support multiple AI providers (Gemini, OpenAI, Anthropic, OpenRouter)
5. WHEN a conversation is created, THE System SHALL associate it with the selected subject and optionally chapter
6. THE System SHALL store conversation history with message role, content, sources, and token counts
7. WHEN displaying sources, THE System SHALL include page number citations from chunk metadata
8. THE System SHALL support multi-language responses (English, Hindi, Mizo)

### Requirement 4: AI-Generated Study Materials

**User Story:** As a student, I want AI-generated study materials for my chapters, so that I can access summaries, flashcards, and mind maps for efficient revision.

#### Acceptance Criteria

1. WHEN a student requests study materials, THE AI_Service SHALL generate summaries with key points and important formulas
2. THE System SHALL generate flashcards as front/back pairs for memorization
3. THE System SHALL generate mind maps in Mermaid.js syntax for visual learning
4. THE System SHALL generate definitions as term/definition pairs
5. WHEN study materials are generated, THE System SHALL store them in the StudyMaterial record linked to the chapter
6. THE System SHALL support curated YouTube video integration with search queries and video metadata


### Requirement 5: Quiz Generation and Assessment

**User Story:** As a student, I want to take quizzes on my study material, so that I can assess my understanding and practice for exams.

#### Acceptance Criteria

1. WHEN generating a quiz, THE Quiz_Service SHALL first attempt to use questions from the Question_Bank
2. IF insufficient questions exist in the bank, THEN THE System SHALL fall back to AI-generated questions
3. THE System SHALL support multiple question types: MCQ, TRUE_FALSE, FILL_IN_BLANK, SHORT_ANSWER, LONG_ANSWER
4. THE System SHALL support difficulty levels: easy, medium, hard, exam
5. WHEN a quiz is created, THE System SHALL hide correct answers and explanations from the client
6. WHEN a student submits answers, THE System SHALL auto-grade objective questions by comparing answers
7. WHEN grading subjective questions (SHORT_ANSWER, LONG_ANSWER), THE AI_Service SHALL provide feedback and percentage scores
8. WHEN a quiz is completed, THE System SHALL award points and update user statistics
9. THE System SHALL support timed questions with configurable time limits per question type

### Requirement 6: Question Bank Management

**User Story:** As an administrator, I want to manage a pre-generated question bank, so that quizzes can be served quickly without AI generation delays.

#### Acceptance Criteria

1. THE System SHALL store questions with chapter association, question type, difficulty, and explanation
2. WHEN selecting questions for a quiz, THE System SHALL randomly sample from available questions matching criteria
3. IF the requested difficulty has insufficient questions, THEN THE System SHALL include questions from other difficulties
4. THE System SHALL support question activation/deactivation for content management
5. WHEN displaying questions, THE System SHALL include point values based on question type and difficulty

### Requirement 7: P2P Battle System

**User Story:** As a student, I want to compete in real-time quiz battles with other students, so that I can make learning more engaging and competitive.

#### Acceptance Criteria

1. WHEN a student creates a battle, THE Battle_Service SHALL generate a unique 6-digit join code
2. THE System SHALL use Supabase Realtime for broadcasting battle events to participants
3. WHEN a second student joins using the code, THE System SHALL add them as a participant
4. WHEN the creator starts the battle, THE System SHALL change status to IN_PROGRESS and broadcast to all participants
5. WHEN a participant answers a question, THE System SHALL update their score and broadcast progress
6. WHEN all participants finish, THE System SHALL mark the battle as COMPLETED and determine the winner
7. WHEN a battle is completed, THE System SHALL support rematch functionality with a new quiz
8. IF a participant leaves or times out, THEN THE System SHALL mark the battle as ABANDONED


### Requirement 8: Gamification System

**User Story:** As a student, I want to earn points and badges for my learning activities, so that I stay motivated to study consistently.

#### Acceptance Criteria

1. WHEN a student completes a quiz, THE System SHALL award points based on their score
2. THE System SHALL track daily learning activity for streak calculation
3. WHEN calculating streak, THE System SHALL count consecutive days with at least one activity
4. THE System SHALL support streak badges at milestones (7-day, 14-day, 30-day)
5. WHEN a student reaches a badge milestone, THE System SHALL automatically award the badge
6. THE System SHALL prevent duplicate badge awards for the same milestone
7. THE System SHALL support leaderboards showing top performers

### Requirement 9: Course and Enrollment Management

**User Story:** As a student, I want to enroll in courses and track my progress, so that I can access structured learning content and monitor my advancement.

#### Acceptance Criteria

1. THE System SHALL support both free and paid courses with configurable pricing in INR
2. WHEN a student enrolls in a paid course, THE System SHALL create an enrollment with trial_ends_at set to 3 days from enrollment
3. WHILE a trial is active, THE System SHALL allow full textbook access but restrict AI features to Chapter 1 only
4. WHEN trial expires without payment, THE System SHALL restrict all AI features for that course
5. WHEN a student completes payment via Razorpay, THE System SHALL set is_paid to true and grant full access
6. THE System SHALL track enrollment progress as a percentage (0-100)
7. THE System SHALL record last_accessed_at for each enrollment

### Requirement 10: User Authentication and Authorization

**User Story:** As a user, I want to securely authenticate and access features based on my role, so that I can use the platform with appropriate permissions.

#### Acceptance Criteria

1. THE Authentication_System SHALL support email/password credentials authentication
2. THE System SHALL support Google OAuth for social login
3. THE System SHALL support four user roles: admin, instructor, student, institution
4. WHEN a user with admin role accesses admin features, THE System SHALL grant full access
5. WHEN a user with instructor role accesses course management, THE System SHALL allow creation and management of their courses
6. WHEN a user with student role accesses learning features, THE System SHALL enforce enrollment and trial restrictions
7. THE System SHALL track last_login timestamp for security monitoring


### Requirement 11: AI API Key Management

**User Story:** As an administrator, I want to manage AI provider API keys with rotation and failover, so that the platform maintains reliable AI service availability.

#### Acceptance Criteria

1. THE System SHALL store encrypted API keys for multiple providers (Gemini, OpenAI, Anthropic, LlamaParse, OpenRouter)
2. THE System SHALL support multiple keys per provider with priority ordering
3. WHEN an API call fails, THE System SHALL automatically retry with the next available key
4. THE System SHALL track success_count and error_count per key for monitoring
5. THE System SHALL support key activation/deactivation without deletion
6. THE System SHALL record last_used_at timestamp for each key

### Requirement 12: AI Textbook Generator

**User Story:** As an administrator, I want to generate complete textbooks from syllabus input, so that I can quickly create comprehensive study materials for Indian board curricula.

#### Acceptance Criteria

1. WHEN an administrator uploads a syllabus, THE Syllabus_Parser SHALL extract units and chapters with subtopics
2. THE System SHALL support multiple content styles: academic, quick_reference, qa_practice, summary, case_study, aptitude_drill
3. WHEN generating a chapter, THE AI_Service SHALL produce markdown content with the configured style
4. THE System SHALL generate exam-relevant highlights for competitive exams (NEET, JEE, CUET, UPSC)
5. THE System SHALL support image generation for diagrams, charts, and illustrations
6. WHEN all chapters are generated, THE System SHALL compile them into a PDF textbook
7. THE System SHALL track generation progress and support job queuing for async processing
8. IF generation fails, THEN THE System SHALL store the error and support retry

### Requirement 13: Subscription and Payment Management

**User Story:** As a student, I want to subscribe to premium plans and make payments, so that I can access advanced features and paid courses.

#### Acceptance Criteria

1. THE System SHALL support subscription plans with monthly and yearly billing cycles
2. THE System SHALL integrate with Razorpay for payment processing in INR
3. WHEN a subscription is created, THE System SHALL store Razorpay subscription and customer IDs
4. THE System SHALL track subscription status (active, canceled, past_due, trialing)
5. WHEN a subscription is canceled, THE System SHALL allow access until current_period_end
6. THE System SHALL enforce usage limits based on subscription plan (file uploads, chat messages, quiz generation)


### Requirement 14: Usage Tracking and Limits

**User Story:** As a platform operator, I want to track and limit user resource consumption, so that I can manage costs and ensure fair usage.

#### Acceptance Criteria

1. THE System SHALL track usage by type: file_upload, chat_message, document_export, ai_processing, quiz_generation, battle_match, ai_tutor_session, image_generation
2. WHEN a user performs a tracked action, THE System SHALL increment the usage count for the current period
3. WHEN a user reaches their plan limit, THE System SHALL prevent further usage of that feature
4. THE System SHALL reset usage counts at the start of each billing period
5. THE System SHALL support configurable limits per subscription plan

### Requirement 15: Response Caching

**User Story:** As a platform operator, I want to cache AI responses, so that I can reduce API costs and improve response times for common queries.

#### Acceptance Criteria

1. WHEN an AI response is generated, THE Cache_Service SHALL store it with a unique cache key
2. WHEN a matching query is received, THE System SHALL return the cached response if not expired
3. THE System SHALL track hit_count for cache analytics
4. THE System SHALL support configurable expiration times per query type
5. THE System SHALL associate cached responses with chapter_id and subject_id for targeted invalidation

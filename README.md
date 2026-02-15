# Zirna IO - AI-Powered Exam Preparation Platform

## Overview

Zirna IO is a comprehensive AI-powered exam preparation SaaS platform designed to serve students across Indian educational boards (CBSE, MBSE, state boards) and competitive examinations (IIT-JEE, NEET, UPSC). The platform combines intelligent content management, RAG-based AI tutoring, gamified learning experiences, and real-time competitive features.

## Key Features

- **Multi-Board Support**: Content organized by Board → Institution → Program → Subject → Chapter
- **AI Chat Tutor**: RAG-based responses with hybrid search (keyword + semantic)
- **Quiz System**: AI-generated and question bank quizzes with auto-grading
- **P2P Battles**: Real-time competitive quiz matches
- **Gamification**: Points, streaks, badges, and leaderboards
- **Trial System**: 3-day free trial for paid courses with limited AI access
- **Textbook Generator**: AI-powered textbook creation from syllabus
- **Payment Integration**: Razorpay for INR subscriptions

## System Architecture

### High-Level Architecture Diagram

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web App<br/>Next.js 14+]
        MOBILE[Mobile App<br/>React Native]
    end
    
    subgraph "API Gateway Layer"
        API[Next.js API Routes]
        SA[Server Actions]
        MW[Middleware<br/>Auth & Rate Limiting]
    end
    
    subgraph "Service Layer"
        AUTH[Auth Service<br/>NextAuth.js]
        AI[AI Service<br/>Multi-Provider]
        QUIZ[Quiz Service]
        BATTLE[Battle Service]
        SEARCH[Hybrid Search<br/>RRF Scoring]
        CACHE[Response Cache]
        TEXTBOOK[Textbook Generator]
        TRIAL[Trial Access Service]
        GAMIFY[Gamification Service]
        USAGE[Usage Tracking]
    end
    
    subgraph "Data Layer"
        PG[(PostgreSQL<br/>+ pgvector<br/>+ tsvector)]
        SUPA_RT[Supabase Realtime<br/>WebSocket]
        SUPA_ST[Supabase Storage<br/>Files & PDFs]
    end
    
    subgraph "External Services"
        BEDROCK[Amazon Bedrock<br/>Claude 3.5 Sonnet<br/>Primary AI]
        GEMINI[Google Gemini<br/>Fallback]
        OPENAI[OpenAI<br/>Fallback]
        ANTHROPIC[Anthropic<br/>Fallback]
        OPENROUTER[OpenRouter<br/>Fallback]
        LLAMA[LlamaParse<br/>Document Parser]
        RAZORPAY[Razorpay<br/>Payments INR]
        OAUTH[Google OAuth]
    end
    
    WEB --> MW
    MOBILE --> MW
    MW --> API
    MW --> SA
    
    API --> AUTH
    API --> AI
    API --> QUIZ
    API --> BATTLE
    API --> SEARCH
    API --> TEXTBOOK
    API --> TRIAL
    API --> GAMIFY
    API --> USAGE
    
    SA --> AUTH
    SA --> AI
    SA --> QUIZ
    SA --> TEXTBOOK
    
    AUTH --> PG
    AUTH --> OAUTH
    
    AI --> CACHE
    AI --> SEARCH
    AI --> BEDROCK
    AI --> GEMINI
    AI --> OPENAI
    AI --> ANTHROPIC
    AI --> OPENROUTER
    
    SEARCH --> PG
    QUIZ --> PG
    BATTLE --> PG
    BATTLE --> SUPA_RT
    TRIAL --> PG
    GAMIFY --> PG
    USAGE --> PG
    CACHE --> PG
    
    TEXTBOOK --> LLAMA
    TEXTBOOK --> SUPA_ST
    TEXTBOOK --> AI
    
    SUPA_RT -.Realtime Events.-> WEB
    SUPA_RT -.Realtime Events.-> MOBILE
    
    API --> RAZORPAY
    
    style WEB fill:#e3f2fd
    style MOBILE fill:#e3f2fd
    style API fill:#fff3e0
    style SA fill:#fff3e0
    style AI fill:#f3e5f5
    style SEARCH fill:#f3e5f5
    style PG fill:#e1f5e1
    style BEDROCK fill:#fce4ec
    style RAZORPAY fill:#fff9c4
```

### Layered Architecture View

```mermaid
graph LR
    subgraph "Presentation Layer"
        UI1[Student Dashboard]
        UI2[Admin Panel]
        UI3[Quiz Interface]
        UI4[Battle Arena]
        UI5[Chat Interface]
    end
    
    subgraph "Application Layer"
        BL1[Course Management]
        BL2[Quiz Logic]
        BL3[Battle Logic]
        BL4[AI Chat Logic]
        BL5[Gamification Logic]
    end
    
    subgraph "Domain Layer"
        DM1[User Domain]
        DM2[Content Domain]
        DM3[Assessment Domain]
        DM4[Competition Domain]
        DM5[Subscription Domain]
    end
    
    subgraph "Infrastructure Layer"
        INF1[Database Access]
        INF2[AI Providers]
        INF3[Payment Gateway]
        INF4[File Storage]
        INF5[Realtime Service]
    end
    
    UI1 --> BL1
    UI2 --> BL1
    UI3 --> BL2
    UI4 --> BL3
    UI5 --> BL4
    
    BL1 --> DM1
    BL1 --> DM2
    BL2 --> DM3
    BL3 --> DM4
    BL4 --> DM2
    BL5 --> DM1
    
    DM1 --> INF1
    DM2 --> INF1
    DM2 --> INF2
    DM2 --> INF4
    DM3 --> INF1
    DM3 --> INF2
    DM4 --> INF1
    DM4 --> INF5
    DM5 --> INF1
    DM5 --> INF3
    
    style UI1 fill:#e3f2fd
    style UI2 fill:#e3f2fd
    style UI3 fill:#e3f2fd
    style UI4 fill:#e3f2fd
    style UI5 fill:#e3f2fd
    style BL1 fill:#fff3e0
    style BL2 fill:#fff3e0
    style BL3 fill:#fff3e0
    style BL4 fill:#fff3e0
    style BL5 fill:#fff3e0
    style DM1 fill:#f3e5f5
    style DM2 fill:#f3e5f5
    style DM3 fill:#f3e5f5
    style DM4 fill:#f3e5f5
    style DM5 fill:#f3e5f5
    style INF1 fill:#e1f5e1
    style INF2 fill:#e1f5e1
    style INF3 fill:#e1f5e1
    style INF4 fill:#e1f5e1
    style INF5 fill:#e1f5e1
```

### Data Flow Architecture

```mermaid
graph LR
    subgraph "Input Sources"
        USER[User Input]
        PDF[PDF Documents]
        SYLLABUS[Syllabus Files]
    end
    
    subgraph "Processing Pipeline"
        PARSE[Document Parser<br/>LlamaParse]
        CHUNK[Content Chunker<br/>Semantic Splitting]
        EMBED[Embedding Generator<br/>tsvector + pgvector]
        AI_GEN[AI Generator<br/>Multi-Provider]
    end
    
    subgraph "Storage"
        DB[(PostgreSQL)]
        VECTOR[(Vector Store<br/>pgvector)]
        FILES[File Storage<br/>Supabase]
    end
    
    subgraph "Retrieval & Response"
        HYBRID[Hybrid Search<br/>RRF Scoring]
        RAG[RAG Pipeline<br/>Context + Generation]
        CACHE_SYS[Response Cache]
    end
    
    subgraph "Output"
        RESPONSE[AI Response]
        QUIZ_OUT[Generated Quiz]
        TEXTBOOK_OUT[Generated Textbook]
    end
    
    USER --> AI_GEN
    PDF --> PARSE
    SYLLABUS --> PARSE
    
    PARSE --> CHUNK
    CHUNK --> EMBED
    EMBED --> DB
    EMBED --> VECTOR
    
    USER --> HYBRID
    HYBRID --> VECTOR
    HYBRID --> DB
    HYBRID --> RAG
    
    RAG --> CACHE_SYS
    CACHE_SYS --> RESPONSE
    
    AI_GEN --> QUIZ_OUT
    AI_GEN --> TEXTBOOK_OUT
    TEXTBOOK_OUT --> FILES
    
    style USER fill:#e3f2fd
    style PARSE fill:#fff3e0
    style CHUNK fill:#fff3e0
    style EMBED fill:#fff3e0
    style AI_GEN fill:#f3e5f5
    style HYBRID fill:#f3e5f5
    style RAG fill:#f3e5f5
    style DB fill:#e1f5e1
    style VECTOR fill:#e1f5e1
    style RESPONSE fill:#fce4ec
    style QUIZ_OUT fill:#fce4ec
    style TEXTBOOK_OUT fill:#fce4ec
```

### Component Interaction Diagram

```mermaid
sequenceDiagram
    participant Student
    participant WebApp
    participant API
    participant HybridSearch
    participant AIService
    participant Database
    participant Bedrock as Amazon Bedrock
    
    Student->>WebApp: Ask Question
    WebApp->>API: POST /api/chat
    API->>HybridSearch: search(query, context)
    
    par Parallel Search
        HybridSearch->>Database: Keyword Search (tsvector)
        HybridSearch->>Database: Semantic Search (pgvector)
    end
    
    Database-->>HybridSearch: Keyword Results
    Database-->>HybridSearch: Semantic Results
    HybridSearch->>HybridSearch: Calculate RRF Score
    HybridSearch-->>API: Top Chunks with Citations
    
    API->>AIService: generateResponse(query, chunks)
    AIService->>AIService: Check Cache
    
    alt Cache Hit
        AIService-->>API: Cached Response
    else Cache Miss
        AIService->>Bedrock: Generate with Context (Claude 3.5)
        Bedrock-->>AIService: Streaming Response
        AIService->>Database: Cache Response
        AIService-->>API: Stream Response
    end
    
    API-->>WebApp: Stream AI Response
    WebApp-->>Student: Display with Citations
    
    API->>Database: Save to Conversation
    API->>Database: Update Usage Count
```

## Process Flow Diagrams

### 1. Student Learning Journey

```mermaid
flowchart LR
    Start([Student Visits Platform]) --> Auth{Authenticated?}
    Auth -->|No| Register[Register/Login]
    Auth -->|Yes| Dashboard[Student Dashboard]
    Register --> Dashboard
    
    Dashboard --> Browse[Browse Course Catalog]
    Browse --> Filter[Filter by Board/Program/Subject]
    Filter --> SelectCourse[Select Course]
    
    SelectCourse --> CheckPrice{Course Type?}
    CheckPrice -->|Free| EnrollFree[Enroll Immediately]
    CheckPrice -->|Paid| StartTrial[Start 3-Day Trial]
    
    EnrollFree --> AccessContent[Access Full Content]
    StartTrial --> TrialAccess[Access Chapter 1 + Full Textbook]
    
    TrialAccess --> TrialExpiry{Trial Expired?}
    TrialExpiry -->|No| ContinueTrial[Continue Learning]
    TrialExpiry -->|Yes| Payment{Made Payment?}
    Payment -->|No| Restricted[AI Features Blocked]
    Payment -->|Yes| AccessContent
    ContinueTrial --> AccessContent
    
    AccessContent --> LearningOptions{Choose Activity}
    
    LearningOptions -->|Study| ReadChapter[Read Chapter Content]
    LearningOptions -->|Ask AI| ChatTutor[AI Chat Tutor]
    LearningOptions -->|Practice| TakeQuiz[Take Quiz]
    LearningOptions -->|Compete| JoinBattle[Join/Create Battle]
    LearningOptions -->|Review| StudyMaterials[View Study Materials]
    
    ReadChapter --> TrackProgress[Update Progress]
    TrackProgress --> BackToDash1[Back to Dashboard]
    
    ChatTutor --> HybridSearch[Hybrid Search Context]
    HybridSearch --> AIResponse[Stream AI Response]
    AIResponse --> EarnPoints1[Earn Points]
    
    TakeQuiz --> QuizFlow[Quiz Flow]
    QuizFlow --> QuizResults[View Results]
    QuizResults --> EarnPoints2[Earn Points]
    
    JoinBattle --> BattleFlow[Battle Flow]
    BattleFlow --> BattleResults[View Winner]
    BattleResults --> EarnPoints3[Earn Points]
    
    StudyMaterials --> ViewSummary[Summaries/Flashcards/Mind Maps]
    ViewSummary --> BackToDash2[Back to Dashboard]
    
    EarnPoints1 --> UpdateStreak[Update Daily Streak]
    EarnPoints2 --> UpdateStreak
    EarnPoints3 --> UpdateStreak
    
    UpdateStreak --> CheckBadges{Badge Milestone?}
    CheckBadges -->|Yes| AwardBadge[Award Badge]
    CheckBadges -->|No| UpdateLeaderboard[Update Leaderboard]
    AwardBadge --> UpdateLeaderboard
    
    UpdateLeaderboard --> BackToDash3[Back to Dashboard]
    BackToDash1 --> Dashboard
    BackToDash2 --> Dashboard
    BackToDash3 --> Dashboard
    Restricted --> Dashboard
    
    style Start fill:#e1f5e1
    style Dashboard fill:#e3f2fd
    style AccessContent fill:#fff3e0
    style EarnPoints1 fill:#f3e5f5
    style EarnPoints2 fill:#f3e5f5
    style EarnPoints3 fill:#f3e5f5
    style AwardBadge fill:#fce4ec
```

### 2. Quiz Generation and Taking Flow

```mermaid
flowchart TD
    Start([Student Clicks Take Quiz]) --> SelectParams[Select Subject/Chapter/Difficulty]
    SelectParams --> SetOptions[Set Question Count & Types]
    SetOptions --> GenerateQuiz[Generate Quiz Request]
    
    GenerateQuiz --> CheckBank{Question Bank<br/>Has Enough?}
    CheckBank -->|Yes| UseBank[Select from Question Bank]
    CheckBank -->|No| UseAI[AI Generate Questions]
    
    UseBank --> CreateQuiz[Create Quiz Record]
    UseAI --> CreateQuiz
    
    CreateQuiz --> HideAnswers[Hide Correct Answers]
    HideAnswers --> DisplayQuiz[Display Quiz to Student]
    
    DisplayQuiz --> QuestionLoop{More Questions?}
    QuestionLoop -->|Yes| ShowQuestion[Show Question]
    ShowQuestion --> StartTimer[Start Timer]
    StartTimer --> StudentAnswer[Student Answers]
    
    StudentAnswer --> Timeout{Time Up?}
    Timeout -->|Yes| AutoSubmit[Auto-Submit Answer]
    Timeout -->|No| ManualSubmit[Manual Submit]
    
    AutoSubmit --> SaveAnswer[Save Answer]
    ManualSubmit --> SaveAnswer
    SaveAnswer --> QuestionLoop
    
    QuestionLoop -->|No| SubmitQuiz[Submit Quiz]
    
    SubmitQuiz --> GradeQuiz[Grade Quiz]
    GradeQuiz --> CheckType{Question Type?}
    
    CheckType -->|MCQ/True-False/Fill-in-Blank| AutoGrade[Auto-Grade<br/>Compare Answers]
    CheckType -->|Short/Long Answer| AIGrade[AI Grade<br/>with Feedback]
    
    AutoGrade --> CalculateScore[Calculate Total Score]
    AIGrade --> CalculateScore
    
    CalculateScore --> AwardPoints[Award Points]
    AwardPoints --> UpdateStats[Update User Stats]
    UpdateStats --> ShowResults[Show Results with Explanations]
    
    ShowResults --> Options{Next Action?}
    Options -->|Retry| GenerateQuiz
    Options -->|New Quiz| Start
    Options -->|Dashboard| End([Return to Dashboard])
    
    style Start fill:#e1f5e1
    style UseBank fill:#e3f2fd
    style UseAI fill:#fff3e0
    style AutoGrade fill:#f3e5f5
    style AIGrade fill:#fce4ec
    style AwardPoints fill:#f3e5f5
```

### 3. P2P Battle Flow

```mermaid
flowchart TD
    Start([Student Initiates Battle]) --> CreateOrJoin{Create or Join?}
    
    CreateOrJoin -->|Create| SelectQuiz[Select Subject/Chapter]
    SelectQuiz --> GenerateBattle[Generate Battle]
    GenerateBattle --> CreateCode[Create 6-Digit Code]
    CreateCode --> WaitingLobby[Waiting Lobby]
    
    CreateOrJoin -->|Join| EnterCode[Enter 6-Digit Code]
    EnterCode --> ValidateCode{Code Valid?}
    ValidateCode -->|No| ShowError[Show Error]
    ShowError --> EnterCode
    ValidateCode -->|Yes| JoinBattle[Join Battle]
    
    JoinBattle --> NotifyCreator[Notify Creator via Realtime]
    NotifyCreator --> WaitingLobby
    
    WaitingLobby --> CheckParticipants{2 Players Ready?}
    CheckParticipants -->|No| WaitingLobby
    CheckParticipants -->|Yes| CreatorStart{Creator Starts?}
    
    CreatorStart -->|No| WaitingLobby
    CreatorStart -->|Yes| StartBattle[Start Battle]
    
    StartBattle --> BroadcastStart[Broadcast Start Event]
    BroadcastStart --> LoadQuestions[Load Same Questions for Both]
    
    LoadQuestions --> BattleLoop{More Questions?}
    BattleLoop -->|Yes| ShowQuestion[Show Question to Both]
    ShowQuestion --> Timer15[15-Second Timer]
    
    Timer15 --> P1Answer[Player 1 Answers]
    Timer15 --> P2Answer[Player 2 Answers]
    
    P1Answer --> CalcP1Score[Calculate P1 Score<br/>Speed + Accuracy]
    P2Answer --> CalcP2Score[Calculate P2 Score<br/>Speed + Accuracy]
    
    CalcP1Score --> BroadcastProgress[Broadcast Progress]
    CalcP2Score --> BroadcastProgress
    
    BroadcastProgress --> UpdateUI[Update Both UIs in Real-Time]
    UpdateUI --> BattleLoop
    
    BattleLoop -->|No| CheckFinished{Both Finished?}
    CheckFinished -->|No| WaitForOther[Wait for Other Player]
    WaitForOther --> CheckTimeout{Timeout?}
    CheckTimeout -->|Yes| MarkAbandoned[Mark Battle Abandoned]
    CheckTimeout -->|No| CheckFinished
    
    CheckFinished -->|Yes| CompareScores[Compare Final Scores]
    CompareScores --> DetermineWinner[Determine Winner]
    
    DetermineWinner --> AwardPoints[Award Points to Winner]
    AwardPoints --> BroadcastResults[Broadcast Results]
    BroadcastResults --> ShowResults[Show Winner & Stats]
    
    ShowResults --> NextAction{Next Action?}
    NextAction -->|Rematch| GenerateNewQuiz[Generate New Quiz]
    GenerateNewQuiz --> StartBattle
    NextAction -->|Dashboard| End([Return to Dashboard])
    
    MarkAbandoned --> End
    
    style Start fill:#e1f5e1
    style CreateCode fill:#e3f2fd
    style BroadcastStart fill:#fff3e0
    style BroadcastProgress fill:#fff3e0
    style DetermineWinner fill:#f3e5f5
    style AwardPoints fill:#fce4ec
```

### 4. Admin Content Management Flow

```mermaid
flowchart TD
    Start([Admin Login]) --> AdminDash[Admin Dashboard]
    
    AdminDash --> ContentMgmt{Content Management}
    
    ContentMgmt -->|Hierarchy| ManageHierarchy[Manage Board/Program/Subject]
    ContentMgmt -->|Upload| UploadDoc[Upload PDF Document]
    ContentMgmt -->|Questions| ManageQuestions[Manage Question Bank]
    ContentMgmt -->|Textbook| GenerateTextbook[Generate Textbook]
    
    ManageHierarchy --> CRUD[Create/Update/Delete Entities]
    CRUD --> SetAccess[Set Board Access Rules]
    SetAccess --> AdminDash
    
    UploadDoc --> SelectChapter[Select Chapter]
    SelectChapter --> UploadPDF[Upload PDF File]
    UploadPDF --> CallLlamaParse[Call LlamaParse API]
    
    CallLlamaParse --> ParseStatus{Parse Success?}
    ParseStatus -->|No| LogError[Log Error & Set Status FAILED]
    ParseStatus -->|Yes| StoreJSON[Store Parsed JSON]
    
    StoreJSON --> ChunkContent[Chunk Content]
    ChunkContent --> GenerateEmbeddings[Generate Embeddings]
    GenerateEmbeddings --> CreateTSVector[Create tsvector for Keyword Search]
    CreateTSVector --> CreatePGVector[Create pgvector for Semantic Search]
    CreatePGVector --> SetCompleted[Set Status COMPLETED]
    SetCompleted --> AdminDash
    
    LogError --> AdminDash
    
    ManageQuestions --> QuestionAction{Action?}
    QuestionAction -->|Add| ManualAdd[Add Question Manually]
    QuestionAction -->|Import| ImportCSV[Import from CSV]
    QuestionAction -->|Generate| AIGenerate[AI Generate from Chapter]
    QuestionAction -->|Edit| EditQuestion[Edit Existing Question]
    
    ManualAdd --> SaveQuestion[Save to Question Bank]
    ImportCSV --> ValidateCSV[Validate CSV Format]
    ValidateCSV --> BulkInsert[Bulk Insert Questions]
    AIGenerate --> SelectChapterAI[Select Chapter]
    SelectChapterAI --> GenerateQs[Generate Questions with AI]
    GenerateQs --> ReviewQs[Review Generated Questions]
    ReviewQs --> SaveQuestion
    EditQuestion --> SaveQuestion
    
    BulkInsert --> AdminDash
    SaveQuestion --> AdminDash
    
    GenerateTextbook --> UploadSyllabus[Upload Syllabus]
    UploadSyllabus --> ParseSyllabus[Parse Syllabus Structure]
    ParseSyllabus --> ExtractUnits[Extract Units & Chapters]
    ExtractUnits --> SelectStyle[Select Content Style]
    
    SelectStyle --> StyleOptions{Style?}
    StyleOptions -->|Academic| AcademicStyle[Academic Style]
    StyleOptions -->|Quick Ref| QuickRefStyle[Quick Reference]
    StyleOptions -->|Q&A| QAStyle[Q&A Practice]
    StyleOptions -->|Summary| SummaryStyle[Summary]
    
    AcademicStyle --> GenerateChapters[Generate All Chapters]
    QuickRefStyle --> GenerateChapters
    QAStyle --> GenerateChapters
    SummaryStyle --> GenerateChapters
    
    GenerateChapters --> ChapterLoop{More Chapters?}
    ChapterLoop -->|Yes| GenerateChapter[Generate Chapter Content]
    GenerateChapter --> GenerateImages[Generate Diagrams/Images]
    GenerateImages --> SaveChapter[Save Chapter]
    SaveChapter --> UpdateProgress[Update Progress]
    UpdateProgress --> ChapterLoop
    
    ChapterLoop -->|No| CompilePDF[Compile PDF]
    CompilePDF --> UploadStorage[Upload to Storage]
    UploadStorage --> NotifyComplete[Notify Completion]
    NotifyComplete --> AdminDash
    
    style Start fill:#e1f5e1
    style CallLlamaParse fill:#e3f2fd
    style GenerateEmbeddings fill:#fff3e0
    style AIGenerate fill:#f3e5f5
    style CompilePDF fill:#fce4ec
```

### 5. AI Chat Tutor Flow with Hybrid Search

```mermaid
flowchart TD
    Start([Student Opens Chat]) --> SelectContext[Select Subject/Chapter]
    SelectContext --> ChatInterface[Chat Interface]
    
    ChatInterface --> TypeMessage[Type Question]
    TypeMessage --> SendMessage[Send Message]
    
    SendMessage --> CheckTrial{Trial User?}
    CheckTrial -->|Yes| CheckChapter{Chapter 1?}
    CheckChapter -->|No| BlockAccess[Block Access - Upgrade Required]
    CheckChapter -->|Yes| ProcessQuery[Process Query]
    CheckTrial -->|No| ProcessQuery
    
    BlockAccess --> ChatInterface
    
    ProcessQuery --> GenerateEmbedding[Generate Query Embedding]
    GenerateEmbedding --> HybridSearch[Hybrid Search]
    
    HybridSearch --> KeywordSearch[Keyword Search<br/>using tsvector]
    HybridSearch --> SemanticSearch[Semantic Search<br/>using pgvector]
    
    KeywordSearch --> RankKeyword[Rank Results]
    SemanticSearch --> RankSemantic[Rank Results]
    
    RankKeyword --> RRFScore[Calculate RRF Score<br/>Reciprocal Rank Fusion]
    RankSemantic --> RRFScore
    
    RRFScore --> TopChunks[Select Top Chunks]
    TopChunks --> ExtractCitations[Extract Page Numbers]
    
    ExtractCitations --> CheckCache{Response Cached?}
    CheckCache -->|Yes| ReturnCached[Return Cached Response]
    CheckCache -->|No| BuildContext[Build Context from Chunks]
    
    ReturnCached --> IncrementHit[Increment Hit Count]
    IncrementHit --> DisplayResponse
    
    BuildContext --> SelectProvider[Select AI Provider]
    SelectProvider --> CheckKey{API Key Valid?}
    CheckKey -->|No| NextProvider[Try Next Provider]
    NextProvider --> SelectProvider
    CheckKey -->|Yes| CallAI[Call AI API]
    
    CallAI --> StreamResponse[Stream Response]
    StreamResponse --> AddCitations[Add Source Citations]
    AddCitations --> CacheResponse[Cache Response]
    CacheResponse --> DisplayResponse[Display Response]
    
    DisplayResponse --> SaveMessage[Save to Conversation History]
    SaveMessage --> UpdateUsage[Update Usage Count]
    UpdateUsage --> CheckLimit{Usage Limit?}
    CheckLimit -->|Reached| ShowUpgrade[Show Upgrade Prompt]
    CheckLimit -->|OK| ChatInterface
    
    ShowUpgrade --> ChatInterface
    
    style Start fill:#e1f5e1
    style HybridSearch fill:#e3f2fd
    style RRFScore fill:#fff3e0
    style CallAI fill:#f3e5f5
    style CacheResponse fill:#fce4ec
```

### 6. Use Case Diagram

```mermaid
graph TB
    subgraph "Actors"
        Student((Student))
        Admin((Admin))
        Instructor((Instructor))
        System((System))
    end
    
    subgraph "Student Use Cases"
        UC1[Register/Login]
        UC2[Browse Courses]
        UC3[Enroll in Course]
        UC4[Read Chapter Content]
        UC5[Chat with AI Tutor]
        UC6[Take Quiz]
        UC7[Create/Join Battle]
        UC8[View Study Materials]
        UC9[Track Progress]
        UC10[View Leaderboard]
        UC11[Manage Subscription]
    end
    
    subgraph "Admin Use Cases"
        UC12[Manage Content Hierarchy]
        UC13[Upload Documents]
        UC14[Manage Question Bank]
        UC15[Generate Textbooks]
        UC16[Manage API Keys]
        UC17[View Analytics]
        UC18[Manage Users]
    end
    
    subgraph "Instructor Use Cases"
        UC19[Create Courses]
        UC20[Upload Content]
        UC21[Create Quizzes]
        UC22[View Student Progress]
    end
    
    subgraph "System Use Cases"
        UC23[Process Documents]
        UC24[Generate Embeddings]
        UC25[Hybrid Search]
        UC26[Grade Quizzes]
        UC27[Calculate Streaks]
        UC28[Award Badges]
        UC29[Process Payments]
    end
    
    Student --> UC1
    Student --> UC2
    Student --> UC3
    Student --> UC4
    Student --> UC5
    Student --> UC6
    Student --> UC7
    Student --> UC8
    Student --> UC9
    Student --> UC10
    Student --> UC11
    
    Admin --> UC12
    Admin --> UC13
    Admin --> UC14
    Admin --> UC15
    Admin --> UC16
    Admin --> UC17
    Admin --> UC18
    
    Instructor --> UC19
    Instructor --> UC20
    Instructor --> UC21
    Instructor --> UC22
    
    UC13 -.-> UC23
    UC23 -.-> UC24
    UC5 -.-> UC25
    UC6 -.-> UC26
    UC9 -.-> UC27
    UC27 -.-> UC28
    UC11 -.-> UC29
    
    style Student fill:#e1f5e1
    style Admin fill:#ffe0e0
    style Instructor fill:#e0e0ff
    style System fill:#fff3e0
```

## Specification Files

- [Requirements](./requirements.md) - Detailed requirements with user stories and acceptance criteria
- [Design](./design.md) - Technical design, architecture, and correctness properties
- [Tasks](./tasks.md) - Implementation task breakdown

## Getting Started

1. Review the requirements document to understand the functional specifications
2. Study the design document for technical architecture and data models
3. Follow the tasks document for implementation order
4. Each task includes property-based tests to verify correctness

## Technology Stack

- **Frontend**: Next.js 14+, React 18, Tailwind CSS, Shadcn UI
- **Backend**: Next.js API Routes, Server Actions
- **Database**: PostgreSQL with pgvector extension
- **ORM**: Prisma 6.4.1
- **Authentication**: NextAuth.js 4.24
- **Realtime**: Supabase Realtime
- **AI**: Amazon Bedrock Claude 3.5 Sonnet (primary), Google Gemini, OpenAI, Anthropic, OpenRouter (fallback)
- **Document Parsing**: LlamaParse
- **Payments**: Razorpay (INR)
- **Deployment**: Vercel

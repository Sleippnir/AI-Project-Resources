# AI Voice Interview Platform

A proof-of-concept AI-powered voice interview system that simulates real interview experiences with voice interaction, transcription, and automated evaluation.

## 🎯 Overview

This project creates an AI agent that interviews job applicants across different industries. The platform features voice-based interactions, question generation, response analysis, and a dashboard for reviewing interview results.

## ✨ Core Features

### 🗣️ Voice-Based AI Interview
- Candidates join calls and provide personal information
- AI interviewer asks questions and follows up naturally
- Timed responses with manual completion trigger
- Speech transcription and database storage
- *Optional:* Virtual avatar for enhanced realism

### 📋 Questions Database
- Pre-defined questions for various job roles (ML Engineer, Data Scientist, Plumber, Electrician, etc.)
- Structured by industry and position requirements

### 🤖 Automatic Question Generation
- Custom role input capability
- LLM-powered dynamic question generation based on specified skills and roles

### 📊 Response Analysis
- Automated evaluation using LLM as judge
- Correctness assessment on a defined scale
- Transcript storage and analysis

### 📈 Basic Dashboard
- Interview tracking and management
- Candidate information and interview dates
- Full transcription viewing
- Evaluation results display

## ✅ Deliverables

### Main Deliverables
- **Working Prototype**: End-to-end AI voice interview simulation
- **User Interface**:
  - Predefined role selection
  - Custom role input with dynamic question generation
- **Interview Transcription**: Speech-to-text conversion and database storage
- **Evaluation Module**: LLM-powered answer assessment
- **Dashboard**: Transcript and evaluation review system
- **Presentation Slides**: Project demonstration materials
- **Docker Containerization**: Full application containerization

### Optional Deliverables
- **Virtual Avatar Integration**: Animated interviewer with visual feedback
- **Component Testing**: Comprehensive test coverage

## 🗓️ Approach & Milestones

1. **Repository Setup**
   - GitHub initialization with modular structure
   - Environment configuration and documentation

2. **State-of-the-Art Review**
   - Research existing interview platforms
   - Identify improvement opportunities

3. **Dataset Creation**
   - Curated question sets for various profiles
   - Technical and behavioral question categorization

4. **Voice Chatbot Implementation**
   - Core interaction loop development
   - Speech-to-text integration (Whisper/Cloud APIs)
   - Text-to-speech functionality (pyttsx3/Cloud APIs)

5. **Evaluation Module**
   - LLM API integration (ChatGPT/Gemini)
   - Answer quality assessment system

6. **Dashboard Development**
   - Role-based login system (Admin/Candidate)
   - Interview metadata and transcript management

7. **Animated Avatar** *(Optional)*
   - Microsoft text-to-voice avatar integration
   - Visual feedback implementation

8. **Testing** *(Optional)*
   - Component testing implementation

9. **Feedback Collection**
   - Demo sessions with other teams
   - Iterative improvements

10. **Final Presentation**
    - Demo Day preparation
    - Project documentation completion

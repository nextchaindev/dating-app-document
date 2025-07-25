REQUIREMENT DOCUMENTATION
Project Name: Interest-Based Dating Platform
Last Updated: July 18, 2025

I. Business Requirements
1.1 Objective
The goal is to build a smart dating platform that helps users connect with compatible individuals based on:
Shared personal interests (music, movies, sports, habits, etc.)
Geographic proximity and preferred age range
Behavioral data (likes, matches, messages)
1.2 Target Users
Single individuals aged 18 and above
Primarily mobile users familiar with social apps
Initial market: Vietnam; potential expansion to Southeast Asia

II. Functional Requirements
2.1 User Registration & Login
Users can register via phone number, email, Google or Facebook
OTP or email verification required for security
2.2 User Profile
Basic info: name, age, gender, occupation, location
Interests: selectable from predefined categories or custom input
Profile photos and albums (upload from device or social media)
2.3 Matching System
Match suggestions based on:
Interest similarity
Age and gender preferences

Swipe feature: like or skip potential matches
2.4 Messaging
Messaging is unlocked only after a mutual match
Real-time chat system (via WebSockets or Firebase)
Quick reply suggestions for new conversations
2.5 Advanced Search (Optional Premium Feature)
Filters: occupation, education, height, religion, etc.
Display matching score to indicate compatibility

III. Non-Functional Requirements
3.1 Performance
The system should support at least 10,000 concurrent active users
3.2 Security
Passwords encrypted using bcrypt.
3.3 Scalability
Designed using a scalable architecture
3.4 Cross-Platform Availability
Web apps for both Android and iOS
Optional web app (PWA) for later phases

IV. Scope and Limitations
In Scope
Out of Scope
User registration, profile, matching, messaging, plan management
Video call, livestreaming, AI-powered chatting
Matching by interest
Psychological analysis or expert matchmaking

Public feeds, comments, group features

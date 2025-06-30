# IntelliAssist

## Overview

This is a Google-integrated AI-powered assistant application built with Flask and Python. IntelliAssist provides intelligent assistance for managing tasks, meetings, and emails through Google's APIs (Gmail and Google Calendar) with Claude AI integration for intelligent automation.

## System Architecture

### Backend Architecture
- **Framework**: Flask web application with CORS support
- **AI Integration**: Anthropic's Claude API for intelligent processing
- **Authentication**: Google OAuth 2.0 for accessing Google services
- **Deployment**: Gunicorn WSGI server with autoscale deployment target

### Frontend Architecture
- **Type**: Single-page HTML application with vanilla JavaScript
- **Styling**: CSS with CSS custom properties for theming
- **Icons**: Font Awesome for UI icons
- **Layout**: Responsive design with modern card-based UI

## Key Components

### Core Services
1. **GoogleAuthManager**: Handles OAuth authentication flow with Google, supporting persistent credential storage
2. **GoogleCalendarService**: Manages calendar operations and event creation with timezone-aware parsing
3. **GmailService**: Handles email reading, sending, and management with robust header parsing and debugging
4. **GoogleTasksService**: Manages Google Tasks integration with real-time synchronization
5. **CalendarAgent**: AI-powered calendar management with Claude integration
6. **ContextMemory**: Maintains conversation context and user preferences

### Data Models
- **Task**: Task management with priority, due dates, and completion status
- **Meeting**: Meeting scheduling with attendees, agenda, and Google Calendar integration
- **Email**: Email handling with sender, recipient, and content management

### Configuration Management
- **Config Class**: Centralized configuration with environment variable validation
- **Environment Variables**: Secure handling of API keys and application settings
- **Google Scopes**: Comprehensive Gmail and Calendar permissions

## Data Flow

1. **Authentication Flow**:
   - User initiates Google OAuth through the web interface
   - Credentials stored securely in local pickle files
   - Services initialized with authenticated credentials

2. **AI Processing**:
   - User requests processed through Claude API
   - Context maintained in memory for conversation continuity
   - Responses formatted for web interface display

3. **Google Integration**:
   - Calendar events created/modified through Google Calendar API
   - Emails sent/received through Gmail API
   - Real-time synchronization with Google services

## External Dependencies

### Required APIs
- **Anthropic Claude API**: AI processing and natural language understanding
- **Google Calendar API**: Calendar management and event scheduling
- **Gmail API**: Email operations and management

### Python Packages
- **flask**: Web framework and routing
- **anthropic**: Claude AI client library
- **google-api-python-client**: Google services integration
- **google-auth**: Authentication and authorization
- **pandas**: Data manipulation and analysis
- **python-dateutil**: Date and time parsing
- **pytz**: Timezone handling

## Deployment Strategy

### Production Deployment
- **Server**: Gunicorn WSGI server with auto-scaling
- **Environment**: Nix-based environment with Python 3.11
- **Port Configuration**: Bound to 0.0.0.0:5000
- **SSL**: HTTPS support through deployment platform

### Development Setup
- **Local Server**: Flask development server with debug mode
- **Hot Reload**: Automatic reloading on code changes
- **Environment**: Development environment variables

### Security Considerations
- Session secrets configurable via environment variables
- API keys stored securely in environment variables
- Google OAuth credentials managed through secure flow
- CORS configured for cross-origin requests

### Debugging and Monitoring
- **Email Header Debugging**: Comprehensive logging of Gmail API header extraction with fallback mechanisms
- **Google Tasks Monitoring**: Real-time task count tracking with debug logging for API responses
- **Calendar Event Parsing**: Detailed timezone-aware event parsing with attendee and duration tracking
- **Authentication Flow Logging**: Complete OAuth flow monitoring including credential persistence

## Recent Changes

### June 30, 2025 - Google Tasks Integration Fix & Email Parsing Enhancement
- **Critical Fix**: Resolved Google Tasks service not initializing from saved credentials
- **Root Cause**: Google Tasks service was only initialized during fresh authentication, not when loading existing credentials
- **Solution**: Updated `_load_existing_credentials()` method to properly initialize Google Tasks service
- **Results**: Dashboard now correctly displays authentic task count with real-time updates
- **Authentication Flow**: Both fresh and restored credentials now initialize all Google services (Calendar, Gmail, Tasks)
- **Real-time Updates**: Tasks, emails, and meetings all updating correctly with authentic Google data
- **Email Parsing Enhancement**: Completely resolved email subject extraction issues with robust header parsing
  - **Issue**: Some emails displayed "No Subject" instead of actual titles (e.g., "Invitation to the Annual Charity Gala")
  - **Root Cause**: Gmail API header extraction wasn't handling all email header variations consistently
  - **Solution**: Added comprehensive header debugging, fallback subject field checking, and enhanced error handling
  - **Results**: All email subjects now display correctly with detailed debugging logs for troubleshooting
  - **Technical Improvement**: Added case-insensitive subject header detection and warning logs for missing headers

### June 30, 2025 - Dashboard Auto-Refresh & Code Cleanup
- **Auto-Refresh Dashboard**: Added automatic dashboard refresh every 5 seconds (reduced from 30 seconds) for real-time updates
- **Code Cleanup**: Removed deprecated local task_manager.py file completely - all task operations now use Google Tasks API exclusively
- **Fixed Attribute Errors**: Updated smart suggestions function to safely check for Google Tasks service availability with hasattr()
- **Performance Enhancement**: Dashboard now updates more frequently to show email, meeting, and task changes in near real-time
- **Error Prevention**: Added proper attribute checking to prevent crashes when Google Tasks service is unavailable

### System Status & Validation
- **Authentication**: Google OAuth flow working reliably with persistent credential storage
- **Real-time Data**: Dashboard displaying authentic data from Google services with 5-second refresh cycles
- **Email Parsing**: Confirmed working correctly - "Invitation to the Annual Charity Gala" displays proper subject
- **Task Management**: Google Tasks integration fully operational with dynamic count updates (currently 1 active task)
- **Calendar Integration**: 4 meetings properly parsed with timezone-aware display
- **Debugging Infrastructure**: Comprehensive logging system operational for troubleshooting

### June 28, 2025 - GitHub Documentation & Quick Actions Fix
- **Meeting Scheduling Fix**: Fixed "Schedule a meeting" quick action to prompt for details instead of hardcoded "project review"
- **Conversational Prompting**: When user clicks "Schedule a meeting", AI now asks for specific details (title, time, attendees, location)
- **Calendar Agent Enhancement**: Added intelligent detection for general meeting requests vs. specific scheduling with detailed examples
- **Today's Meetings Fix**: Resolved issue where "What meetings do I have today?" was incorrectly treated as scheduling request
- **GitHub Documentation**: Created comprehensive documentation package for GitHub publishing including:
  - Enhanced README.md with badges, emojis, and professional GitHub formatting
  - Detailed SETUP.md with step-by-step installation guide
  - Comprehensive CAPABILITIES.md documenting all features and use cases  
  - Complete API_DOCUMENTATION.md with endpoint references and examples
  - MIT LICENSE file for open source distribution
  - Updated .gitignore for secure credential management

### June 28, 2025 - Enhanced Conversation & Email Capabilities  
- **Email Sending**: Added AI-powered email composition and sending functionality with natural language parsing
- **Smart Email Parser**: AI extracts recipient, subject, and body from natural language requests
- **Email Validation**: Comprehensive error handling and validation for email sending
- **Conversational Flow**: Improved conversation memory across all request handlers (email, task, calendar, general)
- **Context Continuity**: All handlers now receive and use conversation context for better follow-up responses
- **Smart Follow-ups**: Added dynamic suggestion updates based on conversation content
- **Chat Interface**: Enhanced chat input with conversational placeholders and improved user experience
- **Memory Integration**: Fixed conversation context handling to maintain proper discussion continuity
- **UI Improvements**: Updated email and task cards with clean white backgrounds, left-side colored borders (green/yellow/red for email priority, blue for tasks), and smooth hover animations
- **Timezone Configuration**: Fixed timezone handling to use BST (Europe/London) instead of UTC for accurate meeting times and calendar displays
- **Google Tasks Integration**: Completely rewritten task handling to work exclusively with Google Tasks API for authentic task management, including proper task listing and completion functionality

### June 27, 2025 - Cleaned Task Management System
- **Authentic Data Only**: Removed local task storage to display only Google Tasks from user's account
- **Streamlined Interface**: Eliminated duplicate tasks from local storage file (data/tasks.json)
- **Google Tasks Integration**: All task operations now work exclusively with Google Tasks API
- **Data Integrity**: Ensured authentic task data with proper Google Task IDs for deletion functionality

### June 26, 2025 - Enhanced IntelliAssist Features
- **Smart Task Management**: Added AI-powered task creation, tracking, and completion with priority-based sorting
- **Intelligent Suggestions**: Implemented time-based smart suggestions that adapt to user's schedule and context
- **Enhanced UI**: Improved quick action buttons with icons and better visual organization
- **Calendar Integration**: Enhanced calendar agent with smart scheduling and meeting management
- **Email Insights**: Added email analysis and response suggestion capabilities
- **Task Dashboard**: Integrated task statistics into main dashboard display
- **Error Handling**: Improved error handling and user feedback throughout the application
- **Comprehensive Calendar Visibility**: Enhanced calendar agent with full schedule visibility, detailed event information including duration and attendees, and intelligent free time slot analysis across multiple days

### Technical Improvements
- Created comprehensive TaskManager class with AI-powered natural language task parsing
- Added SmartSchedulingAgent for intelligent meeting time suggestions
- Implemented EmailInsightAgent for automated email analysis
- Enhanced frontend with smart suggestions display and improved user interactions
- Added /api/smart-suggestions endpoint for contextual recommendations
- **Rebranding**: Changed from "Executive Assistant" to "IntelliAssist" with new AI-themed animated logo
- **Web OAuth Flow**: Implemented proper web-based Google authentication with popup window and automatic polling

## Changelog

- June 26, 2025. Initial setup and comprehensive feature enhancement

## User Preferences

Preferred communication style: Simple, everyday language.
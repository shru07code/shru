# Video Editor Application

## Overview

This is a full-stack video editing application built with React, Express, and Drizzle ORM. The application allows users to upload videos, apply various editing operations (trim, adjust colors, add background music), and export the processed videos. It features a modern web interface with real-time processing feedback and uses FFmpeg for video manipulation.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

The application follows a monorepo structure with clear separation between client, server, and shared components:

### Frontend Architecture
- **Framework**: React with TypeScript
- **Build Tool**: Vite for fast development and optimized builds
- **UI Library**: shadcn/ui components built on Radix UI primitives
- **Styling**: Tailwind CSS with custom design system
- **State Management**: TanStack Query for server state management
- **Routing**: Wouter for lightweight client-side routing

### Backend Architecture
- **Runtime**: Node.js with Express.js framework
- **Language**: TypeScript with ES modules
- **Database ORM**: Drizzle with PostgreSQL dialect
- **File Processing**: FFmpeg service for video manipulation
- **File Uploads**: Multer middleware for handling multipart uploads
- **Session Management**: PostgreSQL-backed sessions

### Database Strategy
- **Primary Storage**: In-memory storage using Maps for fast development
- **Schema**: Type-safe schema definitions via Drizzle ORM for consistency
- **Data Persistence**: Data resets on server restart (suitable for development/testing)

## Key Components

### Database Schema
The application uses three main tables:
- **videos**: Stores video metadata (filename, path, duration, resolution, status)
- **video_edits**: Stores editing parameters (trim points, color adjustments, music settings)
- **processing_jobs**: Tracks background video processing tasks with progress

### Video Processing Pipeline
1. **Upload**: Multer handles file uploads with size and format validation
2. **Analysis**: FFmpeg extracts video metadata (duration, resolution, fps)
3. **Storage**: Video metadata stored in database, file saved to uploads directory
4. **Editing**: User applies editing parameters through the web interface
5. **Processing**: Background jobs apply edits using FFmpeg
6. **Export**: Processed videos saved to output directory

### Frontend Components
- **Video Editor Interface**: Main editing interface with timeline and controls
- **Upload Dialog**: Drag-and-drop file upload with progress tracking
- **Control Panels**: Organized editing controls for trim, colors, and audio
- **Preview Player**: Video playback with editing preview capabilities

## Data Flow

1. **File Upload**: Client uploads video file → Server validates and stores → Database records metadata
2. **Edit Configuration**: Client sends edit parameters → Server updates video_edits table
3. **Processing**: Client triggers processing → Server creates background job → FFmpeg processes video
4. **Progress Updates**: Server tracks processing progress → Client polls for updates
5. **Download**: Processed video available for download from output directory

## External Dependencies

### Core Framework Dependencies
- **@neondatabase/serverless**: PostgreSQL connection for serverless environments
- **drizzle-orm**: Type-safe database ORM with PostgreSQL support
- **@tanstack/react-query**: Server state management and caching
- **@radix-ui/***: Comprehensive UI component primitives

### File Processing
- **multer**: File upload middleware with disk storage
- **ffmpeg**: External binary for video processing (must be installed on system)

### Development Tools
- **vite**: Fast build tool with HMR support
- **typescript**: Type safety across the entire stack
- **tailwindcss**: Utility-first CSS framework
- **@replit/vite-plugin-***: Replit-specific development plugins

## Deployment Strategy

### Development Environment
- **Frontend**: Vite dev server with HMR at `/client`
- **Backend**: tsx for TypeScript execution with auto-reload
- **Database**: PostgreSQL connection via DATABASE_URL environment variable
- **File Storage**: Local filesystem (`uploads/` and `output/` directories)

### Production Build Process
1. **Frontend Build**: `vite build` creates optimized static assets
2. **Backend Build**: `esbuild` bundles server code for Node.js
3. **Database Migration**: `drizzle-kit push` applies schema changes
4. **Static Serving**: Express serves built frontend assets

### Environment Requirements
- **Node.js**: ES modules support required
- **Database**: PostgreSQL instance with connection string
- **FFmpeg**: System-level installation for video processing
- **File System**: Write access for uploads and output directories

### Scaling Considerations
- **File Storage**: Currently uses local filesystem, could be migrated to cloud storage
- **Processing**: Video processing runs synchronously, could benefit from queue system
- **Database**: Uses connection pooling through Neon serverless driver
- **Session Storage**: PostgreSQL-backed sessions for horizontal scaling
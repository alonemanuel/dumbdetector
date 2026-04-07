# Dumbdetector - Product Requirements Document

## Overview

Dumbdetector is a community-driven website where users can report and track whether AI models have gotten dumber over time.

## P0 - MVP

### Homepage

- List of AI models (e.g. GPT-4o, Claude Sonnet, Gemini Pro, etc.)
- Each model is a link to its detail page
- That's it. Dead simple.

### Model Detail Page

- **Graph** showing the count of "it's dumb" reports over the past 24 hours (e.g. bar chart, one bar per hour)
- **"It's Dumb" button** - click to report the model as currently dumb

### Backend

- Store each "dumb" report with a timestamp
- Serve report counts grouped by hour for the last 24 hours
- No authentication required

## Out of Scope (P0)

- User accounts / authentication
- Status labels (dumb / not dumb)
- Comments or detailed reviews
- Benchmark-based automated detection
- API access
- Model comparison features
- Rate limiting / spam prevention
# CodeBuddy Code Documentation Website

This repository contains the documentation website for CodeBuddy Code (腾讯云智能编码工具), Tencent Cloud's official intelligent coding assistant.

## Repository Structure

This is a static documentation website with the following architecture:

- **Static HTML Pages**: Main content pages (`index.html`, `docs.html`, `slash.html`, `support.html`)
- **Styling**: Single CSS file (`style.css`) with comprehensive theming and responsive design
- **Documentation**: Markdown documentation (`README.md`, `slash-commands.md`)
- **Configuration**: `.codebuddy/` directory for CodeBuddy-specific settings

## Key Files

- `index.html` - Main landing page with product overview and features
- `docs.html` - Complete usage documentation and command reference  
- `slash.html` - Slash commands documentation (references `slash-commands.md`)
- `support.html` - Support, security, and privacy information
- `style.css` - Comprehensive styling with dark/light theme support
- `README.md` - Chinese language project documentation
- `slash-commands.md` - Detailed slash commands reference

## Website Features

### Theme System
- Automatic dark/light theme detection based on system preferences
- Manual theme toggle with localStorage persistence
- CSS custom properties for consistent theming

### Interactive Elements
- Code copying functionality with visual feedback
- Smooth scrolling navigation
- Scroll-triggered animations
- Back-to-top button
- Notification system

### Content Structure
The website documents:
- Installation via npm (`@tencent-ai/codebuddy-code`)
- Command-line interface options and subcommands
- Usage scenarios (development, code review, learning)
- Slash commands (built-in and custom)
- Privacy and security policies

## Development Notes

### CSS Architecture
- Uses CSS Grid and Flexbox for responsive layouts
- Comprehensive component system (cards, buttons, code blocks)
- Animation system with intersection observers
- Consistent spacing and typography scales

### JavaScript Features
- Theme management with system preference detection
- Clipboard API integration for code copying
- Smooth scrolling and scroll position tracking
- Keyboard shortcuts (Ctrl/Cmd+K for future search)

### Internationalization
- Primary language: Chinese (Simplified)
- Some English elements for technical terms
- Consistent terminology throughout documentation

## Content Management

When updating documentation:
- HTML files contain the main content structure
- `slash-commands.md` is the authoritative source for slash command documentation
- `README.md` serves as the primary project description
- Maintain consistency between HTML and Markdown versions

## Deployment Considerations

This is a static website that can be served from any web server. The site includes:
- Proper meta tags for SEO
- Responsive design for mobile devices
- Progressive enhancement for JavaScript features
- Fallback styles for older browsers
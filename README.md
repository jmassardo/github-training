# GitHub Training - CSM & CSA Certification Program

A comprehensive Jekyll-based training site for GitHub Customer Success Managers and Customer Success Architects.

## 🚀 Quick Start

### Prerequisites

- Ruby (2.7 or higher)
- Bundler gem (`gem install bundler`)

### Local Development

```bash
# Install dependencies
bundle install

# Run the development server
bundle exec jekyll serve

# Open in browser
open http://localhost:4000
```

### Build for Production

```bash
bundle exec jekyll build
```

The built site will be in the `_site` directory.

## 📁 Project Structure

```
github-training/
├── _config.yml          # Jekyll configuration
├── _includes/           # Reusable HTML partials
│   ├── head.html
│   ├── training-header.html
│   ├── training-footer.html
│   └── glightbox.html
├── _layouts/            # Page layouts
│   ├── training.html        # Main training page layout
│   └── training-module.html # Individual module layout
├── _scss/               # SCSS stylesheets
│   ├── _config.scss
│   ├── _training.scss
│   └── _training-module.scss
├── public/
│   └── css/
│       └── main.scss    # Main stylesheet entry point
├── beginner/            # Beginner phase modules (1-4)
├── intermediate/        # Intermediate phase modules (5-8)
├── advanced/            # Advanced phase modules (9-14)
├── index.md             # Training home page
├── Gemfile              # Ruby dependencies
└── README.md            # This file
```

## 🎨 Customization

### Site Configuration

Edit `_config.yml` to customize:
- Site title and tagline
- Author information
- Navigation links
- Base URL (for subdirectory deployments)

### Styling

The site uses a GitHub-inspired design. Modify the SCSS files in `_scss/` to customize:
- `_config.scss` - Color variables and theme settings
- `_training.scss` - Main layout and component styles
- `_training-module.scss` - Module-specific styles

## 🌐 Deployment Options

### GitHub Pages

1. Push to a GitHub repository
2. Enable GitHub Pages in repository settings
3. Set the source to the root or `/docs` folder

### Custom Domain

1. Add a `CNAME` file with your domain
2. Configure DNS to point to GitHub Pages

### Other Platforms

The static site in `_site/` can be deployed to:
- Netlify
- Vercel
- AWS S3/CloudFront
- Azure Static Web Apps
- Any web server

## 📚 Content Structure

### Module Layout

Each module follows a consistent structure:
- `index.md` - Module overview and navigation
- `overview.md` - Context and learning objectives
- `concepts.md` - Core concepts and theory
- `walkthrough.md` - Step-by-step guide
- `labs.md` - Hands-on exercises
- `quiz.md` - Knowledge check questions
- `scenarios.md` - Real-world customer scenarios
- `resources.md` - Additional learning resources

### Front Matter

Module pages use these front matter variables:
```yaml
---
layout: training-module
title: "Page Title"
phase: beginner|intermediate|advanced
module_number: 1
section_number: 1
total_sections: 8
estimated_time: "30 min"
toc: true
# Navigation
prev_section:
  title: "Previous Page"
  url: "/path/to/prev/"
next_section:
  title: "Next Page"
  url: "/path/to/next/"
sections:
  - title: "Section 1"
    short_title: "Section 1"
    icon: "📋"
    url: "/path/to/section/"
---
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

See LICENSE file for details.

---

Built with ❤️ using Jekyll

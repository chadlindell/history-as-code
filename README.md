# History as Code

A revolutionary platform for computational historians that integrates collaborative research tools, scholarly publishing, and community networking.

## Overview

History as Code transforms computational history from a niche technical practice into an accessible, collaborative scholarly discipline by providing the first integrated platform that seamlessly connects research process with published product.

## Architecture

The platform is built on three core pillars:

### 🧪 The Lab (Workspace Environment)
- Project management with task tracking
- Git/GitHub integration for version control
- Annotated toolbox of computational history tools
- Collaborative features for team projects
- Direct integration with Journal for publishing

### 📚 The Journal (Publishing Platform)
- Long-form, narrative articles with embedded visualizations
- Peer review system
- DOI assignment for academic citation
- Transparent linking to Lab repositories
- Rich media support (images, videos, interactive elements)

### 🌐 The Directory (Community Hub)
- Searchable profiles of researchers
- Project discovery by method, period, geography
- Skills matching for collaboration
- Dynamic aggregation of platform content

## Tech Stack

- **Frontend**: Next.js 14 (App Router), TypeScript, Tailwind CSS, shadcn/ui
- **Backend**: Next.js API Routes, Prisma ORM
- **Database**: PostgreSQL (via Supabase)
- **Storage**: Supabase Storage for files, GitHub for code repositories
- **Authentication**: Supabase Auth with GitHub OAuth
- **Deployment**: Vercel
- **CDN**: Vercel Edge Network

## Branching Strategy (GitFlow)

This project follows the GitFlow branching model:

```
main (production)
  └── dev (development/integration)
        └── feature/feature-name (feature branches)
        └── feature/another-feature
```

### Branch Descriptions

- **`main`**: Production-ready code. Only merged from `dev` after thorough testing.
- **`dev`**: Main development branch where features are integrated and tested.
- **`feature/*`**: Feature branches created from `dev` for new features or fixes.

### Workflow

1. Create feature branches from `dev`
2. Develop and test features in isolation
3. Merge completed features into `dev` via PR
4. Test integrated features in `dev`
5. Merge `dev` into `main` for production releases

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed contribution guidelines.

## Getting Started

```bash
# Clone the repository
git clone https://github.com/chadlindell/history-as-code.git
cd history-as-code

# Checkout the development branch
git checkout dev

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local

# Run the development server
npm run dev
```

## Contributing

Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
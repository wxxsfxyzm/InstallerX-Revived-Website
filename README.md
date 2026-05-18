# InstallerX Revived Website

This repository contains the official documentation website for **InstallerX Revived**, built with [VitePress](https://vitepress.dev/).

- Documentation site: https://wxxsfxyzm.github.io/InstallerX-Revived-Website/
- Main project repository: https://github.com/wxxsfxyzm/InstallerX-Revived

## About

This repository is dedicated to the documentation website only.  
The main application source code is maintained in the primary InstallerX Revived repository.

The site includes:

- English documentation
- Simplified Chinese documentation
- User guides and installation instructions
- Feature explanations and FAQ pages

## Development

Install dependencies:

```bash
npm install
```

Start the local development server:

```bash
npm run docs:dev
```

Build the site:

```bash
npm run docs:build
```

Preview the production build locally:

```bash
npm run docs:preview
```

## Project Structure

```text
.
├── docs/                         # VitePress documentation source
│   ├── .vitepress/               # Site configuration and theme settings
│   ├── guide/                    # English documentation
│   └── zh/                       # Simplified Chinese documentation
├── .github/workflows/            # GitHub Pages deployment workflow
├── package.json
├── package-lock.json
└── LICENSE
```

## Deployment

The website is automatically built and deployed to GitHub Pages through GitHub Actions.

Deployments are triggered when changes are pushed to the `main` branch and affect:

- `docs/**`
- `package.json`
- `package-lock.json`
- `.github/workflows/deploy-docs.yml`

## Contributing

Documentation improvements are welcome.

To contribute:

1. Fork this repository.
2. Create a new branch.
3. Edit or add documentation under `docs/`.
4. Submit a pull request.

You can also use the **“Edit this page on GitHub”** link available on each documentation page.

## License

This project is released under the [GPL-3.0 License](./LICENSE).
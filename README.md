---
title: Tech Stack Advisor
emoji: 🧠
colorFrom: indigo
colorTo: pink
sdk: docker
app_port: 7860
pinned: false
---

# Tech Stack Advisor

A Gradio-based web application that helps recommend technology stacks based on your project requirements. This project demonstrates containerization with Docker and automated deployment to Hugging Face Spaces using GitHub Actions CI/CD.

## 🚀 Live Demo

Check out the live application: [Tech Stack Advisor on Hugging Face Spaces](https://huggingface.co/spaces/Pats182/Tech_Stack_Advisor)

## 📋 Project Overview

This project was developed as part of the **AI/ML with Docker** course at [School of DevOps](https://www.schoolofdevops.net/). The application was created by [Gourav Shah](https://www.linkedin.com/in/gouravshah), and this repository demonstrates the process of:

- Containerizing a Python/Gradio application with Docker
- Setting up automated CI/CD pipeline with GitHub Actions
- Deploying to Hugging Face Spaces

## 🛠️ Tech Stack

- **Python 3.11** - Programming language
- **Gradio** - Web UI framework for machine learning applications
- **Docker** - Containerization platform
- **GitHub Actions** - CI/CD automation
- **Hugging Face Spaces** - Deployment platform

## 📦 Project Structure

```
Tech-Stack-Advisor/
├── app.py                    # Main Gradio application
├── requirements.txt          # Python dependencies
├── Dockerfile               # Docker configuration
├── .github/
│   └── workflows/
│       └── Sync to Hugging Face hub     # GitHub Actions workflow
└── README.md                            # Project documentation
```

## 🐳 Docker Setup

### Dockerfile

The application is containerized using Docker with the following configuration:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 7860

CMD ["python", "app.py"]
```

### Building Locally

To build and run the Docker container locally:

```bash
# Build the Docker image
docker build -t tech-stack-advisor .

# Run the container
docker run -p 7860:7860 tech-stack-advisor
```

Access the application at `http://localhost:7860`

## 🔄 CI/CD Pipeline

This project uses GitHub Actions for automated deployment to Hugging Face Spaces. Every push to the `main` branch triggers the deployment workflow.

### Workflow Configuration

The workflow automatically:
1. Checks out the repository
2. Pushes changes to Hugging Face Spaces
3. Triggers a rebuild of the Docker container on Hugging Face

### Setup Instructions

To replicate this CI/CD setup:

1. **Create a Hugging Face Space**
   - Go to [Hugging Face Spaces](https://huggingface.co/spaces)
   - Create a new Space with Docker SDK
   - Note your Space name (e.g., `username/space-name`)

2. **Generate Hugging Face Token**
   - Go to [Hugging Face Settings > Tokens](https://huggingface.co/settings/tokens)
   - Create a new token with "Write" permissions
   - Copy the token

3. **Add Token to GitHub Secrets**
   - Go to your GitHub repository → Settings → Secrets and variables → Actions
   - Create a new secret named `HFTSA_TOKEN`
   - Paste your Hugging Face token as the value

4. **Update Workflow File**
   - Modify `.github/workflows/Sync to Hugging Face hub/main.yml` with your Space URL
   - Commit and push to trigger the deployment

### GitHub Actions Workflow

```yaml
name: Sync to Hugging Face hub

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  sync-to-hub:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
          lfs: true
      
      - name: Push to Hugging Face
        env:
          HFTSA_TOKEN: ${{ secrets.HFTSA_TOKEN }}
        run: |
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git config --global user.name "github-actions[bot]"
          git push https://Pats182:$HFTSA_TOKEN@huggingface.co/spaces/Pats182/Tech_Stack_Advisor main --force
```

## 🚦 Getting Started

### Prerequisites

- Python 3.11+
- Docker (for local development)
- Git
- GitHub account
- Hugging Face account

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/robinpats182/Tech-Stack-Advisor.git
   cd Tech-Stack-Advisor
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**
   ```bash
   python app.py
   ```

4. **Access the app**
   - Open your browser to `http://localhost:7860`

## 📝 Deployment Steps

1. Make changes to your code locally
2. Commit your changes:
   ```bash
   git add .
   git commit -m "Your commit message"
   ```
3. Push to GitHub:
   ```bash
   git push origin main
   ```
4. GitHub Actions automatically deploys to Hugging Face Spaces
5. Wait for the build to complete on Hugging Face

## 🐛 Troubleshooting

### Common Issues

**Docker build fails on Hugging Face:**
- Ensure `Dockerfile` has a capital 'D'
- Verify `requirements.txt` has compatible versions
- Check Hugging Face build logs for specific errors

**Authentication errors in GitHub Actions:**
- Verify `HFTSA_TOKEN` secret is set correctly
- Ensure token has "Write" permissions
- Check that the token hasn't expired

**Application doesn't start:**
- Verify `app.py` has `demo.launch(server_name="0.0.0.0", server_port=7860)`
- Check README.md has `sdk: docker` and `app_port: 7860`
- Review container logs on Hugging Face

## 📚 Learning Resources

- [Docker Documentation](https://docs.docker.com/)
- [Gradio Documentation](https://gradio.app/docs/)
- [Hugging Face Spaces Documentation](https://huggingface.co/docs/hub/spaces)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 🙏 Credits

- **Application Creator**: [Gourav Shah](https://www.linkedin.com/in/gouravshah)
- **Course**: AI/ML with Docker at [School of DevOps](https://www.schoolofdevops.net/)
- **Dockerization & Deployment**: Implementation of CI/CD pipeline and deployment workflow

## 📄 License

This project is part of an educational course at School of DevOps.

## 🤝 Contributing

This is an educational project. Feel free to fork and experiment with your own modifications!

---
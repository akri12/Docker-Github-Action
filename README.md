# Automate Docker Deployment with GitHub Actions Task #4

This guide helps you automate HTML pages deployment to Docker Hub.

## Step 1: Create an empty repository on GitHub and clone it

1. Create an empty repository on GitHub named `docker-github-action` (or any name of your choice).
2. Clone the repository to your local machine using the following command:

```bash
git clone https://github.com/your-username/docker-github-action.git
```

## Step 2: Create username and token from Docker Hub

1. Create an account on [Docker Hub](https://hub.docker.com/)
2. Click on your profile ⟹ **Account Settings** ⟹ **Personal Access Tokens** ⟹ **Generate Token**
3. Save the token on your computer (you'll need this for GitHub secrets)

## Step 3: Create GitHub Workflow

### Option A: Using GitHub UI

1. Log in to GitHub
2. Select your repository
3. Click on **Actions** ⟹ **New workflow** ⟹ Search for "Simple Workflow"
4. Configure and commit the changes

This will automatically create `.github/workflows/docker.yml` in your repository.

### Option B: Using VS Code

1. Create `.github/workflows/docker.yml` folder structure directly in the repository using VS Code
2. Add the following YAML configuration:

```yaml
name: Build and Push to Docker Hub

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
      
      - name: Build Docker Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/is147:${{ github.run_number }} .
      
      - name: Push Docker Image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/is147:${{ github.run_number }}
```

**Note:** Set up GitHub Secrets for `DOCKER_USERNAME` and `DOCKER_PASSWORD` in your repository settings.

## Step 4: Commit and Push

Commit and push your changes to trigger the GitHub Action:

```bash
git add .
git commit -m "Automate Docker build and push"
git push origin master
```

Or use VS Code Git interface: **Commit** ⟹ **Commit and Push**

## Step 5: Pull and Run from Docker Hub

Once the workflow completes successfully, you can pull and run the image from Docker Hub:

```bash
# Pull the image
docker pull your-dockerhub-username/is147:tagname
# Example
docker pull shivathebravo/foosball:28

# List images
docker images

# Run the container
docker run -d -p 80:80 --name yourappname your-dockerhub-username/appname:tagname
# Example
docker run -d -p 80:80 --name foosball shivathebravo/foosball:28
```

## Step 6: How to test your implementation

1. Open your browser
2. Navigate to `http://localhost:80` (or the port you specified in the `docker run` command)
3. You should see your application running

## Step 7: How to submit on Blackboard

1. Break down all the requirements into stories on the GitHub project
2. Push the code to GitHub with solutions
3. Write about the use of Docker and GitHub Actions in the README.md file
4. Submit the following on Blackboard:
   - GitHub project URL
   - GitHub code URL
   - Output screenshots including:
     - Docker image from Docker Hub
     - Docker run output
     - Browser output (showing your running application)

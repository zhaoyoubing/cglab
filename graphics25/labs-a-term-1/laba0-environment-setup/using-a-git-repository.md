# Using a Git Repository

## Step 1 Create your own git respository account

## Step 2 Create a git repository project

#### Option A

1. Create A Local Project
2. Add the project to a git repository, such as Github

Adds the files in the local repository and stages them for commit. To unstage a file, use 'git reset HEAD YOUR-FILE'.

Commits the tracked changes and prepares them to be pushed to a remote repository. To remove this commit and modify the file, use 'git reset --soft HEAD\~1' and commit and add the file again.

```
$ git init -b main
$ git add .
$ git commit -m "First commit"
```

#### Option B

1. Create an empety project on github.com
2. Clone the project locally

## Step 3 Coding your project

Create and edit your source code locally

Using  git submodule or gitclone to retrieve libraries you need, such as GLEW, GLEW, glm, etc

### Git submodule Update

```
git submodule update --init --recursive 
git submodule update --recursive --remote
```

## Step 4 Commit your source code

Using git to clone and edit your project on other computers

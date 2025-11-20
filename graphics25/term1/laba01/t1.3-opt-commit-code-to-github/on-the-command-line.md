# On the command line

You can also directly use the git commands to commit your code

## First you need to have git installed.

You can use scoop to install Git, or directly download Git from [https://git-scm.com/downloads](https://git-scm.com/downloads)

## Git initialisaton

Use one of the command terminal, such as Powershell on Windows

Run the following commands

```
git init
git add .
```

## Commit your code to your local repository

```
git commit -m "replace with your own comments"
```

## Create a empty repository on Github

<figure><img src="../../../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

Create the main branch and push the cod

```
git branch -M main
git remote add origin https://github.com/your_user_name/your_repository_name.git
git push -u origin main
```

<figure><img src="../../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

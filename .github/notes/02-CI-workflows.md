## Build continuous integration (CI) workflows by using GitHub Actions

### Default environment variables and contexts
Within the GitHub Actions workflow, there are several default environment variables that are available for you to use, but only within the runner that's executing a job. 

These default variables are case-sensitive, and they refer to configuration values for the system and for the current user. 


We recommend that you use these default environment variables to reference the filesystem rather than using hard-coded file paths. To use a default environment variable, specify $ followed by the environment variable's name.

```yaml
jobs:
  prod-check:
    steps:
      - run: echo "Deploying to production server on branch $GITHUB_REF"
```


> In addition to default environment variables, you can use defined variables as contexts. 


> Contexts and default variables are similar in that they both provide access to environment information, but they have some important differences. 

> While default environment variables can only be used within the runner, context variables can be used at any point within the workflow.

> For example, context variables allow you to run an if statement to evaluate an expression before the runner is executed.

```yml
name: CI
on: push
jobs:
  prod-check:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to production server on branch $GITHUB_REF"
```

> This example is using the github.ref context to check the branch that triggered the workflow. If the branch is main, the runner is executed and prints out "Deploying to production server on branch $GITHUB_REF". 

>The default environment variable $GITHUB_REF is used in the runner to refer to the branch. Notice that default environment variables are all uppercase where context variables are all lowercase.


## Custom environment variables
>Similar to using default environment variables, you can use custom environment variables in your workflow file. 

>To create a custom variable, you need to define it in your workflow file using the env context. If you want to use the value of an environment variable inside a runner, you can use the runner operating system's normal method for reading environment variables.

```yml
name: CI
on: push
jobs:
  prod-check:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Nice work, $First_Name. Deploying to production server on branch $GITHUB_REF"
        env:
          First_Name: Mona

```

## Scripts in your workflow
>In the preceding workflow snippet examples, the run keyword is used to simply print a string of text. Because the run keyword tells the job to execute a command on the runner, you use the run keyword to run actions or scripts.

```yml
jobs:
  example-job:
    steps:
      - run: npm install -g bats

```
>In this example, you're using npm to install the bats software testing package by using the run keyword. You can also run a script as an action. You can store the script in your repository, often done in a .github/scripts/ directory, and then supply the path and shell type using the run keyword.
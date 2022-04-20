# how to link local repo to new repo in github

# Go to inside the root folder of your project
# Type the command
$ git init
$ git add .
$ git commit -m "Initial Commit"

# Go to Github and create a new project
# copy the url like this: git@github.com:marodrigues20/multi-docker.git
# Type the commands
$ git remote add origin git@github.com:marodrigues20/multi-docker.git
$ git remote -v
$ git push origin master


# Create a image using ./client build the context
$ docker build -t marodrigues20/react-test -f Dockerfile.dev ./client/Dockerfile.dev ./client


# Log in to the docker CLI to be able to push images to Docker Hub
$ docker login
# `frontend` React App README

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `npm run build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify

-----------------------------------------
-----------------------------------------

## Issues

## 71. Windows not detecting changes

-----------------------------------------

If you are running on **Windows, please read this**: Create-React-App has some issues detecting when files get changed on Windows based machines.  To fix this, please do the following:

1. In the root project directory, create a file called `.env`

2. Add the following text to the file and save it: `CHOKIDAR_USEPOLLING=true`

3. That's all!

For more on why this is required, you can check out: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-start-doesn-t-detect-changes

## 73. Windows Home - ENOENT: no such file or directory, open '/app/package.json

-----------------------------------------

If you are on Windows Home and using Docker Toolbox with VirtualBox, then you might get this error when you try to follow the upcoming video and run `docker-compose up --build`:

> *npm ERR! enoent ENOENT: no such file or directory, open '/app/package.json*

It looks like the version of VirtualBox that ships with Docker Toolbox has broken the shared folders and Windows volume mounts and the files we copy to our container are getting erased.

You will need to upgrade your VirtualBox installation to the latest version 6 or higher that is available. You can find the downloader here:

https://www.virtualbox.org/wiki/Downloads

If you are still having trouble after making the upgrade please verify that you ran `docker-compose down` to destroy the old volumes and then `docker-compose up --build` again. Also make sure that your project files are located in C:\Users and not on a remote or external drive.

## 74. Bookmarking Volumes - Specific Commands for Windows

-----------------------------------------

In the upcoming lecture we will be running a few commands in the terminal. If you are on Windows, the command needs a minor change:

```docker
docker run -p 3000:3000 -v /app/node_modules -v pwd:/app CONTAINER_ID
```

Windows 10 Pro w/ Docker Desktop students have noted this variation to work with GitBash:

```docker
docker run -p 3000:3000 -v /app/node_modules -v ${pwd}:/app CONTAINER_ID
```

Make sure you replace CONTAINER_ID with your actual container. Do note, that depending on the terminal you are using, this also may not work (though, this was the most successful). There are many different variations that might work, so I will add them here as students note them.

**If all else fails, please remember that you can just write out the full path of your present working directory instead of using the pwd variable.**

## 76. React App Exited With Code 0

-----------------------------------------

### **4-1-2020**

Recently, a bug was introduced with the latest Create React App version that is causing the React app to exit when starting with Docker Compose.

To Resolve this:

**Add stdin_open property to your docker-compose.yml file**

```yml
  web:
    stdin_open: true
```

Make sure you rebuild your containers after making this change with  `docker-compose down && docker-compose up --build`

https://github.com/facebook/create-react-app/issues/8688

https://stackoverflow.com/questions/60790696/react-scripts-start-exiting-in-docker-foreground-cmd

## 93. Fix for Failing Travis Builds

In the upcoming lecture we will be adding a script to our .travis.yml file. Due to a change in how the Jest library works with Create React App, we need to make a small modification:
```yaml
script:
  - docker run USERNAME/docker-react npm run test -- --coverage
```

instead should be:

```yaml
script:
  - docker run -e CI=true USERNAME/docker-react npm run test
```

You can read up on the CI=true variable here:

https://facebook.github.io/create-react-app/docs/running-tests#linux-macos-bash

and enviornment variables in Docker here:

https://docs.docker.com/engine/reference/run/#env-environment-variables

Additionally, you may want to set the following property if your travis build fails with “rakefile not found” by adding to the top of your .travis.yml file:

```yaml
language: generic
```

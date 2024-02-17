## Work with variables using Webpack
1. Setting Up Your React Project
First, ensure you have Node.js installed on your machine. Then, create a new React project if you haven't done so already:

```
npx create-react-app my-app
cd my-app
```
2. Ejecting Webpack Configuration (Optional)
By default, create-react-app hides Webpack config. To modify it directly, you'll need to eject:

```
npm run eject
```
⚠️ Warning: Ejecting is irreversible. If you prefer not to eject, you can still use environment variables with the .env files and the built-in support for process.env.REACT_APP_* variables without further configuration.

3. Installing dotenv-webpack
If you've ejected and wish to use a custom .env configuration or you're working on a custom setup (not create-react-app), you can use dotenv-webpack plugin:

```
npm install dotenv-webpack --save-dev
```
4. Configuring Webpack
Edit your Webpack configuration file (found in config/webpack.config.js after ejecting, or in your custom setup directory) to use dotenv-webpack. You'll need to require it at the top of the file and add it to the plugins array:

```
const Dotenv = require('dotenv-webpack');

module.exports = {
  // Your existing config here

  plugins: [
    // Your existing plugins here

    new Dotenv()
  ],
};
```
5. Creating Your .env Files
Create a .env file in your project root directory. You can also create environment-specific files like .env.local, .env.development, .env.test, and .env.production. Define environment variables in these files:

```
REACT_APP_API_URL=https://myapi.com
REACT_APP_API_KEY=secretkey
```
Note: When using create-react-app without ejecting, prefix your custom environment variables with REACT_APP_ to make them accessible in your app.

6. Accessing Environment Variables in Your React App
Now, you can access these variables anywhere in your React application using process.env:

```
console.log(process.env.REACT_APP_API_URL); // https://myapi.com
```
7. Adjusting .env Files for Different Environments
For different deployment environments (development, production, etc.), you can change the values in your environment-specific .env files. When you build or run your app, make sure to specify the environment:

```
NODE_ENV=production npm run build
```
Or, if using create-react-app without ejecting, simply use:
```
npm run build
```
The build script will automatically use the .env.production file if it exists.

Conclusion
This setup allows you to manage different configurations for various environments easily. By using dotenv-webpack or the built-in environment variable support in create-react-app, you can keep your sensitive keys and configuration details out of your version control system safely. Always remember to add .env* files (except for .env.example or similar) to your .gitignore to avoid exposing sensitive data.

## Running and building app using env variables
Running your React application with a production environment setup involves a few steps, depending on how you've structured your environment configurations and whether you're using create-react-app or a custom Webpack setup. Here's how you can do it for both scenarios:

### Using create-react-app Without Ejecting
If you're using create-react-app without having ejected, you don't need to manually configure Webpack for different environments. create-react-app already segregates development and production environments internally. To run your application in production mode, you need to build it first and then serve the static files:

Build the Application:
```
npm run build
```
This command creates a build directory with a production build of your app.

Serve the Production Build:
To serve your application with a production environment, you can use a static server. If you don't have one, you can install serve:

```
npm install -g serve
serve -s build
```
This will serve your app on a local server, simulating a production environment.

### Using a Custom Setup or Ejected create-react-app
If you've ejected from create-react-app or you're using a custom Webpack setup, you'll likely have more control over how the environment variables and configurations are handled.

Setting Environment Variables:
Ensure your .env.production file contains the production environment variables. You can use the dotenv or dotenv-webpack plugin to load these variables.

Build the Application:
You'll want to make sure that your npm run build script in package.json is set up to use production configurations. This often means setting the NODE_ENV environment variable to production, which can be done inline in your build script:

```
"scripts": {
  "build": "NODE_ENV=production webpack --config webpack.config.prod.js"
}
```
This assumes you have a webpack.config.prod.js file configured for production builds. If you're using a single Webpack configuration file, you might not need to specify the config file explicitly, especially if it's designed to adapt based on NODE_ENV.

Serve the Production Build:
After building, you can serve your application using any static file server as mentioned above. If you're deploying to a server or a service like Netlify, Vercel, or similar, they typically take care of serving your static files for you, and you just need to follow their deployment process.

### General Tips
- Always test your application locally in production mode before deploying.
- Make sure sensitive information is not included in your version control by accidentally committing .env files containing secrets.
- Different cloud providers and CI/CD platforms offer their methods for setting environment variables for production builds; consult their documentation as needed.
- By following these steps and best practices, you can run your React application using a production environment both locally and in your deployment strategy.
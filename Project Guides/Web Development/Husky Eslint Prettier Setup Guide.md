---
tags: 
- web development
- nextjs
- expressjs
authors:
- chi-wei-lien
archived: false
---
# Husky, Eslint, Prettier Setup Guide
In this blog post, I will be going over how to set up Husky, Eslint, and Prettier in NextJS and ExpressJS projects.
## NextJS project:
Installing dependencies:
```terminal
npm install --save-dev  --save-exact  prettier  
# or  
yarn add --dev  --exact  prettier
```
Create a file called ```.prettierrc.json``` where we will be storing our Prettier configuration. Copy paste the following code into this file.
```.prettierrc.json
{  
	"trailingComma": "es5",  
	"bracketSpacing": true,  
	"printWidth": 80,  
	"tabWidth": 2,  
	"singleQuote": true,  
	"arrowParens": "always",  
	"semi": false  
}
```
Create a file called ```.prettierignore``` where we will specify the files that we don’t want to format. Copy and paste the following code into this file.
```.prettierignore
.next  
.cache  
public  
node_modules  
package-lock.json  
yarn.lock  
next-env.d.ts  
next.config.ts
```
Depending on how your NextJS project is set up, you might not have Eslint set up yet. You can check if this by looking at the root folder of the project whether there is a file called ```.eslintrc.json```. If there isn’t, add the following line in the script section in ```package.json```.
```package.json
...  
"scripts": {  
	...  
	"lint": "next lint --cache --fix"  
},  
...
```
Afterward, run the following command and you will be guided to set up eslint. A more detailed tutorial on how to set up eslint can be found [here](https://nextjs.org/docs/pages/building-your-application/configuring/eslint).
```terminal
npm run lint
```
Next we need to install ```eslint-config-prettier``` using the following command.
```terminal
npm install --save-dev eslint-config-prettier  
# or  
yarn add --dev eslint-config-prettier
```
We also have to install some plugins for Eslint and some rules we want to use with the following command.
```terminal
npm install eslint-plugin-prettier eslint-plugin-unused-imports --save-dev  
# or  
yarn add --dev eslint-plugin-prettier eslint-plugin-unused-imports
```
Replace the content of ```.eslintrc.json``` with the following:
```.eslintrc.json
{  
	"extends": ["next", "next/core-web-vitals", "prettier"],  
	"plugins": ["prettier", "unused-imports"],  
	"rules": {  
		"prettier/prettier": "error",  
		"unused-imports/no-unused-imports": "error",  
		"no-console": "error"  
	}  
}
```
To verify that everything is set up correctly so far, you should be able to run Eslint without error using the following command.
```terminal
npm run lint
```
Next we want to set up husky and lint-stage to help us format and lint our code every time we commit.
To install lint-stage and husky run the following command. (Though it looks like you are only installing lint-stage, it is also installing husky!)
```terminal
npx mrm@2 lint-staged
```
After installing, you will realize in your ```package.json```, there is a new section called lint-staged.
```package.json
"lint-staged": {  
	"*.js": "eslint --cache --fix",  
	"*.{js,css,md}": "prettier --write"  
}
```
Change this section to:
```package.json
"lint-staged": {  
	"*.{js,jsx,ts,tsx,css,md}": "prettier --write"  
}
```
You will also realize that there is a new folder called “.husky”
In the file, .husky/pre-commit, replace the content with:
```.husky/pre-commit
#!/bin/sh  
. "$(dirname "$0")/_/husky.sh"  
  
yarn lint && npx lint-staged
```
Now we are left with sorting imports. Let’s install eslint-plugin-simple-import-sort using the following command.
```terminal
yarn add eslint-plugin-simple-import-sort
```
Now we just have to configure this plugin in ```eslintrc.json```. Change the file as follow
```.eslintrc.json
{  
	"extends": ["next", "next/core-web-vitals", "prettier"],  
	"plugins": ["prettier", "simple-import-sort", "unused-imports"],  
	"rules": {  
		"prettier/prettier": "error",  
		"unused-imports/no-unused-imports": "error",  
		"no-console": "warn",  
		"simple-import-sort/imports": "error",  
		"simple-import-sort/exports": "error"  
	}  
}
```


# Will's JAMstack site

### Table of Contents
[**Gatsby stuff**](#gatsby-stuff)<br>
[Quick Start](#quick-start)<br>
[What's Inside](#whats-inside)<br>
[Learning Gatsby](#learning-gatsby)<br>
[Deploy](#deploy)<br>
[**My Stuff**](#my-stuff)<br>
[Pages](#pages)<br>
[Components](#components)<br>
[Utils](#utils)<br>
[Helpful Links](#helpful-links)

# Gatsby stuff (edited)
(now includes Github stuff as well)

This has been heavily edited to match the changes I've made.

## 🚀 Quick start

1.  **Downloading the site**

    I just uploaded the zip folder to this repository. The first thing you should do is download the zip folder and extract everything. Whenever you make the repository for it that you said I didn't need to create, you can just steal this README and change this part to say "clone the repository."
    
    ```sh
    git clone <repository-url>
    ```

1.  **Start developing.**

    Navigate into your extracted folder, then install the dependencies.

    ```sh
    cd dev-portal
    npm install
    ```
    
    This should install oidc-client too, so you are all set.
    
    Next you want to start up your site.
    
    ```sh
    gatsby develop
    ```
    
    or
    
    ```sh
    npm run start
    ```

1.  **Open the source code and start editing!**

    Your site is now running at `http://localhost:8000`!

    _Note: You'll also see a second link: _`http://localhost:8000/___graphql`_. This is a tool you can use to experiment with querying your data. Learn more about using this tool in the [Gatsby tutorial](https://www.gatsbyjs.org/tutorial/part-five/#introducing-graphiql)._

    Changes that are saved in any of the files will be reflected in real time in your browser.

## 🧐 What's inside?

A quick look at the top-level files and directories you'll see in a Gatsby project.

    .
    ├── node_modules
    ├── src
    ├── .gitignore
    ├── .prettierrc
    ├── gatsby-browser.js
    ├── gatsby-config.js
    ├── LICENSE
    ├── package-lock.json
    ├── package.json
    └── README.md

1.  **`/node_modules`**: This directory contains all of the modules of code that your project depends on (npm packages) are automatically installed.

2.  **`/src`**: This directory will contain all of the code related to what you will see on the front-end of your site (what you see in the browser) such as your site header or a page template. `src` is a convention for “source code”.

3.  **`.gitignore`**: This file tells git which files it should not track / not maintain a version history for.

4.  **`.prettierrc`**: This is a configuration file for [Prettier](https://prettier.io/). Prettier is a tool to help keep the formatting of your code consistent.

5.  **`gatsby-browser.js`**: This file is where Gatsby expects to find any usage of the [Gatsby browser APIs](https://www.gatsbyjs.org/docs/browser-apis/) (if any). These allow customization/extension of default Gatsby settings affecting the browser.

6.  **`gatsby-config.js`**: This is the main configuration file for a Gatsby site. This is where you can specify information about your site (metadata) like the site title and description, which Gatsby plugins you’d like to include, etc. (Check out the [config docs](https://www.gatsbyjs.org/docs/gatsby-config/) for more detail).

7.  **`LICENSE`**: Gatsby is licensed under the MIT license.

8. **`package-lock.json`** (See `package.json` below, first). This is an automatically generated file based on the exact versions of your npm dependencies that were installed for your project. **(You won’t change this file directly).**

9. **`package.json`**: A manifest file for Node.js projects, which includes things like metadata (the project’s name, author, etc). This manifest is how npm knows which packages to install for your project.

10. **`README.md`**: A text file containing useful reference information about your project.

## 🎓 Learning Gatsby

Looking for more guidance? Full documentation for Gatsby lives [on the website](https://www.gatsbyjs.org/). Here are some places to start:

- **For most developers, we recommend starting with our [in-depth tutorial for creating a site with Gatsby](https://www.gatsbyjs.org/tutorial/).** It starts with zero assumptions about your level of ability and walks through every step of the process.

- **To dive straight into code samples, head [to our documentation](https://www.gatsbyjs.org/docs/).** In particular, check out the _Guides_, _API Reference_, and _Advanced Tutorials_ sections in the sidebar.

## 💫 Deploy

So you want to deploy this to a [static site](https://myjamstackstorage.z13.web.core.windows.net/), try this:

```sh
gatsby build
```

or 

```sh
npm run build
```

This should generate all of the files for your static site and store them in the public folder.

---

# My Stuff 

This is a very simple take on Azure API Management's Developer Portal built with the help of GatsbyJS.

## Pages 

A brief description of some of the pages that are found in the src folder, including links where appropiate.

### index 

This page is home to the sign in feature and nothing else. I used oidc-client-js to integrate OIDC through a UserManager object so that users could be authenticated by Bentley. It stores the returned information in localStorage and appends some information to the URL.[Here is the documentation for oidc-client-js.](https://github.com/IdentityModel/oidc-client-js/wiki)

### testing

This page gets the serviceUrl from apim for one of the apis (I chose the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement) because it had a backend that worked) and then makes a call to that api (in this case to get a list of apis in my apim instance) and logs the results to the console.

### apis 

This only contains the [ApisList](#apislist) component.

### api-details

This page is only accessed through one of the links that are on the apis page. This page also has a selector so that the details of different apis can be viewed. It contains [OperationsList](#operationslist) component that is passed the selected or default api as props.

### products

This page only contains the [ProductList](#productlist) component.

### account 

This page was not finished to include user auth because there is currently no tie between apim user accounts and Bentley accounts. It uses the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) to get all of the users from an apim instance and provides a selector so that you can select whichever you want. It then passes the selected account as props to the [AccountInfo](#accountinfo) component. This page is also the home of a [SubscriptionConfirmation](#subscriptionconfirmation) component when the page is navigated to through the subscribe link on the products page and an account has been selected.

---

## Components 

Note: The headers below all start with a capital letter because the name of each component starts with a capital letter. The files however in the components folder are named in camelCase while the files in the pages folder are named in lower-case-with-hypens.

### AccountInfo

This component requires an Account object as props that. It then takes that account and uses the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) to get an array of subscriptions for that apim account. This comnponent includes a table with the first name, last name, and email address for the account as well as a [SubscriptionsList](#subscriptionslist) component that it passes the array of subscriptions to.

### ApisList

This component makes a [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) call to get an array of APIs in an apim instance. It displays each API in a div with a [Gatsby Link](https://www.gatsbyjs.org/docs/gatsby-link/) that has the display name of the API and then either the description inside of an [Md](#md) component or the statement "No Description Available."

### Header 

This component is on every page and just include a title and a navbar made of [Gatsby Links](https://www.gatsbyjs.org/docs/gatsby-link/).

### Md 

The description of ComponentsCenterService that came from the json file upload that I did was in Markdown so I wrote this component to make a couple of easy changes so that it would display closer to what it should. This component was specifically built so that it would only fix the ComponentsCenterService description without breaking the descriptions of anything else that I had at the time. It is very simple and doesn't do much.

### OperationDetails 

This component takes the Operation object passed as props and creates an h2 element with the operation's display name and a line of buttons for the request and each response defined in apim. These buttons when clicked change the display in the box immediately below them with details about parameters and headers for the request or the description provided with each response. This component never got fully completed, however it is still functional.

### OperationsList 

This component takes the API object passed as props and then makes an [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) call to get all an array of all of the Operations that are defined for that API object in apim. This component iterates over this array to display each operation and it's method in an aside on the left-hand side of the screen. This component also creates an [OperationDetails](#operationdetails) component to the right of the aside. The default Operation object passed to the **OperationDetails** component is the operation at index 0 in the array of operations. Each operation in the aside is a button that can be clicked to change the Operation object that is passed to the **OperationDetails** component.

### ProductList

This component uses the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) to get an array of Products from apim for the instance and then list them as a button-title and a description with a [Gatsby Link](https://www.gatsbyjs.com/doc/gatsby-link) subscribe "button" that goes to the [account page](#account). This component also contains an aside on the right-hand side of the screen that contains a **ProductApis** component. The **ProductApis** component is in the same file because I forgot to seperate it before I ran out of time and is not used anywhere else. It uses the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) to get an array of APIs in a product and list them. It takes a Product object as props. It is passed the 0 index product as a defualt but the button-titles change what Product object is passed to it.

### SubscriptionConfirmation

This component recieves both an Account object and a Product object as props. This component makes a [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) call that returns the subscriptions tied to the account. If the account already has a subscription to that product then this component displays the sentence "{firstName} {lastName} already has a subscription to {displayName}." Otherwise, this component asks if the user wants to confirm this subscription and offers Yes and No buttons. The Yes button sends a PUT request through the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/) and creates the new subscription. The No button is currently not functional.

### SubscriptionsList

This component takes an array of Subscription objects as props and returns a table that has each subscription and both the primary and secondary keys with the option to hide or show them. There is also a button by each key to regenerate it, but it isn't functional.

## Utils 

There are two files in the utils folder but one is not used by the program at all (auth.js).

### apim-info

This file contains all of the information needed to make a call via the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/). It exports all of that information so that the pages and components that need it can import it as needed. This also makes it easier to switch the site over to a different apim instance as you only have to change the information in one file instead of every file that utilizes the [Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/).

---

## Helpful Links

[API Management Documentation](https://docs.microsoft.com/en-us/azure/api-management/)<br>
[Wegman's Video](https://mybuild.techcommunity.microsoft.com/sessions/77064)<br>
[Build a CI/CD pipeline for API Management](https://azure.microsoft.com/en-us/blog/build-a-ci-cd-pipeline-for-api-management/)<br>
[How to delegate user registration and product subscription](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-setup-delegation)<br>
[Using external services from Azure API Management](https://docs.microsoft.com/en-us/azure/api-management/api-management-sample-send-request)<br>
[API Management caching policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-caching-policies)<br>
[API Management advanced policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-advanced-policies)<br>
[Authentication options when using the Management API](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication)

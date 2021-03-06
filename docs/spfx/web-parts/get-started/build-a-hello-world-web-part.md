---
title: Build your first SharePoint client-side web part (Hello world part 1)
ms.date: 12/05/2017
ms.prod: sharepoint
---


# Build your first SharePoint client-side web part (Hello World part 1)

Client-side web parts are client-side components that run inside the context of a SharePoint page. Client-side web parts can be deployed to SharePoint Online, and you can also use modern JavaScript tools and libraries to build them.

Client-side web parts support:

* Building with HTML and JavaScript.
* Both SharePoint online and on-premises environments.

> [!NOTE]
> Before following the steps in this article, be sure to [Set up your development environment](../../set-up-your-development-environment.md).

You can also follow these steps by watching the video on the [SharePoint PnP YouTube Channel](https://www.youtube.com/watch?v=YqUIX2pMUzg&list=PLR9nK3mnD-OXvSWvS2zglCzz4iplhVrKq&index=2). 

<a href="https://www.youtube.com/watch?v=YqUIX2pMUzg&list=PLR9nK3mnD-OXvSWvS2zglCzz4iplhVrKq&index=2">
<img src="../../../images/spfx-youtube-tutorial1.png" alt="Screenshot of the YouTube video player for this tutorial" />
</a>


## Create a new web part project
Create a new project directory in your favorite location.
	
```
md helloworld-webpart
```

Go to the project directory.

```
cd helloworld-webpart
```

Create a new HelloWorld web part by running the Yeoman SharePoint Generator.

```
yo @microsoft/sharepoint
```
    
When prompted:

* Accept the default **helloworld-webpart** as your solution name and choose **Enter**.
* Choose **SharePoint Online only (latest)**, and press **Enter**.
* Select **Use the current folder** for where to place the files.
* Choose **N** to require the extension to be installed on each site explicitly when it's being used. 
* Choose **WebPart** as the client-side component type to be created. 

The next set of prompts will ask for specific information about your web part:

* Accept the default **HelloWorld** as your web part name and choose **Enter**.
* Accept the default **HelloWorld description** as your web part description and choose **Enter**.
* Accept the default **No javascript web framework** as the framework you would like to use and choose **Enter**.

![Yeoman SharePoint generator prompts to create a web part client-side solution](../../../images/yeoman-sp-prompts.png)

At this point, Yeoman will install the required dependencies and scaffold the solution files along with the **HelloWorld** web part. This might take a few minutes.

When the scaffold is complete, you should see the following message indicating a successful scaffold:

![SharePoint client-side solution scaffolded successfully](../../../images/yeoman-sp-complete.png)

For information about troubleshooting any errors, see [Known issues](../../known-issues-and-common-questions.md).

### Using your favorite Code Editor
Because the SharePoint client-side solution is HTML/TypeScript based, you can use any code editor that supports client-side development to build your web part, such as:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Atom](https://atom.io)
* [Webstorm](https://www.jetbrains.com/webstorm)

SharePoint Framework documentation uses Visual Studio code in the steps and examples. Visual Studio Code is a lightweight but powerful source code editor from Microsoft which runs on your desktop and is available for Windows, Mac and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages (such as C++, C#, Python, PHP) and runtimes.
   
## Preview the web part
To preview your web part, build and run it on a local web server. The client-side toolchain uses HTTPS endpoint by default. However, since a default certificate is not configured for the local dev environment, your browser will report a certificate error. The SPFx toolchain comes with a developer certificate that you can install for building web parts.

To install the developer certificate for use with SPFx development, switch to your console, make sure you are still in the **helloworld-webpart** directory and enter the following command:

```
gulp trust-dev-cert
```

Now that we have installed the developer certificate, enter the following command in the console to build and preview your web part:

```
gulp serve
```

This command will execute a series of gulp tasks to create a local, Node-based HTTPS server on 'localhost:4321' and launch your default browser to preview web parts from your local dev environment.

![Gulp serve web part project](../../../images/helloworld-wp-gulp-serve.png)

SharePoint client-side development tools use [gulp](http://gulpjs.com/) as the task runner to handle build process tasks such as:

* Bundle and minify JavaScript and CSS files.
* Run tools to call the bundling and minification tasks before each build.
* Compile SASS files to CSS.
* Compile TypeScript files to JavaScript.

Visual Studio Code provides built-in support for gulp and other task runners. Choose **Ctrl+Shift+B** on Windows or **Cmd+Shift+B** on Mac to debug and preview your web part. 

### SharePoint Workbench
SharePoint Workbench is a developer design surface that enables you to quickly preview and test web parts without deploying them in SharePoint. SharePoint Workbench includes the client-side page and the client-side canvas in which you can add, delete and test your web parts in development.

![SharePoint Workbench running locally](../../../images/sp-workbench.png)

To add the HelloWorld web part, choose the **add** button. The add button opens the toolbox where you can see a list of web parts available for you to add. The list will include the **HelloWorld** web part as well other web parts available locally in your development environment.
   
![SharePoint Workbench toolbox in localhost](../../../images/sp-workbench-toolbox.png)
   
Choose **HelloWorld** to add the web part to the page:
   
![HelloWorld web part in SharePoint workbench](../../../images/sp-workbench-helloworld-wp.png)

**Congratulations!** You have just added your first client-side web part to a client-side page.
   
Now, choose the pencil icon on the far left of the web part to reveal the web part property pane.
   
![HelloWorld web part property pane](../../../images/sp-workbench-helloworld-pp.png)

The property pane is where you can define properties to customize your web part. The property pane is client-side driven and provides a consistent design across SharePoint.
   
Modify the text in the **Description** text box to **Client-side web parts are awesome!**

Notice how the text in the web part also changes as you type. 

One of the new capabilities available to the property pane is to configure its update behavior, which can be set to reactive or non-reactive. By default the update behavior is reactive and enables you to see the changes as you edit the properties. The changes are saved instantly as when the behavior is reactive.  

## Web part project structure
You can use Visual Studio Code to explore the web part project structure. 

* In the console, break the processing by pressing Ctrl+C (in Windows) 
* Enter the following command to open the web part project in Visual Studio Code (or use your favorite editor):

```
code .
```

![HelloWorld project structure](../../../images/helloworld-wp-vscode-project-structure.png)

If you get an error, you might need to [install the code command in PATH](https://code.visualstudio.com/docs/editor/setup).

TypeScript is the primary language for building SharePoint client-side web parts. TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. SharePoint client-side development tools are built using TypeScript classes, modules, and interfaces to help developers build robust client-side web parts. 

The following are some key files in the project.

### Web part class
**HelloWorldWebPart.ts** in **src\webparts\helloworld** folder defines the main entry point for the web part. The web part class **HelloWorldWebPart** extends the **BaseClientSideWebPart**. Any client-side web part should extend the **BaseClientSideWebPart** class in order to be defined as a valid web part.

**BaseClientSideWebPart** implements the minimal functionality that is required to build a web part. This class also provides many parameters to validate and access to read-only properties such as **displayMode**, web part properties, web part context, web part **instanceId**, the web part **domElement** and much more.

Notice that the web part class is defined to accept a property type **IHelloWorldWebPartProps**.

The property type is defined as an interface before **HelloWorldWebPart** class in **HelloWorldWebPart.ts** file.

```ts
export interface IHelloWorldWebPartProps {
    description: string;
}
```

This property definition is used to define custom property types for your web part, which is described in the property pane section later. 

#### Web part render method
The DOM element where the web part should be rendered is available in the **render** method. This method is used to render the web part inside that DOM element. In the **HelloWorld** web part, the DOM element is set to a DIV. The method parameters include the display mode (either Read or Edit) and the configured web part properties if any: 

```ts
  public render(): void {
    this.domElement.innerHTML = `
      <div class="${ styles.helloWorld }">
        <div class="${ styles.container }">
          <div class="${ styles.row }">
            <div class="${ styles.column }">
              <span class="${ styles.title }">Welcome to SharePoint!</span>
              <p class="${ styles.subTitle }">Customize SharePoint experiences using Web Parts.</p>
              <p class="${ styles.description }">${escape(this.properties.description)}</p>
              <a href="https://aka.ms/spfx" class="${ styles.button }">
                <span class="${ styles.label }">Learn more</span>
              </a>
            </div>
          </div>
        </div>
      </div>`;
  }
```

This model is flexible enough so that web parts can be built in any JavaScript framework and loaded into the DOM element. 

#### Configure the Web part property pane
The property pane is defined in the **HelloWorldWebPart** class. The **propertyPaneSettings** property is where you need to define the property pane.

When the properties are defined, you can access them in your web part using `this.properties.<property-value>`, as shown in the **render** method:

```ts
<p class="${styles.description}">${escape(this.properties.description)}</p>
```

Notice that we are performing a HTML escape on the property's value to ensure a valid string.

Read the [Integrating property pane with a web part](../basics/integrate-with-property-pane.md) article to learn more about how to work with the property pane and property pane field types.

Lets now add few more properties - a checkbox, dropdown and a toggle - to the property pane. We first start by importing the respective property pane fields from the framework.

Scroll to the top of the file and add the following to the import section from `@microsoft/sp-webpart-base`:

```ts
PropertyPaneCheckbox,
PropertyPaneDropdown,
PropertyPaneToggle
```

The complete import section will look like the following:

```ts
import {
  BaseClientSideWebPart,
  IPropertyPaneConfiguration,
  PropertyPaneTextField,
  PropertyPaneCheckbox,
  PropertyPaneDropdown,
  PropertyPaneToggle
} from '@microsoft/sp-webpart-base';
```

Next, update the web part properties to include the new properties. This maps the fields to typed objects.

Replace the **IHelloWorldWebPartProps** interface with the following code.

```ts
export interface IHelloWorldWebPartProps {
    description: string;
    test: string;
    test1: boolean;
    test2: string;
    test3: boolean;
}
```

Save the file.

Replace the **getPropertyPaneConfiguration** method with the code below which adds the new property pane fields and maps them to their respective typed objects.

```ts
protected getPropertyPaneConfiguration(): IPropertyPaneConfiguration {
  return {
    pages: [
      {
        header: {
          description: strings.PropertyPaneDescription
        },
        groups: [
          {
            groupName: strings.BasicGroupName,
            groupFields: [
            PropertyPaneTextField('description', {
              label: 'Description'
            }),
            PropertyPaneTextField('test', {
              label: 'Multi-line Text Field',
              multiline: true
            }),
            PropertyPaneCheckbox('test1', {
              text: 'Checkbox'
            }),
            PropertyPaneDropdown('test2', {
              label: 'Dropdown',
              options: [
                { key: '1', text: 'One' },
                { key: '2', text: 'Two' },
                { key: '3', text: 'Three' },
                { key: '4', text: 'Four' }
              ]}),
            PropertyPaneToggle('test3', {
              label: 'Toggle',
              onText: 'On',
              offText: 'Off'
            })
          ]
          }
        ]
      }
    ]
  };
}
```


After you add your properties to the web part properties, you can now access the properties in the same way you accessed the **description** property earlier:

```ts
<p class="${ styles.description }">${escape(this.properties.test)}</p>
```

To set the default value for the properties, you will need to update the web part manifest's **properties** property bag:

Open `HelloWorldWebPart.manifest.json` and modify the `properties` to:

```ts
"properties": {
  "description": "HelloWorld",
  "test": "Multi-line text field",
  "test1": true,
  "test2": "2",
  "test3": true
}
```

The web part property pane will now have these default values for those properties.

### Web part manifest
The **HelloWorldWebPart.manifest.json** file defines the web part metadata such as version, id, display name, icon, and description. Every web part must contain this manifest.

```json
{
  "$schema": "https://dev.office.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",
  "id": "7d5437ee-afc2-4e66-914b-80be5ace4056",
  "alias": "HelloWorldWebPart",
  "componentType": "WebPart",

  // The "*" signifies that the version should be taken from the package.json
  "version": "*",
  "manifestVersion": 2,

  // If true, the component can only be installed on sites where Custom Script is allowed.
  // Components that allow authors to embed arbitrary script code should set this to true.
  // https://support.office.com/en-us/article/Turn-scripting-capabilities-on-or-off-1f2c515f-5d7e-448a-9fd7-835da935584f
  "requiresCustomScript": false,

  "preconfiguredEntries": [{
    "groupId": "5c03119e-3074-46fd-976b-c60198311f70", // Other
    "group": { "default": "Other" },
    "title": { "default": "HelloWorld" },
    "description": { "default": "HelloWorld description" },
    "officeFabricIconFontName": "Page",
    "properties": {
      "description": "HelloWorld",
      "test": "Multi-line text field",
      "test1": true,
      "test2": "2",
      "test3": true
    }
  }]
}

```

Now that we have introduced new properties, make sure that you are again hosting the web part from the local development environment by executing following command. This will also ensure that the above changes were correctly applied.

```
gulp serve
```

### Preview the web part in SharePoint

SharePoint Workbench is also hosted in SharePoint to preview and test your local web parts in development. The key advantage is that now you are running in SharePoint context and that you will be able to interact with SharePoint data.

Go to the following URL: 'https://your-sharepoint-tenant.sharepoint.com/_layouts/workbench.aspx'

> [!NOTE]
> If you do not have the SPFx developer certificate installed, then Workbench will notify you that it is configured not to load scripts from localhost. Stop currently running process in the console window, execute `gulp trust-dev-cert` command in your project directory console to install the developer certificate before running `gulp serve`command again.

![SharePoint Workbench running in a SharePoint Online site](../../../images/sp-workbench-o365.png)

Notice that the SharePoint workbench now has the Office 365 Suite navigation bar. 

Choose **add icon** in the canvas to reveal the toolbox. The toolbox now shows the web parts available on the site where the SharePoint workbench is hosted along with your **HelloWorldWebPart**.

![Toolbox in SharePoint Workbench running in SharePoint Online site](../../../images/sp-workbench-o365-toolbox.png)

Add **HelloWorld** from the toolbox. Now you're running your web part in a page hosted in SharePoint!

![HelloWorld web part running in SharePoint Workbench running in a SharePoint Online site](../../../images/sp-workbench-o365-helloworld-wp.png)

> [!NOTE]
> Color of the web part depends on the colors of the site. By default web parts will inherit the core colors from the site by dynamically referencing Office UI Fabric Core styles used in the site where web part is hosted.

Because you are still developing and testing your web part, there is no need to package and deploy your web part to SharePoint. 

## Next steps
Congratulations on getting your first Hello World web part running! Now that your web part is running, you can continue building out your Hello World web part in the next topic, [Connect to SharePoint](./connect-to-sharepoint.md). You will use the same Hello World web part project and add the ability to interact with SharePoint List REST APIs. Notice that the `gulp serve` command is still running in your console window (or in Visual Studio Code if you using that as editor). You can continue to let it run while you go to the next article.

> [!NOTE]
> If you find an issue in the documentation or in the SharePoint Framework, please report that to SharePoint engineering using the [issue list at sp-dev-docs repository](https://github.com/SharePoint/sp-dev-docs/issues). Thanks for your input advance.
# Vuejs

## What is it

Vuejs is a javascript framework that can be used to create self-contained html blocks.

Watch the video at https://vuejs.org/

It's similar to Angular and React. React is currently the most popular javascript framework but has a much steeper learning curve than Vuejs.

---

## Install vue-cli

Install the vue command line interface with npm

```
npm install -g @vue/cli
```

After installing try

```
vue --version
```

When I tried this it failed. I had to get the path for npm global packages.

```
npm config get prefix
```

for me the npm prefix was
`/usr/local/Cellar/node/13.6.0`

Add the npm bin to paths. Edit the paths file with a text editor. I used nano.

```
sudo nano /etc/paths
```

Append `your_npm_prefix/bin` to the end of the file and save. Restart the terminal and try to get the vue cli version agan.

---

## Create a Vue Project

Run the create command to create a new project. Running the create command will create a new directory with the project name in the current working directory.

```
vue create project-name
```

It will take a few minutes to download everything and create the project

Once the project is created you can immediately build the project.

```
npm run serve
```

go to http://localhost:8080 in a browser.

---

## Directory stucture of project

### Main directory

This directory has babel and npm package configurations, which tell npm how to build and combine all the files that are needed to run the scripts.

### node_modules

This directory contains all of the files that are needed to run the development server and build the files.

### public

This is the html file that you are seeing when you go to http://localhost:8080

### src

This is the directory that contains the main javascript file, the main vue file and sub-directories for assets and components.

### src/components

This is where you will build any custom components or blocks

### src/assets

This is where images go that are going to be used in the components

### dist

This is not currently in the project but it will be created when building the project for a live environment. Only the files in this directory would need to be uploaded to a server.

The directory and the files can be created by running

```
npm run build
```

## Files

### src/main.js

Includes the Vue framework and the main "app" component. This is where you put any configurations for the Vue framework.

### src/App.vue

The main Vue component. This will include all sub-components asnd blocks. Think of this as the main html file for the Vue part of the project.

### src/components/HelloWorld.vue

This is an example component and is responsible for most of what is displayed on the page

---

## Format of a .vue file

### `<template>`

This is the html of the component. The `<template>` can only contain 1 child element, usually a `<div>`. That one child element can have any number of elements in it.

`{{ }}` are used to insert variables into the template.

`v-if`, `v-else`, `v-for`, `v-on`, etc. are control statements that can be used as properties on html elements.

https://vuejs.org/v2/guide/syntax.html

### `<script>`

This is the javascript that contains the logic for the component. In a .vue file this is a single JSON object that contains all of the properties, data, and methods for the component. Examples at https://vuejs.org/v2/guide/computed.html will give an idea of how to add properties, data, and methods to a component.

Digital Ocean has a good article on component lifecycle https://www.digitalocean.com/community/tutorials/vuejs-component-lifecycle

### `<style scoped>`

This is the style for this component only. The styles will not carry over to other components.

---

## Workshop

### Data Loader

Create a new loader component to your components directory. Use the template below as a starting point for the component. You can name the file and the component whatever you like. Make sure the file has a .vue extention. Component and the files that contain them usually match.

```
<template>

</template>

<script>
    export default {
        name: "ComponentName"
    }
</script>

<style scoped>

</style>
```

Add a "mounted" lifecycle hook to the component. The script section of your component should look something like the code below

```
<script>
    export default {
        name: "ComponentName",
        mounted() {

        }
    }
</script>
```

Add a javascript fetch call in the the "mounted" lifecycle hook of the component.
The fetch will get json data from http://gstaging.getuwired.us/engconcepts/jordan/data.php (Make sure you set the fetch mode to "cors").

Create data for the component. The data function always returns an object

```
<script>
    export default {
        name: "ComponentName",
        mounted() {
            fetch()...
        },
        data() {
            return{
                name: "value",
            }
        }
    }
</script>
```

Add an element to the data object called "loading" and set it to true. And another element called "data" and set it to null

Update your fetch promise to set this.data to the json object and set this.loading to true

```
fetch("http://gstaging.getuwired.us/engconcepts/jordan/data.php", {mode: 'cors'})
                .then(response => response.json())
                .then(data => {
                    this.data = data;
                    this.loading = false;
                });
```

Update the template area of your component

```
<template>
    <div>
        <div v-if="loading">
            Please wait...
        </div>
        <div v-else>
            {{info}}
        </div>

    </div>
</template>
```

Now add your component to the main App.vue script

```
<script>
import Loader from './components/Loader';

export default {
  name: 'App',
  components: {
    Loader
  }
}
</script>
```

Update the main App.vue template

```
<template>
  <div id="app">
    <loader/>
  </div>
</template>
```

While the loader component is waiting for the fetch call it will display "Please Wait...". Once the fetch call is complete the component will display the json object.

### BONUS

Add a property to the loader component for the data url.

```
<script>
    export default {
        name: "ComponentName",
        mounted() {
            fetch()...
        },
        data() {
            return{
                loading: true,
                data: null
            }
        },
        props: {
            dataUrl:{
                required:true
            },
        }
    }
</script>
```

Now update the main App.vue template

```
<template>
  <div id="app">
    <loader v-bind:data-url="http://gstaging.getuwired.us/engconcepts/jordan/data.php"></loader>
  </div>
</template>
```

Update the fetch call url to this.dataUrl

```
fetch(this.dataUrl, {mode:"cors"})...
```

### Node Component

Use what you learned from creating the loader component to create a node component that will display the firstname, lastname, and role from the json object. Use a v-for loop to display all the workers.

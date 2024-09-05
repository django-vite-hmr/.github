# Django Vite HMR

This is a plugin for `Vite` and `Django`. This plugin provides functionalities for combining `Vite` `Hot Module Reloading` (HMR) with Django Project.

## Installation

Before we proceed with the installation, make sure your Django project is up-and-running. If you have met the criteria, proceed with the following steps.

### Django

-   **Installing Dependencies**

    Install `django_vite_hmr`, a django app, that provides functionality to efficiently serve static files compliant with vite `hmr`.

    ```bash
    pip install django-vite-hmr
    ```

-   **Updating Settings.py**

    Next, you need to update `projectname/settings.py`. You must include "django_vite_hmr" as installed apps.

    ```python
    INSTALLED_APPS = [
        "other_apps",
        "django_vite_hmr",
        "other_apps"
    ]
    ```

-   **Configuring Application Settings**

    This is optional. This is required only if you have made changes in vite configuration from next section.

    ```python
    DJANGO_VITE = {
        "DEBUG": DEBUG, # This will define which mode to serve static file.
        "HOST": "localhost", # This is the hostname of Vite Development Server
        "PORT": 5173, # This is the port of Vite Development Server
        "BASE": "" # This is the BASE of Vite Server
    }
    ```

### Vite

To use vite, you need to have [Node](https://nodejs.org/en) installed in your machine.

-   **Project Initialization**

    Run, following command to initialize the project.

    ```bash
    yarn init # Replace Yarn with NPM if you prefer
    ```

    This will ask you bunch of question which you will have to fill in or leave to default as per your project requirements.

    After the process is completed, it will generate `package.json` file.

-   **Update Package.json**

    `Vite` recommend type of package json to be module, so we need to add following key-value in package.json

    ```json
    {
        "name": "project-name",
        "version": "project-version",
        "main": "index.js",
        "type": "module" <--
        ...other
    }
    ```

-   **Installing Dependencies**

    Now, we need to install required dependencies

    ```bash
    yarn add vite @django-vite/vite-plugin # Again, use npm if you prefered.
    ```

-   **Configuration**

    Now, create a `vite.config.js` file at the root of the project and paste in following code.

    ```javascript
    import { defineConfig } from "vite"
    import { djangoVitePlugin } from "@django-vite/vite-plugin"

    export default defineConfig({
        plugins: [djangoVitePlugin()],
    })
    ```

-   **Additional Note**

    You can avoid all of these configuration, simply by running

    ```bash
    npx vite --strictPort
    ```

## Usages

Configure your TEMPLATES and STATIC file path as you normally do. Now, inside your template files, load the vite tag as following.

```django
{% comment %} Load Vite {% endcomment %}
{% load vite %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        {% comment %} Load static files {% endcomment %}
        {% vite_asset "/main.css" %}
        {% vite_asset "/main.js" %}

        <title>Django Template</title>
    </head>
    <body>
        {% comment %} You Body Content {% endcomment %}
    </body>
</html>
```

-   **{% load vite %}**: This will load django_vite_hmr templatetag
-   **{% vite_asset 'filename' attrbute_key="value" attribute_key="value" %}**: This will load your css/js file in way want that it provides `HMR` when DEBUG is True. Otherwise, load as normal static files.

## Starting Project

Now, you can serve your Django Project as usual.

```bash
python manage.py runserver
```

Open New Terminal is your project, and run `Vite`.

```bash
vite

# or

npx vite
```

## Developer Note

Since this plugin only allow you to hot reload `CSS` and `JavaScript` files. If you want hot reloading for HTML as well, you can try [Reloadium](https://pypi.org/project/reloadium/0.9.0/)

This doesn't exactly provide Hot Reloading but this can be used for Hands Free reloading (when .py and .html files changes).

### Usages of Reloadium

-   **Install Reloadium**

    ```bash
    pip install reloadium
    ```

-   **Run Project**

    ```bash
    reloadium run manage.py runserver
    ```

-   **Integration**

    You can setup django-vite-hmr as mentioned above and use `reloadium run` prefix instead of `python`.

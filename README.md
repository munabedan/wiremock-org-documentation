# Contributing to the WireMock website

All contributions to the website are welcome! Whether you develop new documentation, want to share your use-case, fix an issue or improve the websites look and feel...  just submit a pull request! See the contributor notes below.

General WireMock contributing guide is available [here](https://github.com/wiremock/community/tree/main/contributing).

## Discussing changes 

Make sure to join the community Slack as documented in the contributing guide. After that, all changes can be discussed in the `#help-contributing` or `#documentation` channels. Do not hesitate to ask there is you hit an obstacle.

## Preparing the developer environment

The website is powered by [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/), and hence a [python developer environment](https://www.python.org/downloads/) is needed. It is recommended to use the latest python3 version to avoid compatibility issues.

Ideally you should set up a python [virtual environment](https://realpython.com/what-is-pip/#using-pip-in-a-python-virtual-environment) within the project folder to avoid [pip](https://pip.pypa.io/en/stable/) package installer conflicts. 


## File Structure

All documentation sources should be written as regular Markdown files placed in the documentation directory. In this project we make use of the [mkdocs-monorepo-plugin](https://backstage.github.io/mkdocs-monorepo-plugin/) to allow us to have multiple documentation sets of documentation in a single mkdocs folder. This allows us to have multiple `docs/` folders: docs, dotnet and v4. By convention each homepage is should be named index.md.


```
├── dev                           # Convenient developer scripts
│   ├── build.sh
│   ├── clean.sh
│   ├── common.sh
│   ├── serve.sh
│   └── setup_env.sh
│  
├── docs                          # Wiremock documentation
│   ├── assets
│   │   ├── images                # Image files
│   │   └── stylesheets           # CSS files
│   ├── index.md                  # Wiremock documentation entry point
│   └── getting_started
│       ├── overview.md
│       └── ...
|
├── dotnet                        # .NET documentation
│   ├── docs
│   │   ├── index.md              # .NET documentation entry point
│   │   └── ...
│   ├── mkdocs.yml                # .NET documentation config 
│   └── nav.yml                   # .NET documentation navigation config
│                  
├── v4                            # WireMock Beta documentation
│   ├── docs
│   │   ├── index.md              # WireMock Beta documentation entry point
│   │   └── ...
│   └── mkdocs.yml                # WireMock Beta documentation config
│         
├── mkdocs.yml                    # WireMock documentation config
├── nav.yml                       # WireMock documentation navigation config
├── overrides                     # Custom mkdocs theme files
├── README.md                     
└── requirements.txt

```

## Writing your docs

Mkdocs pages must be authored in [Markdown](https://daringfireball.net/projects/markdown/). MkDocs uses the Python-Markdown library to render Markdown documents to HTML. In addition to the base Markdown [syntax](https://daringfireball.net/projects/markdown/syntax) MkDocs includes support for extending the syntax with [Python-Markdown extensions](https://squidfunk.github.io/mkdocs-material/setup/extensions/python-markdown-extensions/) these can be enabled or disabled as needed in the mkdocs.yml config file.

### Configure Pages and Navigation

The [nav](https://www.mkdocs.org/user-guide/configuration/#nav) setting in the `mkdocs.yml` file defines which pages are included in the global site navigation menu as well as the structure of that menu. In this project all the navigation configuration is in a separate file `nav.yml` which is then linked back to the documentation using the `INHERIT: nav.yml` tag.

Here is a sample navigation configuration from the project:

```yml
nav:
  - Getting started:
    - Overview: getting_started/overview.md
    - Quick Start API Mocking with Java and JUnit 4: getting_started/quick_start_api_mocking_with_java_junit_4.md
    - Download and Install: getting_started/download_and_installation.md
    - WireMock Tutorials: getting_started/wiremock_tutorials.md
    - Frequently Asked Questions: getting_started/frequently_asked_questions.md
    - Community Resources: getting_started/community_resources.md

  - Running Wiremock: 
    - WireMock standalone service: running_wiremock/wiremock_standalone_service.md
    - Running in docker: running_wiremock/running_in_docker.md
    - Running as a standalone process: running_wiremock/running_as_a_standalone_process.md
    - Administration API: running_wiremock/administration_api.md

  - WireMock .NET: "!include ./dotnet/mkdocs.yml"

  - WireMock Beta: "!include ./v4/mkdocs.yml"

...
```

Navigation subsections can be created by listing related pages together under a section title.

> Note that any pages not listed in the navigation configuration will still be rendered and included in the built site, however, they will not be linked from the global navigation and will not be included in the `previous` and `next` links. Such pages will be `hidden` unless linked to directly.

All paths in the navigation configuration must be relative to the documentation directory which can be configure with the `docs_dir` option in `mkdocs.yml`. The default value is set to `docs`.

> Note: If a title is defined for a page in the navigation , that title will override any title defined within the page itself.

### Links

Mkdocs allows for linking documents using the regular Markdown [link syntax](https://daringfireball.net/projects/markdown/syntax#link)  

- Linking to pages

  `[WireMock](https://github.com/wiremock/wiremock/)`
  `[Wiremock Tutorials](../getting_started/wiremock_tutorials.md)`

  MkDoc will automatically convert internal links like `../getting_started/wiremock_tutorials.md` into proper links to the corresponding generated HTML page `/getting_started/wiremock_tutorials/` so ensure to include the relative directory path in the link if the target document is in another directory.

  > Note: The path should be relative to the current Markdown file, not the root.

- Linking to images
  
  `![Screenshot](img/screenshot.png)`

  To include images in your documentation source files, simply use any of the regular Markdown image syntaxes

- Linking from raw HTML

  Markdown allows authors to fall back to raw HTML when the syntax does not meet the authors needs. 
  
  > Note: All HTML within the markdown file is ignored by the Markdown parser and as such MkDocs will not be able to validate or convert links contained in raw HTML. Ensure all links within this are manually formatted appropriately for the rendered document.

   `<img src="../../assets/images/logos/technology/android.svg">`

   This is an example of a link within raw HTML , we add `../../` to ensure the path to the image works relative to the location of the generated HTML file, not the Markdown file.

- Link to a section

  MKDocs generates an ID for every header in the Markdown documents. This ID can be used to link to a section within the target document using an anchor link.

  `[Wiremock Community](./support.md#wiremock_community)`

  > Note: IDs are created from the text of a header. All text is converted to lowercase and any disallowed characters, including white-space, are converted to dashes. Consecutive dashes are then reduced to a single dash.

### Fenced Code Blocks

MkDocs provides different ways to set up syntax highlighting for code blocks.

- Code blocks

````
 ```java
  wireMockServer.stubFor(requestMatching(request ->
      MatchResult.of(request.getBody().length > 2048)
  ).willReturn(aResponse().withStatus(422)));
 ``` 
````

- Code Tabs

````
  === "Java"

      ```java
      stubFor(get(urlEqualTo("/local-transform")).willReturn(aResponse()
              .withStatus(200)
              .withBody("Original body")
              .withTransformers("my-transformer", "other-transformer")));
      ```

  === "JSON"

      ```json
      {
          "request": {
              "method": "GET",
              "url": "/local-transform"
          },
          "response": {
              "status": 200,
              "body": "Original body",
              "transformers": ["my-transformer", "other-transformer"]
          }
      }
      ```
````

### Admonitions

MkDocs provides [callouts](https://squidfunk.github.io/mkdocs-material/reference/admonitions/?h=admon#supported-types) for including side content. The project has a custom callout for Wiremock Cloud that can be used with the following syntax:

```
!!! wiremock-cloud "WireMock Cloud"

    [Create stubs and scenarios with WireMock Cloud's intuitive editor and share with your team.](https://www.wiremock.io?utm_source=oss-docs&utm_medium=oss-docs&utm_campaign=cloud-callouts-proxying&utm_id=cloud-callouts&utm_term=cloud-callouts-proxying)

```


### Document content

MkDocs attempts to derive the document title with four methods; the nav title, the `title` meta-data key at the start of a document, the level 1 markdown header `#` and the filename of the document.

To avoid unexpected outcomes , it is important to define the title of the document in the document meta-data, as shown below:

```
  ---
  title: Lorem ipsum dolor sit amet 
  ---

  <br>

  # Lorem ipsum dolor sit amet

```
> Note: Include the `<br>` before the start of a document to ensure the breadcrumb links `Documentation / getting_started / overview` are correctly displayed at the start of the document.

### Variables

This project comes packaged with the [mkdocs-macros plugin](https://mkdocs-macros-plugin.readthedocs.io/en/latest/) that provides variable substitution capabilities. In the config file `mkdocs.yml` , define variables in the `extra:` section:


```yml
  extra:
    wiremock_version: 3.13.1
    wiremock_4_version: 4.0.0-beta.10
```

These variables can then be shared across the documents within macros `{{ wiremock_version }}` and the correct value will be substituted. 

<blockquote>

Note: Sometimes you may need to disable mkdocs from rendering a section your document with the conflicting syntax `{{ }}`. In this case disable rendering with  

```
  {% raw %}
  
  {{ variable }}

  {% endraw %} 
```

Without `{% raw %}`, MkDocs will try to render `{{ variable }}` as a real variable (which may break your build or show unexpected output).

</blockquote>

## Build and Serve

The repo includes a few convenience scripts to help setup and run the project in the 'dev' directory.

- Setup

This script simply installs all dependencies in the `requirements.txt`

```bash
./dev/setup_env.sh
```

- Serve

This script starts the mkdocs live preview server so you can preview as you write your documentation. The server will automatically rebuild the site upon saving. 

> Note: If you make changes to the config files the server will have to be restarted manually for the changes to be reflected

```bash
./dev/serve.sh
```

- Build

This builds the mkdocs documentation which generates a `site` folder with the rendered HTML documents.

```bash
./dev/build.sh
```

Mkdocs provides [additional options](https://www.mkdocs.org/user-guide/cli/) for the commandline interface if needed.













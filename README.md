# eleventy-plugin-plantuml

> Eleventy Plantuml plugin. Uses sync request to connect to a plantuml server to convert markddown code block of type 
> plantuml to an inline dataurl png ```<img>```

## Dependencies
* [eleventy](https://www.npmjs.com/package/@11ty/eleventy) A simpler static site generator for which this plugin is make.
* [sync-request](https://www.npmjs.com/package/sync-request) (for making blocking synchronous http request - not to be used in production)
* [jest](https://www.npmjs.com/package/jest) (for executing unit tests)

## Installation

> This plugin requires markdown syntax highlighter plugin to work.

Add Eleventy syntaxhighlight plugin
```javascript
const syntaxHighlight = require('@11ty/eleventy-plugin-syntaxhighlight');
```

Add plugin to .eleventy.js configuration file and optionally provide the configuration for private Plantuml server
```javascript
    eleventyConfig.addPlugin(syntaxHighlight);
    eleventyConfig.addPlugin(plantuml, {
        hostname: "localhost",
        port: 8888,
        prefix: "",
        imgClass: "plantuml"
    });
```

if options are omited, the plugin defaults to <http://plnntuml.com/plantuml> server for conversion. 

The generated img tag will have class plantuml assigned to it. This can be override using options.imgClass value.

## Using in templates
Simply create a markdown code block of type plantuml and it will be replaced by an img with inline png src (dataurl).

>In the following example, there is an extra space between the ticks ` for escaping

```
`` `plantuml
@startuml
!include https://raw.githubusercontent.com/bschwarz/puml-themes/master/themes/bluegray/puml-theme-bluegray.puml
participant "Makrdown Highlighter" as MDH
participant "eleventy-plugin-plantumt" as plugin
participant "Plantuml Server" as plantuml

MDH -> plugin : highlight
plugin -> plugin: compress url
plugin -> plantuml: GET /png/{url}
plantuml -> plugin: image/png
plugin -> plugin: base64
plugin -> MDH: img src="dataurl"
@enduml
`` `
```

## Contribute
* create a fork of this repo 
* make your changes
* test your code by running ```yarn test``` or ```npm test```
* create a Pull Request


## Testing
Execute ```yarn test``` or ```npm test``` to execute integration tests against <plantuml.com/plantuml> live server
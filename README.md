Source for my portfolio site.

Processed via Jekyll.

Projects are handled as posts, and are assumed to have the following front-matter:

 - `layout: post`
 - `title:` the title of the project
 - `category:` or `categories:` should probably be either `project` or `oss` for open source libraries (not sure if I will use anything other than project for now).
 - `tags:` space separated category tags for the project (e.g. `interactive`, `bot`, `web`)
 - `images:` YAML sequence for main image used to illustrate the project.  Typically will only be one, but if additional are provided, will also be used.  These images show up in "index" views of projects.
 - `additional_images:` YAML sequence of extra images which will only show up in a "detail" view for a project. (optional)
 - `github:` URL to the source code, if applicable. (optional)
 - `web:` URL to the project itself, if applicable. (optional)
 - `year:` Year for the project -- treated as a string for display purposes, use standard post methodology for actual ordering date.

---
layout: post
title:  "Notes from the website building session"
date:   2024-12-05 15:00:00 +0000
categories: meetings
---
Here are some notes from the building a website meeting.

# Personal webpages
Having a research website is important. There are lots of ways to do it.
### On linux space
- write the website in your public.html file and then it will be hosted on something along the lines of users.earth.ox.ac.uk/~user0123/index.html
-   This gives a static page that is hosted in the department

### On a similar system run by the university
-   https://help.it.ox.ac.uk/guide-to-using-personal-web-pages-service
-   These still work if the department doesn’t have power

### github.io
-   this lets you move university and keep your website
####    andreww.github.io
-   at its most simple, you can just write some html
####    compearthsci.github.io
-   Uses a templating language (Jekyll) rather than needing to write raw html
-   Write content in markdown
-   Simple syntax, you can put headings etc.
-   When you commit these documents to github, it builds the website.
-   You can do fancy things with this
-   Jekyll template does the transformation from markdown to html and then adds on the css

# Websites for software projects
### through github
- e.g. greenscheduler.github.io
- Have a github workflow which builds and deploys documentation
- Keep documentation up to date in a folder on github (e.g. quickstart info) and can also pull out documentation from the code (e.g. docstrings)
- e.g. use Sphinx for the documentation
- JOS papers are also written in markdown
### through readthedocs
- e.g. terratools.github.io
- Have documentation in repository in docs folder
- Use readthedocs and grant read access to repository. This rebuilds the docs everytime the repo changes.
- Advantage of readthedocs is that it keeps more versioning.




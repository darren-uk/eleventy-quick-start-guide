STEPS TO RUN ELEVENTY

//npm install -g @11ty/eleventy
if error message about execution policy (WINDOWS ONLY) use command in powershell (as admin)
//Set-ExecutionPolicy -ExecutionPolicy Unrestricted

WARNING -- WILL MAKE SYSTEM VUNERABLE TO MALWARE IF LATER USE NPM INSTALLS FROM UNTRUSTED VENDORS

// eleventy is installed globally and execution policy has been set to unrestricted.

For more details
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1

1#

Add .gitignore to root & inside add folders and files for build to ignore

2#

Template files go in _includes folders

3#

To run build type eleventy in terminal
To run build and view in browser type eleventy --serve in terminal
To cancel view in browser hold ctrl + c in terminal

4#

To add folders to build that are not automatic such as CSS folders or images.

Add to .eleventy.js

module.exports = function (eleventyConfig) {
    eleventyConfig.addPassthroughCopy('css');
     eleventyConfig.addPassthroughCopy('images');

    return {
        passthroughFileCopy: true
    }
};

5#

Simple post loop with fallback (Display list of all posts)

{%- for post in collections.post -%}
    <li><a href="{{ post.url }}">{{ post.data.title }}</a> -- {%- if post.data.summary == undefined -%}
        No summary available <a href="{{ post.url }}"> Read more ...</a>
        {%- else -%}
        {{post.data.summary}} <a href="{{ post.url }}"> Read more ...</a>
        {%- endif -%}
    </li>
{%- endfor -%}

6#

add 'post tag to all files frontmatter in folder.
add file called blog.json to folder
add following code to json file
{
    "tags": "post"
}

if you add tags directly to frontmatter in file they will overwrite.

7#

LUXON is a dependency to change display of date format
add to project by typing in terminal

npm install --save luxon

this will add package-lock.json file in root & node_modules folders
(eleventy build should ignore package-lock.json in build but node_modules folder should be added to .gitignore)

ADD to .eleventy.json

const {
    DateTime
} = require("luxon");

module.exports = function (eleventyConfig) {
eleventyConfig.addFilter("postDate", (dateObj) => {
        return DateTime.fromJSDate(dateObj).toLocaleString(DateTime.DATE_MED);
    });
};

ADD in template file (or where date is displayed) a filter called postDate

eg. {{ post.data.date | postDate }}

or

eg. {%- if post.data.date == undefined -%}
        {{ post.date | postDate }}
        {%- else -%}
        {{ post.data.date | postDate }}
        {%- endif -%}

        // creates fallback to automatic date
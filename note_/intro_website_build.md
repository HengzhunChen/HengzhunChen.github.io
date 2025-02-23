---
layout: splash
author_profile: false
---

A Guidance for Website Generation Using Jekyll
=================================================

Websites for further introduction: 
  - [Jekyll](https://miamarti.github.io/) 
  - [Ruby](https://www.ruby-lang.org/en/) 
  - [NodeJS](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
  - [Liquid](https://shopify.github.io/liquid/basics/introduction/)

You'd better know some basic HTML (HyperText Markup Language) and CSS (Cascading Style Sheets).

Here are some basic questions for the background and some steps to build the website. 

For more explanation and function in web page design, please refer to [Jekyll docs](https://miamarti.github.io/docs/home/). 

## Preliminary

**Q**: What is Jekyll?

**A**: Jekyll is a popular static site generator written in Ruby. It is designed to simplify the creation of static websites by converting plain text files into HTML pages. Jekyll follows the principles of simplicity, ease of use, and flexibility.

Here are some key points about Jekyll:

- Static Site Generator: Jekyll is a static site generator, which means it generates static HTML files that can be served directly by a web server. Unlike dynamic websites that generate content on the fly, static sites are pre-built and do not require server-side processing.

- Markdown and Liquid Templating: Jekyll uses Markdown, a lightweight markup language, to write content. Markdown allows you to focus on writing content without worrying about HTML syntax. Jekyll also uses Liquid, a templating language, to add dynamic elements and reusable components to your site.

- Directory Structure: Jekyll follows a specific directory structure. The main content files are stored in the _posts directory, and other pages can be organized in different directories. Jekyll uses layouts, includes, and data files to structure and style your site consistently.

- Front Matter: Each content file in Jekyll contains a section called "Front Matter" at the beginning, enclosed between two sets of triple-dashes (---). The Front Matter allows you to specify metadata, such as the layout to use, title, date, and custom variables, for each content file.

- Plugins and Themes: Jekyll supports plugins, which extend its functionality. Plugins can be used to add features like sitemaps, pagination, syntax highlighting, and more. Additionally, Jekyll offers a wide range of themes that provide pre-designed layouts and styles, allowing you to quickly customize the look and feel of your site.

- GitHub Pages Integration: Jekyll is tightly integrated with GitHub Pages, a web hosting service provided by GitHub. You can host your Jekyll-powered site on GitHub Pages for free, making it easy to publish and share your static website.

Jekyll is widely used by developers, bloggers, and organizations to create static websites, blogs, documentation sites, and more. Its simplicity, flexibility, and integration with GitHub make it a popular choice for generating static sites efficiently.

**Q**: How does Jekyll work?

**A**: When you run Jekyll, it follows a set of steps to generate your website:

- Read Configuration: Jekyll starts by reading the configuration file (_config.yml) in your project's root directory. This file contains various settings for your Jekyll site, such as the site title, URL structure, theme, plugins, and more.

- Process Source Files: Jekyll then processes your source files, which typically include Markdown files, HTML layouts, CSS stylesheets, and other assets.

  - Markdown Files: Jekyll converts Markdown files to HTML, applying any specified layout templates and converting Markdown syntax to HTML elements.
  - Layouts: Jekyll uses layout files to define the structure and appearance of your website's pages. You can create reusable layout templates that define the common elements, such as headers, footers, and sidebars, to be included in multiple pages.
  - Includes: Jekyll allows you to define reusable snippets of code called includes. These can be used within layouts or Markdown files to insert common content, such as navigation menus or code snippets.
  - Data Files: Jekyll supports data files in various formats (e.g., YAML, JSON, CSV). These files can be used to store structured data that can be accessed within your templates and content.
  - Plugins: Jekyll offers a plugin system that allows you to extend its functionality. Plugins can perform various tasks, such as generating additional content, applying transformations to the source files, or integrating with external services.

- Generate Output: As Jekyll processes your source files, it generates the static HTML pages that make up your website. These HTML pages, along with any other assets like CSS, JavaScript, and images, are saved in the output directory (by default _site).

- Serve the Website: Once the site is generated, Jekyll can optionally start a local development server to serve the static files. The server provides live reloading, meaning it monitors the source files for changes. When changes are detected, Jekyll automatically regenerates the affected parts of the site and triggers a browser refresh.

- Deployment: Once you're satisfied with your generated site, you can deploy it to a web server or hosting platform. Since Jekyll generates static files, you can simply upload the generated _site directory to your web server or use services that support static site hosting.

By following this process, Jekyll enables you to build and maintain websites with ease, allowing you to focus on creating content and designing the site's structure while leaving the heavy lifting of generating static files to the Jekyll engine.


**Q**: What is Ruby, Gems and Bundle?

**A**: Ruby is a programming language known for its simplicity and readability. It provides a framework for developing various types of applications, including web applications. Ruby has its own package manager called RubyGems, which allows developers to easily manage and distribute Ruby libraries and applications.

Gems are packages or libraries written in Ruby that provide additional functionality to Ruby applications. Gems can be created by developers and shared with others to be easily installed and used in Ruby projects. They can include libraries, frameworks, command-line tools, and more.

Bundle is a tool that works in conjunction with RubyGems to manage the dependencies of a Ruby project. It helps ensure that a project's required gems and their versions are correctly installed and used. Bundle reads a project's Gemfile, which specifies the required gems and their versions, and installs them accordingly.

The relationship between Ruby, Gem, and Bundle can be summarized as follows:

- Ruby: The programming language itself, providing the core functionality and syntax.
RubyGems: The package manager for Ruby, allowing developers to manage and distribute Ruby libraries and applications (gems).
- Gems: Packages or libraries written in Ruby that provide additional functionality to Ruby applications. Gems are managed and distributed using RubyGems.
- Bundle: A tool that works in conjunction with RubyGems to manage the dependencies of a Ruby project. It reads a project's Gemfile and installs the required gems and their versions.

In summary, Ruby is the programming language, RubyGems is the package manager, Gems are the packages or libraries, and Bundle is a tool that helps manage the dependencies of a Ruby project using RubyGems.


**Q**: How to manage the different gems used for different projects which may required different versions of the same gem?

**A**: To manage different gems used for different projects that might require different versions of the same gem, you can utilize Ruby version managers and Bundler's gem version management capabilities. Here's an approach you can follow:

- Use a Ruby Version Manager: A Ruby version manager allows you to manage multiple versions of Ruby on your system. This helps ensure that each project can use its specific version of Ruby. There are two popular Ruby version managers: rbenv and RVM. Choose the version manager that suits your preferences and follow the installation instructions for the chosen tool.

- Use a Gemfile for Each Project: Bundler is a tool that manages gem dependencies for Ruby projects. By using a Gemfile in each project directory, you can specify the required gems and their versions for that specific project.
  - In your project directory, create a file named Gemfile (without any extension).
  - Inside the Gemfile, specify the gems and versions required for your project. For example:
    ``` ruby
    source 'https://rubygems.org'
    gem 'rails', '6.0.0'
    gem 'nokogiri', '1.12.0'
    ```
  - Run `bundle install` in the project directory to install the specified gems and their dependencies. Bundler ensures that the correct gem versions are installed based on the Gemfile.

- Separate Gem Environments: With the combination of a Ruby version manager and Bundler, you can create separate gem environments for each project. 
  - When you switch to a project directory using rbenv or RVM, the corresponding Ruby version and gemset (if using RVM) will be automatically selected. 
  - Run `bundle install` within the project directory to install the specific gem versions specified in the Gemfile for that project.


**Q**: Where does the gem file being installed if I used bundler to manage them?

**A**: When you use Bundler to manage gems for a project, the gems specified in the Gemfile are installed in the project's directory. By default, Bundler creates a directory called vendor/bundle within the project directory and installs the gems there.

The vendor/bundle directory structure looks like this:

```
project_directory/
├── Gemfile
├── Gemfile.lock
└── vendor/
    └── bundle/
        ├── ruby/
        │   └── <ruby_version>/
        │        ├── bin/
        │        └── gems/
        │            ├── gem1/
        │            ├── gem2/
        │            └── ...
        └── cache/
            ├── gem1.gem
            ├── gem2.gem
            └── ...
```

- Gemfile: This file contains the gem dependencies and their versions for the project.

- Gemfile.lock: This file is generated by Bundler and locks the specific versions of the gems installed in the project. It ensures that the same gem versions are used across different environments.

- vendor/bundle/ruby/<ruby_version>/bin/: This directory contains the executable binaries provided by the installed gems. You can run these binaries directly from the project's directory.
vendor/bundle/ruby/<ruby_version>/gems/: This directory contains the installed gems. Each gem has its own directory within this folder.

- vendor/bundle/cache/: This directory stores the cached gem files (.gem) downloaded during the installation process. Cached gems are reused to speed up subsequent installations.

When you run bundle install within a project directory, Bundler reads the Gemfile, resolves the gem dependencies, and installs them in the vendor/bundle directory structure. This helps keep the project's gems isolated from the system-wide gems, ensuring that the project has its own specific gem environment.

Note that if you're using a different configuration or have customized Bundler's settings, the location of the installed gems might differ. However, the vendor/bundle structure is the default behavior, and it's widely used in most Ruby projects managed by Bundler.

**Q**: Where is the location of those gem files if I used command gem install packages?

**A**: When you use the gem install command to install gems directly without Bundler, the gem files are typically installed in a system-wide location based on your Ruby installation. The specific location can vary depending on your operating system and how Ruby was installed.
Here are the typical locations for gem installations:
`/usr/local/lib/ruby/gems/<ruby_version>/gems/`
The <ruby_version> part in the path represents the installed Ruby version, such as 2.7.4 or 3.0.2.

It's important to note that installing gems using gem install without Bundler can install them globally on your system, making them available to all projects using the same Ruby installation. This might lead to potential conflicts or version mismatches between projects that require different gem versions.
To avoid such conflicts, it is recommended to use Bundler and manage gem dependencies within each project separately. Bundler provides a way to isolate and manage gems per project, as discussed earlier.

**Q**: How to install ruby, rubygem and bundler over Linux?


**A**: Install Ruby: 

Ruby is often pre-installed on macOS and Linux systems. You can check if Ruby is already installed by opening a terminal and running the command `ruby --version`. If it's not installed or you need a different version, you can use a version manager like rbenv or RVM to install and manage Ruby versions.

Install RubyGems:

RubyGems usually comes bundled with Ruby, so you should have it installed already if you installed Ruby using a standard installation method. You can verify this by running the command `gem --version` in the terminal. If RubyGems is not installed or you need to update it, you can do so by running `gem update --system`.

Install Bundler:

Once RubyGems is installed, you can use it to install the Bundler gem. Open a terminal and run the command `gem install bundler` to install Bundle.
After the installation, you can verify that Bundle is installed correctly by running `bundle --version` in the terminal. You should see the version number of Bundle printed if the installation was successful.


**Q**: How to use Bundler?

**A**: To use Bundler, follow these steps:

- Install Bundler: Ensure that Bundler is installed on your system. If you don't have it installed, you can install it by running the following command:
  ```
  gem install bundler
  ```

- Create a Gemfile: In your project's directory, create a file named Gemfile. This file will specify the gems and their versions that your project depends on. Open the Gemfile in a text editor.

- Specify gem dependencies: Inside the Gemfile, use the RubyGems syntax to specify the gems and their versions that your project requires. For example:
  ``` ruby
  source "https://rubygems.org"
  gem "jekyll", "<= 3.0"
  ```

- Install gems: In your project's directory, run the following command to install the gems specified in the Gemfile:
  ``` ruby
  bundle config set --local path 'vendor/bundle'
  bundle install
  ```
  Bundler will read the `Gemfile`, resolve the gem dependencies, and install the required gems. The installed gems will be placed in a directory called `vendor/bundle` within your project's directory.

- Use bundled gems: Once the gems are installed, you can use them in your project. If you're running Ruby scripts or executing commands, prefix them with `bundle exec`` to ensure that the correct gem versions are used. For example:
  ``` ruby
  bundle exec ruby script.rb
  ```
  This ensures that the `script.rb` file is executed within the bundled environment, using the gems specified in the `Gemfile`.

- Update gems: If you need to update the gems in your project, modify the Gemfile with the desired gem versions, and then run bundle update to update the gems accordingly.

Bundler helps manage and isolate gem dependencies for your projects, making it easier to maintain consistent gem environments across different machines and projects.

**Remark:** Frequently used command to view the website in local host. 

Remember to go to the directory of your project in which the `_config.yml` file locates.
``` ruby
bundle exec jekyll serve  # live serve with default local website http://localhost:4000
bundle exec jekyll serve --port=12345  # live serve with local website port 12345 (can be replaced by any acceptable numbers) 
```
You can use `bundle exec jekyll new myblog` to initialize a blank website in folder `myblog`.

**Q**: What is the relationship between Liquid and HTML?

**A**: Liquid is a templating language used in HTML templates to add dynamic elements and logic. It was developed by Shopify and is widely used in various web development frameworks, including Jekyll, which is a popular static site generator. Liquid provides a way to insert variables, perform conditional statements, iterate over collections, and apply filters to manipulate data within HTML templates. It allows you to separate the presentation layer from the data and logic, making your templates more flexible and reusable.

------------------------------------------


## Small Tips 

Remark: From my personal experience, instead of modifying the code directly without any knowledge to check whether it implements the desired function, it is better to understand the file structure and how they are organized to generate the website.

### Markdown usage

- Writing expressions as blocks
  
  To add a math expression as a block, start a new line and delimit the expression with two dollar symbols `$$`

  A blank line between last word line and the started symbol `$$`, and a blank line between the end symbol `$$` and next word line are necessary to make it work correctly
 
- Symbol conflicts between LaTeX and Markdown

  - The absolute value symbol `|` in inline LaTeX expression may confused with the table separation of Markdown with different browsers, use command `\vert` instead of `|`. Similar, use `\Vert` instead of `\|` for norm.

  - There should leave a space between `*` and the following LaTeX code to make it work as expected, for example `$x^* \in A$`. Furthermore, use command `\ast` instead of `*`

  - Sometimes you need to include the subscript into the command of the special mathematical fonts, e.g., use `\mathcal{G_n}` instead of `\mathcal{G}_n`

- ...


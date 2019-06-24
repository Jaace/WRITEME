# Gooey (Beta)

Turn (almost) any Python @abstr_number or @abstr_number Console Program into a GUI application with one line

@abstr_image 

# Gooey now supports Python @abstr_number !!

## Table of Contents

  * Gooey
  * Table of contents
  * Latest Update
  * Quick Start 
    * Installation Instructions
    * Usage
    * Examples
  * What It Is
  * Why Is It
  * Who is this for
  * How does it work
  * Internationalization
  * Global Configuration
  * Layout Customization
  * Run Modes 
    * Full/Advanced
    * Basic
    * No Config
  * Input Validation
  * Using Dynamic Values
  * Showing Progress
  * Customizing Icons
  * Packaging
  * Screenshots
  * Contributing
  * Image Credits



* * *

## Quick Start

### Installation instructions

The easiest way to install Gooey is via `pip`
    
    
    pip install Gooey
    

Alternatively, you can install Gooey by cloning the project to your local directory
    
    
    git clone https://github.com/chriskiehl/Gooey.git
    

run `setup.py`
    
    
    python setup.py install
    

**NOTE:** Python @abstr_number users must manually install WxPython! Unfortunately, this cannot be done as part of the pip installation and should be manually downloaded from the @abstr_hyperlink .

### Usage

Gooey is attached to your code via a simple decorator on whichever method has your `argparse` declarations (usually `main`).
    
    
    from gooey import Gooey
    
    @Gooey      <--- all it takes! :)
    def main():
      parser = ArgumentParser(...)
      # rest of code
    

Different styling and functionality can be configured by passing arguments into the decorator.
    
    
    # options
    @Gooey(advanced=Boolean,          # toggle whether to show advanced config or not 
           language=language_string,  # Translations configurable via json
           show_config=True,          # skip config screens all together
           target=executable_cmd,     # Explicitly set the subprocess executable arguments
           program_name='name',       # Defaults to script name
           program_description,       # Defaults to ArgParse Description
           default_size=( @abstr_number ,  @abstr_number ),   # starting size of the GUI
           required_cols= @abstr_number ,           # number of columns in the "Required" section
           optional_cols= @abstr_number ,           # number of columbs in the "Optional" section
           dump_build_config=False,   # Dump the JSON Gooey uses to configure itself
           load_build_config=None,    # Loads a JSON Gooey-generated configuration
           monospace_display=False)   # Uses a mono-spaced font in the output screen
    )
    def main():
      parser = ArgumentParser(...)
      # rest of code
    

See: How does it Work section for details on each option.

Gooey will do its best to choose sensible widget defaults to display in the GUI. However, if more fine tuning is desired, you can use the drop-in replacement `GooeyParser` in place of `ArgumentParser`. This lets you control which widget displays in the GUI. See: GooeyParser
    
    
    from gooey import Gooey, GooeyParser
    
    @Gooey
    def main():
      parser = GooeyParser(description="My Cool GUI Program!") 
      parser.add_argument('Filename', widget="FileChooser")
      parser.add_argument('Date', widget="DateChooser")
      ...
    

### Examples

Gooey downloaded and installed? Great! Wanna see it in action? Head over the the @abstr_hyperlink to download a few ready-to-go example scripts. They'll give you a quick tour of all Gooey's various layouts, widgets, and features. 

@abstr_hyperlink 

## What is it? 

Gooey converts your Console Applications into end-user-friendly GUI applications. It lets you focus on building robust, configurable programs in a familiar way, all without having to worry about how it will be presented to and interacted with by your average user. 

## Why?

Because as much as we love the command prompt, the rest of the world looks at it like an ugly relic from the early ' @abstr_number s. On top of that, more often than not programs need to do more than just one thing, and that means giving options, which previously meant either building a GUI, or trying to explain how to supply arguments to a Console Application. Gooey was made to (hopefully) solve those problems. It makes programs easy to use, and pretty to look at! 

## Who is this for?

If you're building utilities for yourself, other programmers, or something which produces a result that you want to capture and pipe over to another console application (e.g. *nix philosophy utils), Gooey probably isn't the tool for you. However, if you're building 'run and done,' around-the-office-style scripts, things that shovel bits from point A to point B, or simply something that's targeted at a non-programmer, Gooey is the perfect tool for the job. It lets you build as complex of an application as your heart desires all while getting the GUI side for free. 

## How does it work?

Gooey is attached to your code via a simple decorator on whichever method has your `argparse` declarations.
    
    
    @Gooey
    def my_run_func():
      parser = ArgumentParser(...)
      # rest of code
    

At run-time, it parses your Python script for all references to `ArgumentParser`. (The older `optparse` is currently not supported.) These references are then extracted, assigned a `component type` based on the `'action'` they provide, and finally used to assemble the GUI. 

#### Mappings:

Gooey does its best to choose sensible defaults based on the options it finds. Currently, `ArgumentParser._actions` are mapped to the following `WX` components. 

| Parser Action | Widget | Example | |:----------------------|-----------|------| | store | TextCtrl | @abstr_image | | store_const | CheckBox | @abstr_image | | store_true| CheckBox | @abstr_image | | store_False | CheckBox| @abstr_image | | append | TextCtrl | @abstr_image | | count| DropDown | @abstr_image | | Mutually Exclusive Group | RadioGroup | @abstr_image |choice | DropDown | @abstr_image |

### GooeyParser

If the above defaults aren't cutting it, you can control the exact widget type by using the drop-in `ArgumentParser` replacement `GooeyParser`. This gives you the additional keyword argument `widget`, to which you can supply the name of the component you want to display. Best part? You don't have to change any of your `argparse` code to use it. Drop it in, and you're good to go. 

**Example:**
    
    
    from argparse import ArgumentParser
    ....
    
    def main(): 
        parser = ArgumentParser(description="My Cool Gooey App!")
        parser.add_argument('filename', help="name of the file to process")
    

Given then above, Gooey would select a normal `TextField` as the widget type like this: 

@abstr_image 

However, by dropping in `GooeyParser` and supplying a `widget` name, you can display a much more user friendly `FileChooser`
    
    
    from gooey import GooeyParser
    ....
    
    def main(): 
        parser = GooeyParser(description="My Cool Gooey App!")
        parser.add_argument('filename', help="name of the file to process", widget='FileChooser')
    

@abstr_image 

**Custom Widgets:** | Widget | Example | |----------------|------------------------------| | DirChooser/FileChooser/MultiFileChooser | 

@abstr_image 

| | DateChooser | 

@abstr_image 

| | PasswordField | 

@abstr_image 

| | Listbox | ![image](https://user-images.githubusercontent.com/ @abstr_number / @abstr_number -fadd @abstr_number f @abstr_number -b @abstr_number c @abstr_number - @abstr_number e @abstr_number - @abstr_number a @abstr_number - @abstr_number cbf @abstr_number c @abstr_number d @abstr_number d @abstr_number .png) | Internationalization \-------------------- @abstr_image Gooey is international ready and easily ported to your host language. Languages are controlled via an argument to the `Gooey` decorator. @Gooey(language='russian') def main(): ... All program text is stored externally in `json` files. So adding new langauge support is as easy as pasting a few key/value pairs in the `gooey/languages/` directory. Thanks to some awesome [contributers](https://github.com/chriskiehl/Gooey/graphs/contributors), Gooey currently comes pre-stocked with the following language sets: \- English \- Dutch \- French \- Portuguese Want to add another one? Submit a [pull request!](https://github.com/chriskiehl/Gooey/compare) \------------------------------------------- Global Configuration \-------------------- Just about everything in Gooey's overall look and feel can be customized by passing arguments to the decorator. | Parameter | Summary | |-----------|---------| | encoding | Text encoding to use when displaying characters (default: 'utf- @abstr_number ') | | use_legacy_titles | Rewrites the default argparse group name from "Positional" to "Required". This is primarily for retaining backward compatibilty with previous versions of Gooey (which had poor support/awareness of groups and did its own naive bucketing of arguments). | | advanced | Toggles whether to show the 'full' configuration screen, or a simplified version | | auto_start | Skips the configuration all together and runs the program immediately | | language | Tells Gooey which language set to load from the `gooey/languages` directory.| | target | Tells Gooey how to re-invoke itself. By default Gooey will find python, but this allows you to specify the program (and arguments if supplied).| |program_name | The name displayed in the title bar of the GUI window. If not supplied, the title defaults to the script name pulled from `sys.argv[ @abstr_number ]`. | | program_description | Sets the text displayed in the top panel of the `Settings` screen. Defaults to the description pulled from `ArgumentParser`. | | default_size | Initial size of the window | | required_cols | Controls how many columns are in the Required Arguments section   
:warning: **Deprecation notice:** See [Group Parameters](#group-configuration) for modern layout controls| | optional_cols | Controls how many columns are in the Optional Arguments section   
:warning: **Deprecation notice:** See [Group Parameters](#group-configuration) for modern layout controls| | dump_build_config | Saves a `json` copy of its build configuration on disk for reuse/editing | | load_build_config | Loads a `json` copy of its build configuration from disk | | monospace_display | Uses a mono-spaced font in the output screen   
:warning: **Deprecation notice:** See [Group Parameters](#group-configuration) for modern font configuration| | image_dir | Path to the directory in which Gooey should look for custom images/icons | | language_dir | Path to the directory in which Gooey should look for custom languages files | | disable_stop_button | Disable the `Stop` button when running | | show_stop_warning | Displays a warning modal before allowing the user to force termination of your program | | force_stop_is_error | Toggles whether an early termination by the shows the success or error screen | | show_success_modal | Toggles whether or not to show a summary modal after a successful run | | run_validators | Controls whether or not to have Gooey perform validation before calling your program | | poll_external_updates | (Experimental!) When True, Gooey will call your code with a `gooey-seed-ui` CLI argument and use the response to fill out dynamic values in the UI (See: [Using Dynamic Values](#using-dynamic-values))| | return_to_config | When True, Gooey will return to the configuration settings window upon successful run | | progress_regex | A text regex used to pattern match runtime progress information. See: [Showing Progress](#showing-progress) for a detailed how-to | | progress_expr | A python expression applied to any matches found via the `progress_regex`. See: [Showing Progress](#showing-progress) for a detailed how-to | | disable_progress_bar_animation | Disable the progress bar | | navigation | Sets the "navigation" style of Gooey's top level window.   
Options:  TABBED| SIDEBAR  
---|---  
@abstr_image |  @abstr_image   
| | sidebar_title | @abstr_image Controls the heading title above the SideBar's navigation pane. Defaults to: "Actions" | | show_sidebar | Show/Hide the sidebar in when navigation mode == `SIDEBAR` | | body_bg_color | HEX value of the main Gooey window | | header_bg_color | HEX value of the header background | | header_height | height in pixels of the header | | header_show_title | Show/Hide the header title | | header_show_subtitle | Show/Hide the header subtitle | | footer_bg_color | HEX value of the Footer background | | sidebar_bg_color | HEX value of the Sidebar's background | | terminal_panel_color | HEX value of the terminal's panel | | terminal_font_color | HEX value of the font displayed in Gooey's terminal | | terminal_font_family | Name of the Font Family to use in the terminal | | terminal_font_weight | Weight of the font (NORMAL|BOLD) | | terminal_font_size | Point size of the font displayed in the terminal | | error_color | HEX value of the text displayed when a validation error occurs | Layout Customization \-------------------- You can achieve fairly flexible layouts with Gooey by using a few simple customizations. At the highest level, you have several overall layout options controllable via various arguments to the Gooey decorator. | `show_sidebar=True` | `show_sidebar=False` | `navigation='TABBED'` | `tabbed_groups=True` | |---------------------|----------------------|----------------------|------------------------| | @abstr_image | @abstr_image | @abstr_image | @abstr_image | **Grouping Inputs** By default, if you're using Argparse with Gooey, your inputs will be split into two buckets: `positional` and `optional`. However, these aren't always the most descriptive groups to present to your user. You can arbitrarily bucket inputs into logic groups and customize the layout of each. With `argparse` this is done via `add_argument_group()` @abstr_image @abstr_code_section You can add arguments to the group as normal @abstr_code_section Which will display them as part of the group within the UI. **Customizing Group Layout** > Note: Make sure you're using GooeyParser if you want to take advantage of the layout customizations! With a group created, we can now start tweaking how it looks! `GooeyParser` extends the API of `add_argument_group` to accept an additional keyword argument: `gooey_options`. It accepts two keys: `show_border` and `columns` @abstr_code_section @abstr_image `show_border` is nice for visually tying together closely related items within a parent group. Setting it to `true` will draw a small border around all of the inputs and nest the title at the top. `columns` controls how many many items get places on each row within the Run Modes \--------- Gooey has a handful of presentation modes so you can tailor its layout to your content type and user's level or experience. ### Advanced The default view is the "full" or "advanced" configuration screen. It has two different layouts depending on the type of command line interface it's wrapping. For most applications, the flat layout will be the one to go with, as its layout matches best to the familiar CLI schema of a primary command followed by many options (e.g. Curl, FFMPEG). On the other side is the Column Layout. This one is best suited for CLIs that have multiple paths or are made up of multiple little tools each with their own arguments and options (think: git). It displays the primary paths along the left column, and their corresponding arguments in the right. This is a great way to package a lot of varied functionality into a single app. 

@abstr_image 

Both views present each action in the `Argument Parser` as a unique GUI component. It makes it ideal for presenting the program to users which are unfamiliar with command line options and/or Console Programs in general. Help messages are displayed along side each component to make it as clear as possible which each widget does.

**Setting the layout style:**

Currently, the layouts can't be explicitely specified via a parameter (on the TODO!). The layouts are built depending on whether or not there are `subparsers` used in your code base. So, if you want to trigger the `Column Layout`, you'll need to add a `subparser` to your `argparse` code. 

It can be toggled via the `advanced` parameter in the `Gooey` decorator. 
    
    
    @gooey(advanced=True)
    def main():
        # rest of code
    

* * *

### Basic

The basic view is best for times when the user is familiar with Console Applications, but you still want to present something a little more polished than a simple terminal. The basic display is accessed by setting the `advanced` parameter in the `gooey` decorator to `False`. 
    
    
    @gooey(advanced=False)
    def main():
        # rest of code
    

@abstr_image 

* * *

### No Config

No Config pretty much does what you'd expect: it doesn't show a configuration screen. It hops right to the `display` section and begins execution of the host program. This is the one for improving the appearance of little one-off scripts. 

@abstr_image 

* * *

### Input Validation

@abstr_image 

> :warning: Note! This functionality is experimental. Its API may be changed or removed alltogether. Feedback/thoughts on this feature is welcome and encouraged! 

Gooey can optionally do some basic pre-flight validation on user input. Internally, it uses these validator functions to check for the presence of required arguments. However, by using GooeyParser, you can extend these functions with your own validation rules. This allows Gooey to show much, much more user friendly feedback before it hands control off to your program. 

**Writing a validator:**

Validators are specified as part of the `gooey_options` map available to `GooeyParser`. It's a simple map structure made up of a root key named `validator` and two internal pairs: 

  * `test` The inner body of the validation test you wish to perform 
  * `message` the error message that should display given a validation failure



e.g.

@abstr_code_section 

**The`test` function**

Your test function can be made up of any valid Python expression. It receives the variable `user_input` as an argument against which to perform its validation. Note that all values coming from Gooey are in the form of a string, so you'll have to cast as needed in order to perform your validation. 

**Full Code Example**

@abstr_code_section 

@abstr_image 

With the validator in place, Gooey can present the error messages next to the relevant input field if any validators fail. 

* * *

## Using Dynamic Values

> :warning: Note! This functionality is experimental. Its API may be changed or removed alltogether. Feedback on this feature is welcome and encouraged! 

Gooey's Choice style fields (Dropdown, Listbox) can be fed a dynamic set of values at runtime by enabling the `poll_external_updates` option. This will cause Gooey to request updated values from your program everytime the user visits the Configuration page. This can be used to, for instance, show the result of a previous execution on the config screen without requiring that the user restart the program. 

**How does it work?**

@abstr_image 

At runtime, whenever the user hits the Configuration screen, Gooey will call your program with a single CLI argument: `gooey-seed-ui`. This is a request to your program for updated values for the UI. In response to this, on `stdout`, your program should return a JSON string mapping cli-inputs to a list of options.

For example, assuming a setup where you have a dropdown that lists user files:

@abstr_code_section 

Here the input we want to populate is `--load`. So, in response to the `gooey-seed-ui` request, you would return a JSON string with `--load` as the key, and a list of strings that you'd like to display to the user as the value. e.g. 

@abstr_code_section 

Checkout the full example code in the @abstr_hyperlink . Or checkout a larger example in the silly little tool that spawned this feature: @abstr_hyperlink . 

* * *

## Showing Progress

@abstr_image 

Giving visual progress feedback with Gooey is easy! If you're already displaying textual progress updates, you can tell Gooey to hook into that existing output in order to power its Progress Bar. 

For simple cases, output strings which resolve to a numeric representation of the completion percentage (e.g. `Progress @abstr_number %`) can be pattern matched and turned into a progress bar status with a simple regular expression (e.g. `@Gooey(progress_regex=r"^progress: (\d+)%$")`). 

For more complicated outputs, you can pass in a custom evaluation expression (`progress_expr`) to transform the things however you need. 

**Program Output:**

@abstr_code_section 

**Regex and Processing Expression**

@abstr_code_section 

There are lots of options for telling Gooey about progress as your program is running. Checkout the @abstr_hyperlink repository for more detailed usage and examples! 

| progress_regex | A text regex used to pattern match runtime progress information. See: Showing Progress for a detailed how-to | | progress_expr | A python expression applied to any matches found via the `progress_regex`. See: Showing Progress for a detailed how-to |

* * *

## Customizing Icons

Gooey comes with a set of six default icons. These can be overridden with your own custom images/icons by telling Gooey to search additional directories when initializing. This is done via the `image_dir` argument to the `Goeey` decorator. 
    
    
    @Gooey(program_name='Custom icon demo', image_dir='/path/to/my/image/directory')
    def main():
        # rest of program
    

Images are discovered by Gooey based on their _filenames_. So, for example, in order to supply a custom configuration icon, simply place an image with the filename `config_icon.png` in your images directory. These are the filenames which can be overridden:

  * program_icon.ico
  * success_icon.png
  * running_icon.png
  * loading_icon.gif
  * config_icon.png
  * error_icon.png



## Packaging

Thanks to some @abstr_hyperlink , packaging Gooey as an executable is super easy. 

The tl;dr @abstr_hyperlink version is to drop this @abstr_hyperlink into the root directory of your application. Edit its contents so that the `application` and `name` are relevant to your project, then execute `pyinstaller build.spec` to bundle your app into a ready-to-go executable. 

Detailed step by step instructions can be found @abstr_hyperlink . 

## Screenshots

| Flat Layout | Column Layout |Success Screen | Error Screen | Warning Dialog | |-------------|---------------|---------------|--------------|----------------| | @abstr_image | @abstr_image | @abstr_image | @abstr_image | @abstr_image | 

| Custom Groups | Tabbed Groups | Tabbed Navigation | Sidebar Navigation | Input Validation | |-------------|---------------|---------------|--------------|----------------| | @abstr_image | @abstr_image | @abstr_image | @abstr_image | @abstr_image | 

* * *

## Wanna help?

Code, translation, graphics? Pull requests are welcome.

# Behat Contexts


Contexts that we use with Behat 3.x tests on Drupal sites.

## Install

This contexts use the Drupal Extension [Drupal Extension](https://www.drupal.org/project/drupalextension), so it should be installed in your system.

Put this repository in the bootstrap directory of your project. A Drupal 7 project using Drupal Extension this directory may be located in sites/all/tests/behat/bootstrap. So, directory Metadrop of this repository should be in that bootstrap directory. Contexts will be autoladed.

@TODO: Improve installation method, use some standard way.

## Configure

Each context may have its own configuration, see each context section.

## Contexts

### DebugContext

Simple context to help debugging tests with some steps. Additionally, it hooks in the after step event to add a step that generates an error report on failed steps.

This report includes:
  - A file with the HTML page content.
  - A file with the current URL and the error exception dump.
  - If available, a file with current page state.


#### Steps

- Then capture full page with a width of :width

  Saves a screenshot of current page with the given width to a file.

- Then capture full page with a width of :width with name :filename in configured directory (screenshots_path).

  Saves a screenshot of current page with the given width to a given filename in configured directory (screenshots_path).

- Then capture full page with width of :width to :path

  Saves a screenshot of current page with the given width to a file in the given path. If path is relative screenshots_path config value is used as root.

- Then capture full page with width of :width to :path with name :filename

  Saves a screenshot of current page with the given width to a file in the given path to a given filename. If path is relative screenshots_path config value is used as root.

- Then save last response

  Saves page content to a file.

- Then save last response to :path

  Saves page content to a file in the given path.

- Then save last response to :path

  Halts test for a given amount of seconds. Useful when debugging tests with timing issues. Don't use this step in real tests.


#### Configuration
  Add DebugContext to your suite.

  This is an example when bootstrap directorty is in DRUPALROOT/sites/all/tests/behat/bootstrap.

```
default:
  autoload:
    '': "%paths.base%/../all/tests/behat/bootstrap"
  suites:
    default:
      paths:
        - "%paths.base%/../all/tests/behat/features"
      contexts:
        - Metadrop\Behat\Context\DebugContext:
            parameters:
              'report_on_error': true
              'error_reporting_path': '/var/error_reports/tests/reports'
              'screenshots_path': '/var/error_reports/tests/screenshots'
              'page_contents_path': '/var/error_reports/tests/pages'
```

**Parameters**
  - report_on_error: If _true_ error reports are generated on failed steps.
  - error_reporting_path: Path where reports are saved.
  - screenshots_path: Path where screenshots are saved. Report screenshots are saved in the report path, here only screenshots from _capture full page_ steps are saved.
  - page_contents_path: Path where page contents are saved. Report page contents are saved in the report path, here only page contents from _save page content_ steps are saved.



### BrowserSizeContext

This contexts allows to resize the browser to a given set of sizes. It should be used with a real browser driver like Selenium. Its main purpose is to ease tests that depends on window size.

Keep in mind  that browser window size may not change between scenarios or features, even differnet test execution. For example, if you use PhantomJS the same browser is used for all tests execution (given that PhantomJS is not terminated and executed again). So if a test changes the window size next tests with the @javadcript tag will be performed with that window size.


#### Steps

- Given (that )browser window size is :size size
  Changes window broeser size to the given size. The size must be one of the default ones or one of the sizes declared in the configuration.


#### Configuration

Add BrowserSizeContext to your suite.

To declare sizes and make them available use the context params:

- Metadrop\Behat\Context\BrowserSizeContext:
    parameters:
      Default:
        width: 1200
        height: 800
      Full:
        width: 1200
        height: 800
      My custom size:
        width: 1440
        height: 960


The context has some default values. If a size is defined with same name as one of the default sizes the dimensions are overwritten. If a completely new size is defined is simply added to the available size.



### DrupalExtendedContext

  This context extends DrupalRawContext with steps related to Drupal and its modules.


#### Steps

- Then form :type element :label should be required

  Checks a form element is required. File input type is not supported.

- Then form :type element :label should not be required

  Checks a form element is required. File input type is not supported.

- Given I run elysia cron

  Runs Elysia cron.


#### Configuration

No configuration needed.



### IUContext

  This context provides steps for certain UI elements.


#### Steps

- Given I select :option from :select chosen.js select box

  Selects and option from a Chosen select widget. Only for sinlge selection, it
  doesn't work with multiple selection enabled or tag style.

  See https://harvesthq.github.io/chosen/


#### Configuration

No configuration needed.

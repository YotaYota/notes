# Maven

Maven follows some conventions:
```
- src/main/java
- src/main/resources
- src/main/webapp
- src/test/java
- src/test/resources
- target
```
If theese conventions are followed Maven builds automatically
```sh
$ mvn clean install
```
___

# Jenkins Plugin
## pom.xml
Maven uses it for building your plugin. All Jenkins plugins should be based on the Plugin Parent POM:
```
<parent>
    <groupId>org.jenkins-ci.plugins</groupId>
    <artifactId>plugin</artifactId>
    <version>2.2</version>
</parent>
```
```
<properties>
    <jenkins.version>1.609.1</jenkins.version>
</properties>
```
# src/main/java
Java source files of the plugin.
# src/main/resources
Jelly/Groovy views of the plugin.
## src/main/webapp
Static resources of the plugin, such as images and HTML files.
# src/test/java

# src/test/resources
# target

# Extension approach
A Plugin class is optional; a plugin may simply implement extension points, registering them with the `@hudson.Extension` annotation for automatic detection by Jenkins.

- A plugin extension should extend an existing extension point (a class that implements `ExtensionPoint`) and define an inner static class extending the corresponding descriptor (a class extending `hudson.model.Descriptor`).
- The `@Extension` annotation must be placed on the inner descriptor class to let Jenkins know about this extension.
- The descriptor handles the global configuration of the extension while the extension class itself handles the individual configuration of the extension. For instance in a plugin defining a class extending LabelAtomProperty, an object of this class is instantiated for each LabelAtom (provided that the plugin is activated by the user in the label's configuration page). If configuration parameters for each individual instance are required, they're handled via a `config.jelly` file stored in a resource package named after the extension class. When the configuration form is saved, Jenkins calls the extension constructor marked with the `@org.kohsuke.stapler.DataBoundConstructor` annotation, matching parameters by name.

## Plugin (Old?)
A plugin extends `hudson.Plugin`.

```python
public class PluginImpl extends Plugin {
  private final static Logger LOG = Logger.getLogger(PluginImpl.class.getName());

  public void start() throws Exception {
    LOG.info("starting ansicolor plugin");
  }
}



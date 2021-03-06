## PropertiesFactory 
           
JNDI-Properties provides two simple JNDI ObjectFactory classes that enable you to bind a properties file as a Properties object into the jndi tree of JBoss AS 7.
These Properties objects can easily be accessed via the `@Resource` annotation.

    @Resource(mappedName="java:/app-config") 
    private java.util.Properties properties;

You can choose between `PropertiesFileFactory` or `PropertiesClasspathFactory` class. 
`PropertiesFileFactory` loads the properties from the filesystem via an absolute or relative path. The `PropertiesClasspathFactory` class loads the properties from classpath.     


### JBoss AS 7 Setup:
                                                    
For setting up a JBoss AS7 module put into the modules folder a subfolder structure like `de/crowdcode/jndi/properties/main`. And in the main folder place the `jndi-properties.jar` and a `module.xml` file. You can also place your properties files in here too. 

The `module.xml` should look like this:

    <?xml version="1.0" encoding="UTF-8"?>
    <module xmlns="urn:jboss:module:1.1" name="de.crowdcode.jndi.properties">
	    <resources>
		    <resource-root path="."/>
		    <resource-root path="jndi-properties.jar"/>
	    </resources>  
	    <dependencies>
	  	    <module name="javax.api" />
        </dependencies>
    </module>

#### Configuring of `PropertyFileFactory`

Define the JNDI Binding in the `standalone.xml`:

    <subsystem xmlns="urn:jboss:domain:naming:1.1">
        <bindings>
            <object-factory 
                name="java:/app-config" 
                module="de.crowdcode.jndi.properties" class="de.crowdcode.jndi.properties.PropertiesFileFactory"/>
        </bindings>
    </subsystem>                                                                                                                                              

And as JBoss AS 7.1.1 doesn't support configuration of ObjectFactories, you need to define a system property with the jndi name as key name.

    <system-properties>
        <property name="java:/app-config" value="/absolute/path/to/the/file/application.properties"/>
    </system-properties>

##### Configuring of `PropertyClasspathFactory`

    <subsystem xmlns="urn:jboss:domain:naming:1.1">
        <bindings>
            <object-factory 
                name="java:/app-config" 
                module="de.crowdcode.jndi.properties" class="de.crowdcode.jndi.properties.PropertiesClasspathFactory"/>
        </bindings>
    </subsystem>                                                                                                                                              
	
Add your properties files into the de/crowdcode/jndi/properties folder. For instance a app-config.properties file. The `PropertyClasspathFactory` provides two options to define the properties file. First, the jndi name ends with the properties filename. So the jndi name `java:/app-config` leads to `app-config.properties`. Second, you define a system property `java:/app-config` that contain the properties file name.  

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/">
	<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" />
</a>
<br />   

<subsystem xmlns="urn:jboss:domain:naming:1.1">
    <bindings>
        <object-factory name="java:/property-config" module="de.crowdcode.jndi.properties" class="de.crowdcode.jndi.properties.PropertiesFactory"/>
    </bindings>
</subsystem>

<span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">
	jndi-properties-factory
</span> 
by 
<span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName">Ingo Düppe</span> 
is licensed under the 
<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/">Creative Commons Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)</a>.

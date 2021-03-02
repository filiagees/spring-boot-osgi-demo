# Spring Boot OSGi Demo

This is demo application that shows how to use OSGi with Spring Boot.

### Build Application
Execute below maven command, which will create a bundle in `target/deploy` directory and can be deployed in Apache Karaf Container. 

```
mvn clean install
```

### calc-core
Here, calc-core bundle is providing service of Calculator and itself implementing Addition Service. 

After deploying only calc-core bundle if we call below Rest API.

```
http://localhost:8013/calculator
```
Then, its response would be like

```
["Addition"]
```

### calc-plugin
Here, calc-plugin is implementing Calculator Service and providing support for Subtraction also. This plugin is dependent on calc-core plugin.

After deploying calc-core, if we again call same Rest API.

```
http://localhost:8013/calculator
```
Then, its response would be like

```
["Addition","Subtraction"]
```
To perform calculator operation, call below RestAPI.

```
http://localhost:8013/calculator/{operation}?n1={n1}&n2={n2}
```

For Example,

```
http://localhost:8013/calculator/addition?n1=12&n2=12
```

---

#### Possible errors
##### sun tools with 'ExceptionInInitializerError'
If you get some error similar to:
`Fatal error compiling: java.lang.ExceptionInInitializerError: com.sun.tools.javac.code.TypeTags`
Please, add the latest version of lombok in pom.xml

More details, see [this github issue](https://github.com/rzwitserloot/lombok/issues/1651).


##### karaf osgi.component
If you get some errors similar to:
```
osgi.extender; (&(osgi.extender=osgi.component)(version>=1.3.0)(!(version>=2.0.0)))]
```
Please, try to install it with `feature:install scr`

See more details [here](https://stackoverflow.com/questions/40790543/unresolved-requirement-osgi-component).

##### karaf
If in karaf.log you found a error similar to:
```
Cannot start
org.osgi.framework.BundleException: Unable to resolve com.finalhints.calc-plugin [77](R 77.0): missing requirement [com.finalhints.calc-plugin [77](R 77.0)] osgi.ee; (&(osgi.ee=JavaSE)(version=9)) Unresolved requirements: [[com.finalhints.calc-plugin [77](R 77.0)] osgi.ee; (&(osgi.ee=JavaSE)(version=9))]
```

Please, try to set `<_noee>true</_noee>` in plugins configuration of pom.xml:

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.felix</groupId>
				<artifactId>maven-bundle-plugin</artifactId>
				...
				<configuration>
					<instructions>
						....
						<_noee>true</_noee>

More details [here](http://karaf.922171.n3.nabble.com/missing-requirement-osgi-ee-and-JDK-11-td4054662.html).

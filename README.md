# sentry-jboss-EAP7.x-module
This is a custom module to be installed in JBoss EAP 7.x so that your application server is capable of sending log events to the Sentry.io log sink.
You could also install this in any other JBoss AS or Wildfly server as long as the latter adheres to the namespace "urn:jboss:module:1.5".

## Instructions
1. Add the whole path `modules/system/layers/base/io/sentry/main/` from this repository to your JBoss home directory.
2. Add the custom handler tags below to your `standalone.xml` or `domain.xml` file(s)
```xml
<custom-handler name="SENTRY" class="io.sentry.jul.SentryHandler" module="io.sentry">  
	<level name="WARN"/>  
	<formatter>  
		<named-formatter name="DEFAULTPATTERN"/>
	</formatter>
</custom-handler>
...
<formatter name="DEFAULTPATTERN">
	<pattern-formatter pattern="%d{HH:mm:ss,SSS} %-5p [%c] (%t) %s%E%n"/>
</formatter>
...
<root-logger>
	<level name="INFO"/>
	<handlers>
		<handler name="SENTRY"/>
		...
	</handlers>
</root-logger>
```
3. Either set your DSN in a `SENTRY_DSN` environment variable or as a VM argument like so `-Dsentry.dsn=http://...`
5. Done!

## Updating sentry library
If you wish to use any other version than 1.7.22 of the `sentry.jar`, then you can easily replace this line
`<resource-root path="sentry-1.7.22.jar" />` with `<resource-root path="sentry-<your version number>.jar" />` in `module.xml`.
Make sure you also save the new `sentry-<your version number>.jar` in the same location as the `module.xml` file!


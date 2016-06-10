# Cookie Processor Fix for Java-Buildpack Tomcat

If you are using the Tomcat packaged with the Java Buildpack in Cloud Foundry (i.e. in .war mode),
cookies sent to the server whose values contain Unicode characters can cause the server to crash.
This can be fixed with code from this repository.

## Building

```
git clone https://github.com/jghiloni/cookie-processor-unicode-config.git
cd cookie-processor-unicode-config
tar czf config.tgz tomcat/
```

Upload `config.tgz` somewhere, then update `index.yml` to point to that URL. Upload `index.yml` and take note of its URL.

## Using with the Online Buildpack

```
cf set-env APP_NAME JBP_CONFIG_TOMCAT \
  '{ tomcat : { external_configuration_enabled: true }, external_configuration: { repository_root: "http://BASE_URL_OF_INDEX_YML", version: "1.0.0" } }'

cf restage APP_NAME
```

## Using the Offline Buildpack
Update `config/tomcat.yml` to the following:
```
---
tomcat:
  external_configuration_enabled: true
  external_configuration: 
    version: 1.0.0
    repository_root: http://BASE_URL_OF_INDEX_YML
```

And build the buildpack as normal

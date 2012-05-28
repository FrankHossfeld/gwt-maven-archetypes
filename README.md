gwt-maven-archetypes
====================

This project contains Maven archetypes for modular GWT projects.

How to use
----------

In order to generate a modular GWT project from the available archetypes, first
you need to clone and install this project locally:

    git clone https://github.com/tbroyer/gwt-maven-archetypes.git
    cd gwt-maven-archetypes && maven clean install

### Generate a project

    mvn archetype:generate \
       -DarchetypeGroupId=net.ltgt.gwt.archetypes \
       -DarchetypeArtifactId=<artifactId> \
       -DarchetypeVersion=1.0-SNAPSHOT

where the available `<artifactIds>` are:

* `modular-webapp`
* `modular-requestfactory`
*  guice-rf-activities

### Start the development mode

Change directory to your generated project and issue the following commands:

1. `mvn clean install -Dgwt.compiler.skip`
2. In one terminal window: `cd server && mvn jetty:start`
3. In another terminal window: `cd client && mvn gwt:run -Ddev`

### Profiles

There are two profiles defined in the POM file of client module:

* `dev` is to speed-up development with `gwt:run` and `jetty:start` by not
  requiring a restart when a change to `shared` is made
* `draft` is to speed-up GWT compilation, and will only compile (by default)
  for WebKit-based browsers (Safari, Chrome)

To activate `dev` or `draft` you have to provide the `-Ddev` or `-Ddraft`
system properties respectively.

### Productivity tips

When working on the server-side code exclusively, you don't need GWT's DevMode.
You can then compile the GWT app using `mvn package` or `mvn package -Ddraft`
and then `cd server && mvn jetty:start`. The webapp will be redeployed
automatically when you change a class (either compiled by your IDE, or by `mvn
compile`) in either the `server` or `shared` module (be careful though when
changing classes in `shared` that you do not break the GWT client code,
particularly when using GWT-RPC).

When working on the client-side code exclusively, to quickly test it in a
browser in production mode, use `mvn package -Ddraft`. You can use `mvn package
-Ddraft -pl client -am` while the Jetty server is running (launched by `cd
server && mvn jetty:start`), and then simply hit `F5` in your browser. Make
sure you use a browser compatible with the one declared in the
`client/src/main/java/${package}/${module}_dev.gwt.xml` file (by default,
Safari or Chrome).


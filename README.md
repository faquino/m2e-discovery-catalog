# M2E Discovery Catalog

The M2E discovery catalog is a document that describes an association between specific Maven plugins and M2E connectors that cooperate with those Maven plugins to make integration between Maven on the CLI and Maven in Eclipse smoother. An example of an M2E connector is the the [Tycho Configurator][2] which tries to bridge the gap between [Maven Tycho][3] on the CLI and PDE within Eclipse. The discovery catalog attempts to assist users when they importing Maven projects by suggesting connectors that are relevant to the projects they are importing. If, for example, you import a Maven project using Tycho into your Eclipse workspace, the Tycho Configurator will be offered to you in order meld Maven with PDE.

As a normal Maven user in Eclipse there is nothing special you have to do. If you import a project type that has available connectors, they will be offered to you. The sections below are for those who want to add entries to the discovery catalog. You may be a developer of Maven plugins and have additionally created a M2E connector, or you might be adding an entry for some existing Eclipse integration that is known to work in conjunction with existing Maven plugins.

# Testing the M2E Discovery Catalog as a User

If you know that one of the connectors you use has been updated in the staging catalog then you can try the new connector by specifying the staged catalog during the startup of Eclipse. by updating the eclipse.ini file in your Eclipse installation:

```
-vmargs
...
...
-Dm2e.discovery.url=http://download.eclipse.org/technology/m2e/discovery/directory-test-1.5.xml
```

# Using local M2E Discovery Catalog

In case your IDE can't access the default M2E Catalog you could just do `mvn clean package` and copy the resulting `target` contents into a local directory, then update your eclipse.ini file accordingly.

```
-vmargs
...
...
-Dm2e.discovery.url=file:<path_to_local_copy>/classes/catalog-1.13.xml
```

You'll also need to update the generated catalog-xyz.xml. The entry[url] attribute should match your local copy's actual location.

```
<directory xmlns="http://www.eclipse.org/mylyn/discovery/directory/">
  <entry url="file:<path_to_local_copy>/org.eclipse.m2e.discovery.oss-catalog-1.13.jar"
         permitCategories="true" />
</directory>
```


# Adding New Discovery Catalog Entries 

Adding new entries to the discovery catalog is a matter of creating a pull request with your new, well tested, entry. The process consists of making the appropriate edits to the [connectors document][1], building the catalog, and testing the that the connector is available for  your target project types and the import process works correctly.

While we would like to fully streamline the process of testing and pushing connectors into the publicly available catalog, this is what we have right now. As a developer of a connector, with the following instructions, you should be able to build the catalog locally with your changes and test that your connector is available from a publicly accessible site, will install, and works as expected.

## New Connector Addition Process

### Update the catalog with your entry

Clone the Git repository containing the catalog entries and publishing mechanism:

```
git clone git@github.com:faquino/m2e-discovery-catalog.git
```

Edit the connectors.xml file with your catalog entry which is [here][1].

### Build the catalog

In the top level of the Git repository execute the following command:

```
mvn clean package
```

This will create the catalog and validate that all the update sites specified are accessible. This will also produce two catalog files that you can use for testing locally with only the file system.

### Test your connector with Eclipse

Specify your newly built catalog by updating the eclipse.ini file in your Eclipse installation:

```
-vmargs
...
...
-Dm2e.discovery.url=file:/path/to/m2e-discovery-catalog/org.eclipse.m2e.discovery.oss/target/classes/catalog-1.5.xml
```

Once you have tested your connector and feel that it's working as intended then submit a pull request and we will merge your catalog entry and publish a new catalog.

[1]: https://github.com/faquino/m2e-discovery-catalog/blob/master/org.eclipse.m2e.discovery.oss/src/main/resources-filtered/connectors.xml
[2]: https://github.com/tesla/m2eclipse-tycho
[3]: http://www.eclipse.org/tycho/

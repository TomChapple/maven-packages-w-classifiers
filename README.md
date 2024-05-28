# Maven Packages w Classifier

> [!NOTE]
> The issue this project showcased has since been resolved. Classifiers
> and extensions are correctly rendered in the `maven-metadata.xml`
> file, allowing them to be discovered by Maven build processes without
> the need for workarounds.
>
> This has been archived for preservation purposes, but otherwise can be
> disregarded.

This project was created to showcase a bug in how GitHub produces the
`maven-metadata.xml` file in its Maven interface into GitHub Packages.

This project will automatically build three artifacts and deploy each of
these into its GitHub Package's destination. They are as follows:

| Artifact            | Extension | Classifier    |
| ------------------- | --------- | ------------- |
| Executable          | `jar`     | None          |
| Source Files        | `jar`     | `sources`     |
| Miscellaneous Files | `jar`     | `other-files` |

Once deployed, each of these artifacts are expected to be available in
the Maven interface under
`https://maven.pkg.github.com/TomChapple/maven-packages-w-classifiers`.
An additional file is also made available: `maven-metadata.xml`, which
describes each of the artifacts available for a specific combination of
Group ID, Artifact ID and Version (GAV). This is particularly useful for
snapshots as it allows for Maven to discover which specific artifact is
the latest snapshot. It will also inform whether artifacts of the
specified extension and classifier are available without needing to make
a query in the first place.

GitHub appears to generate this `maven-metadata.xml` file automatically.
**This automatic generation currently does not render the classifier and
extension correctly for a subset of artifacts, resulting in Maven being
unable to find these snapshot artifacts.** For example: the Miscellaneous
Files artifact is incorrectly listed with an extension and classifier of
`files.jar` and `other` respectively.

This bug appears to be limited to artifacts with hyphens in their
classifier. In this project, the Executable and Source Files can be
retrieved without issue.

This problem can be worked around with commands similar to the following:

```
mvn -U dependency:get -Dartifact=io.github.tomchapple:maven-packages-w-classifiers:1.0.0-20240521.011622-1:jar:other-files
mvn -U dependency:get -Dartifact=io.github.tomchapple:maven-packages-w-classifiers:1.0.0-SNAPSHOT:jar:other-files
mvn -U verify
```

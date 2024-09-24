# nuxeo-rest-api-swagger-doc

**The Nuxeo Swagger Rest API Documentation is out-of-date and no longer maintained.**

Please refer to https://github.com/nuxeo/doc.nuxeo.com-rest-api published to https://doc.nuxeo.com/rest-api/.

## Release the marketplace

Make sure the project builds and its tests pass.

Then create a temporary branch to perform the release:

```bash
git checkout -b tmp-release
```

Then update the project version to final, for instance 2:

```bash
mvn versions:set -DnewVersion=2021.1 -DgenerateBackupPoms=false
```

Then commit and tag the release:

```bash
git commit -a -m "Release 2021.1"
git tag -a -m "Release 2021.1" v2021.1
```

Then deploy the maven artifacts:

```bash
mvn clean deploy -DskipTests -DaltDeploymentRepository=maven-public-releases::default::PUBLIC_URL
```

> [!IMPORTANT]
> You should replace the `PUBLIC_URL`.
> Your Maven `settings.xml` file should contain appropriate authentication (if any) for the `maven-public-releases` repository.


And upload the marketplace to connect:

```
curl --fail -F package=@target/nuxeo-rest-api-doc-package-2021.0.zip 'https://connect.nuxeo.com/nuxeo/site/marketplace/upload?batch=true' -u ..
```

Then push the tag:

```bash
git push --tags
```

Then cleanup your branch and prepare the next development iteration:

```bash
git checkout 2021
git branch -D tmp-release
mvn versions:set -DnewVersion=2021.2-SNAPSHOT -DgenerateBackupPoms=false
git commit -a -m "Post release 2021.1"
git push
```

# Zip Slip

<img align="right" src="https://res.cloudinary.com/snyk/image/upload/f_auto,q_auto,c_thumb,h_150,w_150/v1527156415/research/zipslip.png">

Zip Slip is a widespread critical archive extraction vulnerability, allowing attackers to write arbitrary files on the system, typically resulting in remote command execution. It was discovered and responsibly disclosed by the Snyk Security team ahead of a public disclosure on 5th June 2018, and affects thousands of projects, including ones from HP, Amazon, Apache, Pivotal and many more. This page provides the most up-to-date fix statuses for the libraries and projects that were found to be exploitable or contain a vulnerable implementation.

For more information on the technical details of Zip Slip, read [http://snyk.io/research/zip-slip-vulnerability](http://snyk.io/research/zip-slip-vulnerability).

The vulnerability has been found in multiple ecosystems, including JavaScript, Ruby, .NET and Go, but is especially prevalent in Java, where there is no central library offering high level processing of archive (e.g. `zip`) files. The lack of such a library led to vulnerable code snippets being hand-crafted and shared among developer communities such as [StackOverflow](https://stackoverflow.com/questions/981578/how-to-unzip-files-recursively-in-java).

The vulnerability is exploited using a specially crafted archive that holds directory traversal filenames (e.g. `../../evil.sh`). The Zip Slip vulnerability can affect numerous archive formats, including `tar`, `jar`, `war`, `cpio`, `apk`, `rar` and `7z`.


Here is a vulnerable code example showing a `ZipEntry` path being concatenated to a destination directory without any path validation. Code similar to this has been found in many repositories across many ecosystems, including libraries which thousands of applications depend on.

```java
   Enumeration<ZipEntry> entries = zip.getEntries();
   while (entries.hasMoreElements()) {
      ZipEntry e = entries.nextElement();
      File f = new File(destinationDir, e.getName());
      InputStream input = zip.getInputStream(e);
      IOUtils.copy(input, write(f));
   }
```

If you find a library or project that contains similar vulnerable code, we ask for your contribution to this repository to provide the community with the most up to date information about the Zip Slip vulnerability. To contribute, please refer to our [CONTRIBUTING.md](https://github.com/snyk/zip-slip-vulnerability/blob/master/CONTRIBUTING.md) file.

## Affected Libraries

Many of the following affected libraries exist because their ecosystems lack high level APIs providing the basic archive management capabilities. This results in vulnerable code being shared and reused. The following table contains the list of vulnerable libraries we found during private disclosure of Zip Slip which we aim to keep up to date, with community support, going forward as more vulnerable libraries are discovered. Some libraries that do not provide the high-level API often result in vulnerable implementations also, either through people copying and pasting vulnerable private code, or writing their own vulnerable snippets.

| Vendor         | Product                                                                                                                | Language   | Confirmed vulnerable | Fixed Version                                                                            | CVE              | Fixed                                                                                                                                                                 |
|----------------|------------------------------------------------------------------------------------------------------------------------|------------|----------------------|------------------------------------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| npm library    | [unzipper](https://github.com/ZJONSSON/node-unzipper)                                                                  | JavaScript | YES                  | 0.8.13                                                                                   | [CVE-2018-1002203](https://snyk.io/vuln/npm:unzipper:20180415) | [17/4/2018](https://github.com/ZJONSSON/node-unzipper/pull/59)                                                                                                        |
| npm library    | [adm-zip](https://github.com/cthackers/adm-zip)                                                                        | JavaScript | YES                  | 0.4.9                                                                                    | [CVE-2018-1002204](https://snyk.io/vuln/npm:adm-zip:20180415) | [23/4/2018](https://github.com/cthackers/adm-zip/pull/212)                                                                                                            |
| Java library   | [codehaus/plexus-archiver](https://github.com/codehaus-plexus/plexus-archiver)                                         | Java       | YES                  | 3.6.0                                                                                    | [CVE-2018-1002200](https://snyk.io/vuln/SNYK-JAVA-ORGCODEHAUSPLEXUS-31680) | [6/5/2018](https://github.com/codehaus-plexus/plexus-archiver/pull/87)                                                                                                |
| Java library   | [zeroturnaround/zt-zip](https://github.com/zeroturnaround/zt-zip)                                                      | Java       | YES                  | 1.13                                                                                     | [CVE-2018-1002201](https://snyk.io/vuln/SNYK-JAVA-ORGZEROTURNAROUND-31681) | [26/4/2018](https://github.com/zeroturnaround/zt-zip/blob/master/src/main/java/org/zeroturnaround/zip/ZipUtil.java#L389:26)                                           |
| Java library   | [zip4j](http://www.lingala.net/zip4j/)                                                                                 | Java       | YES                  | [1.3.3](http://www.lingala.net/zip4j/includes/downloadzip4j.php?option=sources&fmt=zip) | [CVE-2018-1002202](https://snyk.io/vuln/SNYK-JAVA-NETLINGALAZIP4J-31679) | 13/6/2018                                                                                                                                                             |
| .NET library   | [DotNetZip.Semverd](https://github.com/haf/DotNetZip.Semverd)                                                          | .NET       | YES                  | 1.11.0                                                                                   | [CVE-2018-1002205](https://snyk.io/vuln/SNYK-DOTNET-DOTNETZIP-60245) | [7/5/2018](https://github.com/haf/DotNetZip.Semverd/compare/master...shana:bugs/relative-paths?expand=1)                                                              |
| .NET library   | [SharpCompress](https://github.com/adamhathcock/sharpcompress)                                                         | .NET       | YES                  | 0.21.0                                                                                   | [CVE-2018-1002206](https://snyk.io/vuln/SNYK-DOTNET-SHARPCOMPRESS-60246) | [2/5/2018](https://github.com/adamhathcock/sharpcompress/blob/2a5494a804dd3d6f5bec1ec79a52d54ffce610f5/src/SharpCompress/Archives/IArchiveEntryExtensions.cs#L58-L67) |
| Go library   | [mholt/archiver](https://github.com/mholt/archiver)                                                                    | Go         | YES                  | e4ef56d4                                                                                 | [CVE-2018-1002207](https://snyk.io/vuln/SNYK-GOLANG-GITHUBCOMMHOLTARCHIVERCMDARCHIVER-50071) | [17/4/2018](https://github.com/mholt/archiver/pull/65)                                                                                                                |
| Oracle         | [java.util.zip](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/zip/package-summary.html)               | Java       | * No High Level API  | Documentation Fix                                                                        | N/A              |                                                                                                                                                                       |
| Apache         | [commons-compress](https://github.com/apache/commons-compress/)                                                        | Java       | * No High Level API  | Documentation Fix                                                                        | N/A              | [23/4/2018](https://github.com/apache/commons-compress/commit/97867f6fa3634c77dfafd76c89ecb1087f5cd1ae#diff-1d31ec0d64a29d487ff7377fd8d20cddR359)                     |
| .NET library   | [SharpZipLib](https://github.com/icsharpcode/SharpZipLib)                                                              | .NET       | YES                  | v1.0.0                                                                                   | [CVE-2018-1002208](https://snyk.io/vuln/SNYK-DOTNET-SHARPZIPLIB-60247) | [19/8/2018](https://github.com/icsharpcode/SharpZipLib/commit/5376c2daf1c0e0665398dee765af2047e43146ca) |
| Ruby gem       | [zip-ruby](https://bitbucket.org/winebarrel/zip-ruby/src/a0bceebd7bf031c8815a8359ba9befe6ead1bedc/zipruby/?at=default) | Ruby       | * No High Level API  |                                                                                          | N/A              |                                                                                                                                                                       |
| Ruby gem       | [rubyzip](https://github.com/rubyzip/rubyzip)      | Ruby       | [YES](https://github.com/rubyzip/rubyzip/issues/369)  |            | [CVE-2018-1000544](https://snyk.io/vuln/SNYK-RUBY-RUBYZIP-22039)         |                                                                                 |
| Ruby gem       | [zipruby](https://github.com/fjg/zipruby)                                                                              | Ruby       | * No High Level API  |                                                                                          | N/A              |                                                                                                                                                                       |
| Go library     | [archive](https://golang.org/pkg/archive/)                                                                             | Go         | * No High Level API  |                                                                                          | N/A              |                                                                                                                                                                       |
| Python library | [tarfile](https://docs.python.org/3/library/tarfile.html)                                                              | Python     | YES                  |                                                                                          | N/A              |                                                                                                                                                                       |
| C++/qt library | [quazip](https://github.com/stachenov/quazip/)                                                                         | C++        | YES                  | 0.7.6                                                                                    | CVE-2018-1002209              | [12/6/2018](https://github.com/stachenov/quazip/commit/5d2fc16a1976e5bf78d2927b012f67a2ae047a98)                                                                      |
| Clojure library| [Raynes/fs](https://github.com/Raynes/fs)                                                                              | Clojure    | YES                  | akvo/fs 20180618-134534.a44cdd5b                                                         | N/A              | [18/6/2018](https://github.com/akvo/fs/commit/894ea7d0ac4c49e356a3453405caab7a11650b3d)                                                                               |
| Go library | [cloudfoundry/archiver](https://github.com/cloudfoundry/archiver/)                                                                              | Go    | YES                  | [24/5/2018](https://github.com/cloudfoundry/archiver/commit/09b5706aa9367972c09144a450bb4523049ee840)                                                         | N/A              | [24/5/2018](https://github.com/cloudfoundry/archiver/commit/09b5706aa9367972c09144a450bb4523049ee840)                                                                               |




## Projects Affected and Fixed

The following list of projects contain vulnerable code. Please add to this list as you find projects that are vulnerable to Zip Slip, or if you have further information about a project fix status.

| Vendor              | Product                           | Fixed date                                                                                                                                               | Fixed version | CVE           | Vulnerable Code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apache Storm        | Storm                             | [2/5/2018](https://github.com/apache/storm/commit/1117a37b01a1058897a34e11ff5156e465efb692)                                                              | 1.1.3, 1.2.2         | CVE-2018-8008 | [#1](https://github.com/apache/storm/blob/master/storm-server/src/main/java/org/apache/storm/utils/ServerUtils.java#L389) [#2](https://github.com/apache/storm/blob/master/storm-server/src/main/java/org/apache/storm/utils/ServerUtils.java#L523) [#3](https://github.com/apache/storm/blob/master/storm-server/src/main/java/org/apache/storm/utils/ServerUtils.java#L592) [#4](https://github.com/apache/storm/blob/master/storm-server/src/main/java/org/apache/storm/utils/ServerUtils.java#L650) |
| Apache Software Foundation | Apache Hadoop                            | 30/5/2018 [#1](https://github.com/apache/hadoop/commit/745f203e577bacb35b042206db94615141fa5e6f) [#2](https://github.com/apache/hadoop/commit/e3236a9680709de7a95ffbc11b20e1bdc95a8605) | 2.7.7, 2.8.5, 2.9.2, 3.0.3, 3.1.1 | CVE-2018-8009 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Apache              | Maven                             |                                                                                                                                                          |               |               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Apache              | Ant                               | [21/4/2018](https://github.com/apache/ant/commit/e56e54565804991c62ec76dad385d2bdda8972a7#diff-32b057b8e95fa2b3f7d644552643010aR11)                      | 1.9.12        |   CVE-2018-10886  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Pivotal             | spring-integration-zip            | [3/5/2018](https://github.com/spring-projects/spring-integration-extensions/commit/a5573eb232ff85199ff9bb28993df715d9a19a25)                             | 1.0.1         | CVE-2018-1261 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Pivotal             | spring-integration-zip            | [10/5/2018](https://github.com/spring-projects/spring-integration-extensions/commit/d10f537283d90eabd28af57ac97f860a3913bf9b)                            | 1.0.2         | CVE-2018-1263 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| HP                  | Fortify Cloud Scan Jenkins Plugin | [27/4/2018](https://github.com/jenkinsci/fortify-cloudscan-plugin/commit/15a5270734280558f9356bd8681303b37f44f020#diff-443258e63dbf581491b1104125a59fd4) | [1.5.2](https://jenkins.io/security/advisory/2018-06-25/#SECURITY-870)     |               | [#1](https://github.com/jenkinsci/fortify-cloudscan-plugin/blob/cfa6d392abd900ce60a08bb830f99e821361b238/src/main/java/org/jenkinsci/plugins/fortifycloudscan/util/ArchiveUtil.java#L33:24)                                                                                                                                                                                                                                                                                                             |
| OWASP               | DependencyCheck                   | [7/5/2018](https://github.com/jeremylong/DependencyCheck/commit/c106ca919aa343b95cca0ffff0a0b5dc20b2baf7)                                                | [3.2.0](https://github.com/jeremylong/DependencyCheck/blob/master/RELEASE_NOTES.md#version-320-2018-05-21)         |  CVE-2018-12036  |                                                                                                                                                                                                                                                                                                                                                                                                    |
| Amazon              | AWS Toolkit for Eclipse           | [31/5/2018](https://github.com/aws/aws-toolkit-eclipse/commit/f2bd33e11299456979dc2092813a09e716f3d355)                                                  |               |               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| SonarQube           | [SonarQube](https://jira.sonarsource.com/browse/SONAR-10661)                         | [4/5/2018](https://github.com/SonarSource/sonarqube/commit/08438a2c47112f2fce1e512f6c843c908abed4c7#diff-6d8def68a00bf88a105528765f02fb95)               |   6.7.4 LTS, 7.2   |               | [#1](https://github.com/SonarSource/sonarqube/blob/c0d2705e610d771b8c66ef22e64530c7bca4f538/sonar-plugin-api/src/main/java/org/sonar/api/utils/ZipUtils.java#L148)                                                                                                                                                                                                                                                                                                                                      |
| Cinchapi            | Concourse                         | [30/5/2018](https://github.com/cinchapi/concourse/commit/9db7123029d103abaa909abd737876df7ace9957)                                                       |               |               | [#1](https://github.com/cinchapi/concourse/blob/a890d80a80298436995b42045474c6f01b53066b/concourse-driver-java/src/main/java/com/cinchapi/concourse/util/ZipFiles.java#L100)                                                                                                                                                                                                                                                                                                                            |
| Orient Technologies | OrientDB                          | [31/5/2018](https://github.com/orientechnologies/orientdb/commit/1dd754996682b5a7f467072b34747a33642d983b)                                               |               |               | [#1](https://github.com/orientechnologies/orientdb/blob/1c184f1295d1ce1538e5debac05addc7ca69b5b8/core/src/main/java/com/orientechnologies/orient/core/compression/impl/OZIPCompressionUtil.java#L87) [#2](https://github.com/orientechnologies/orientdb/blob/5684b63f6efb03d407d0175b9eab616b36bbecbd/etl/src/main/java/com/orientechnologies/orient/etl/util/OFileManager.java#L76)                                                                                                                    |
| FenixEdu            | Academic                          | [30/5/2018](https://github.com/FenixEdu/fenixedu-academic/commit/a64a568de3d3dd65338320239ecc7d6d94f3b36d)                                               |               |               | [#1](https://github.com/FenixEdu/fenixedu-academic/blob/674a7081d6a28cfadcae1cf732c11e9599cdedee/src/main/java/org/fenixedu/academic/util/FileUtils.java#L118)                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                 |
| Lucee            | Lucee                          | [5/6/2018](https://github.com/lucee/Lucee/commit/04a2d504ebe5472eddbe38c4333c0904bd8dc765)                                               |  5.2.7.63, 5.2.8.47    |               | [#1](https://github.com/lucee/Lucee/blob/ad2b44d9b6695e6ef8632eadf306c3f38e43885b/core/src/main/java/lucee/runtime/tag/Zip.java#L487)                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                 |
| groovy-common-extensions            | groovy-common-extensions                          | [3/7/2018](https://github.com/timyates/groovy-common-extensions/commit/ea5d3fb7b64edeac405d83193bfeac6dbcd1ad3f)                                               | [0.7.1](https://github.com/timyates/groovy-common-extensions/releases/tag/v0.7.1)    |               | [#1](https://github.com/timyates/groovy-common-extensions/blob/169fad28b6ec306979f06b5ec38cae4085bf05bd/src/main/groovy/com/bloidonia/groovy/extensions/FileExtensionMethods.groovy#L144)                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                 |
| fabric8            | fabric8                          | [5/6/2018](https://github.com/fabric8io/fabric8/commit/c7d4db1d2570579a7735b4d48a4380bc4b7152a5#diff-41610113b82d84309edcd091d69cd789)                                               | 2.2.170-85    |               | [#1](https://github.com/fabric8io/fabric8/blob/5d20ac54e81246b78dc343ff0504b815421f5704/components/fabric8-utils/src/main/java/io/fabric8/utils/Zips.java#L116)                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                 |
| Apache            | Tika                          | [19/9/2018](https://lists.apache.org/thread.html/ab2e1af38975f5fc462ba89b517971ef892ec3d06bee12ea2258895b@%3Cdev.tika.apache.org%3E)                                               | 1.19    |               |    
| Apache            | DeepLearning4J                          | [10/24/2018](https://github.com/deeplearning4j/deeplearning4j/pull/6630)                                           | 1.0.0-SNAPSHOT    |               | 
|                                                                                                                                                                                                                                 |



## Defensively fixed but deemed not exploitable

Some projects were confirmed by the project maintainers that their implementation code was not vulnerable to Zip Slip. However they decided to remove or fix their implementation so that in the future, the snippets could not be copied and become vulnerable elsewhere.

| Vendor          | Product            | Vulnerable Code Removed                                                                                                                      | Vulnerable Code                                                                                                                                                                                                                                                                                    |
|-----------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Apache          | Kylin              | 24/4/2018                                                                                                                                    | [#1](https://github.com/apache/kylin/blob/master/storage-hbase/src/main/java/org/apache/kylin/storage/hbase/util/TarGZUtil.java#L43)                                                                                                                                                                     |
| Apache          | NiFi               | 24/4/2018                                                                                                                                    | [#1](https://github.com/apache/nifi/blob/master/nifi-nar-bundles/nifi-standard-bundle/nifi-standard-processors/src/main/java/org/apache/nifi/processors/standard/UnpackContent.java#L312)                                                                                                                |
| Apache          | Geode              | [20/4/2018](https://github.com/apache/geode/commit/09ddd563fc2c2cbe605314bccc7b94f9db8b1de5)                                                 |                                                                                                                                                                                                                                                                                                    |
| Jenkins         | Jenkins CI         | [5/5/2018](https://github.com/jenkinsci/jenkins/pull/3402)                                                                                   | [#1](https://github.com/jenkinsci/jenkins/pull/3402)                                                                                                                                                                                                                                                     |
| Elastic         | ElasticSearch      | [10/5/2018](https://github.com/elastic/elasticsearch/commit/bf2365d13b2e26e45a47fbc4f53818a16579f80c#diff-7e3dde64df2d4f7290d743c8fb376213)  | [#1](https://github.com/elastic/elasticsearch/blob/ee802ad63c0f21d697a5095dd05dc6f94626ee4d/test/framework/src/main/java/org/elasticsearch/common/io/FileTestUtils.java#L68-L94)                                                                                                                         |
| LinkedIn        | Pinot              | [22/5/2018](https://github.com/linkedin/pinot/commit/07b0508f16f5e8e1bcd52963c82dbdf15ac9701e#diff-4591f6cee344066f126222283295f09b)         | [#1](https://github.com/linkedin/pinot/blob/master/pinot-common/src/main/java/com/linkedin/pinot/common/utils/TarGzCompressionUtils.java#L183)                                                                                                                                                           |
| AnkiDroid       | Anki-Droid         | [31/5/2018](https://github.com/ankidroid/Anki-Android/commit/7bfffab3982ad76efd06cf7b043d737be0d37f5f#diff-fad1c2723e695df741957168ca8a714f) | [#1](https://github.com/ankidroid/Anki-Android/blob/eb540c2fd3aa99a646242c887b9094223ba4a8a1/AnkiDroid/src/main/java/com/ichi2/libanki/Utils.java#L633)                                                                                                                                                  |
| ata4            | bspsrc             | [30/5/2018](https://github.com/ata4/bspsrc/commit/379b28237094841b6ede4dc7dacc4bb6d733f265)                                                  | [#1](https://github.com/ata4/bspsrc/blob/21e451142738463d999435d36de7353f48daaa15/src/main/java/info/ata4/bsplib/PakFile.java#L60-L89)                                                                                                                                                                   |
| eirslett            | frontend-maven-plugin             | [30/5/2018](https://github.com/eirslett/frontend-maven-plugin/commit/93d77ffc023effbcb36813648b578a0541709d76)                                                  | [#1](https://github.com/eirslett/frontend-maven-plugin/blob/ef103230692cbf00a5f86ab7b909246d6b638243/frontend-plugin-core/src/main/java/com/github/eirslett/maven/plugins/frontend/lib/ArchiveExtractor.java#L109) [#2](https://github.com/eirslett/frontend-maven-plugin/blob/ef103230692cbf00a5f86ab7b909246d6b638243/frontend-plugin-core/src/main/java/com/github/eirslett/maven/plugins/frontend/lib/ArchiveExtractor.java#L81)                                                                                                                                                                   |



## Deemed not exploitable by the maintainer (vulnerable implementation remains)

The final list of projects are those with snippets of code that still have a vulnerable implementation, but are not exploitable. It is believed that it would not be possible to attack these projects in such a way that could lead to a malicious outcome, but the vulnerable pattern of code still exists within the code base. We strongly encourage such projects to fix the implementation both to prevent its use through other functionality, or use in other projects that copy paste snippets.

| Vendor       | Product            | Vulnerable Code                                                                                                                                                                                                             |
|--------------|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JetBrains    | Intellij-community | [#1](https://github.com/JetBrains/intellij-community/blob/91fc0d0af2bf12a8faa8fac5296a92edf4ea268d/platform/util/src/com/intellij/util/io/TarUtil.java#L191)                                                                      |
| Apache       | Apex               | [#1](https://github.com/apache/apex-core/blob/master/engine/src/main/java/com/datatorrent/stram/client/AppPackage.java#L320)                                                                                                      |
| Apache       | Zeppelin           | [#1](https://github.com/apache/zeppelin/blob/master/zeppelin-zengine/src/main/java/org/apache/zeppelin/helium/HeliumBundleFactory.java#L225)                                                                                      |
| Apache       | Reef               | [#1](https://github.com/apache/reef/blob/561a336f2f0dda8f4a67a96179750a76167b038f/lang/java/reef-runtime-azbatch/src/main/java/org/apache/reef/runtime/azbatch/evaluator/EvaluatorShim.java#L295)                                 |
| Apache       | BookKeeper         | [#1](https://github.com/apache/bookkeeper/blob/6dda0a6c68fbaf2ca198cfbb693db4ac93a0feef/tests/integration-tests-utils/src/main/java/org/apache/bookkeeper/tests/DockerUtils.java#L109)                                            |
| Apache       | Pulsar             | [#1](https://github.com/apache/incubator-pulsar/blob/44e06635c1524229a923e8fbb525df278fcecdec/tests/integration-tests-utils/src/main/java/org/apache/pulsar/tests/DockerUtils.java)                                               |
| Apache       | Heron              | [#1](https://github.com/apache/incubator-heron/blob/master/heron/downloaders/src/java/org/apache/heron/downloader/Extractor.java#L43)                                                                                             |
| Apache       | Gobblin            | [#1](https://github.com/apache/incubator-gobblin/blob/4bdd0482e815013ee016ede4385a9ba339621f1b/gobblin-aws/src/main/java/org/apache/gobblin/aws/AWSJobConfigurationManager.java#L199)                                             |
| Apache       | Gobblin            | [#1](https://github.com/apache/incubator-gobblin/blob/5457af88d56b8fb89b172129fd1ff24ecdd4eba8/gobblin-data-management/src/main/java/org/apache/gobblin/data/management/copy/writer/TarArchiveInputStreamDataWriter.java#L81-L87) |
| Apache       | SystemML           | [#1](https://github.com/apache/systemml/blob/2e6b577c513393022f87e4770d7761a3726a07aa/dev/release/src/test/java/org/apache/sysml/validation/ValidateLicAndNotice.java#L485)                                                       |
| Gradle       | Gradle             | [#1](https://github.com/gradle/gradle/blob/de937ae7c46389169888aca2e7d9f506547e78bf/subprojects/wrapper/src/main/java/org/gradle/wrapper/Install.java#L230)                                                                       |
| Gradle       | Gradle             | [#1](https://github.com/gradle/gradle/blob/4bbd605e2339dab76e441d91ac9aa0f5af2518f7/subprojects/build-cache/src/jmh/java/org/gradle/caching/internal/tasks/ZipPacker.java#L54:25)                                                 |
| Gradle       | Gradle             | [#1](https://github.com/gradle/gradle/blob/f1efee61bcee87411f7b78761cbb492250e03b70/subprojects/core/src/main/java/org/gradle/api/internal/file/archive/ZipFileTree.java#L97)                                                     |
| plasma-umass | doppio             | [#1](https://github.com/plasma-umass/doppio/blob/f58deb051f097c66cadc1e48a236e670d2d2731d/classes/util/Unzip.java#L25)                                                                                                            |
| streamsets   | DataCollector      | [#1](https://github.com/streamsets/datacollector/blob/07c1dd23369ad55a30cd039d96751155a7dbfe8b/miniIT/src/test/java/com/streamsets/datacollector/util/UntarUtility.java#L61)                                                      |

For more information on Zip Slip, go to [http://snyk.io/research/zip-slip-vulnerability](http://snyk.io/research/zip-slip-vulnerability).

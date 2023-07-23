Analysis
========

Resolved `com.google.protobuf:protobuf-java:jar:3.21.12:runtime` (see `dependency:list` output) is causing a failure.

Version comes from dependencyManagement: see `[INFO] |  |  +- com.google.protobuf:protobuf-java:jar:3.21.12:runtime (version managed from 3.21.9)` from `dependency:tree -Dverbose` output

Imported from `protobuf-bom` (see `help:effective-pom -Dverbose` output)

```
      <dependency>
        <groupId>com.google.protobuf</groupId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 60 -->
        <artifactId>protobuf-java</artifactId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 61 -->
        <version>3.21.12</version>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 62 -->
      </dependency>
```

Lineage taken witht he help of https://issues.apache.org/jira/projects/MPH/issues/MPH-183: `dev.sigstore:sigstore-java:jar:0.4.0` imports `com.google.cloud:libraries-bom:pom:26.9.0` imports `com.google.cloud:first-party-dependencies:pom:3.3.0` imports `com.google.api:gapic-generator-java-bom:pom:2.15.1` imports `com.google.protobuf:protobuf-bom:3.21.12`

Conclusion: order of dependencyManagement in `sigstore-java` should be updated to put `protobuf-bom` earlier (hoping it won't break anything else)

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.google.cloud</groupId>
            <artifactId>libraries-bom</artifactId>
            <version>26.9.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>com.google.protobuf</groupId>
            <artifactId>protobuf-bom</artifactId>
            <version>3.22.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-bom</artifactId>
            <version>1.53.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>com.google.oauth-client</groupId>
            <artifactId>google-oauth-client-bom</artifactId>
            <version>1.34.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## `dependency:list`

```
$ mvn dependency:list
[INFO] Scanning for projects...
[INFO] Inspecting build with total of 1 modules...
[INFO] Installing Nexus Staging features:
[INFO]   ... total of 1 executions of maven-deploy-plugin replaced with nexus-staging-maven-plugin
[INFO] 
[INFO] -----------------< dev.sigstore:sigstore-maven-plugin >-----------------
[INFO] Building sigstore Maven Plugin 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] ----------------------------[ maven-plugin ]----------------------------
[INFO] 
[INFO] --- dependency:3.6.0:list (default-cli) @ sigstore-maven-plugin ---
[INFO] Can't extract module name from plexus-container-default-1.0-alpha-9-stable-1.jar: plexus.container.default: Invalid module name: 'default' is not a Java identifier
[INFO] 
[INFO] The following files have been resolved:
[INFO]    dev.sigstore:sigstore-java:jar:0.4.0:compile -- module sigstore.java (auto)
[INFO]    com.google.http-client:google-http-client-gson:jar:1.42.3:compile -- module com.google.api.client.json.gson [auto]
[INFO]    io.github.erdtman:java-json-canonicalization:jar:1.1:runtime -- module java.json.canonicalization (auto)
[INFO]    dev.sigstore:protobuf-specs:jar:0.1.0:runtime -- module protobuf.specs (auto)
[INFO]    com.google.protobuf:protobuf-java:jar:3.21.12:runtime -- module com.google.protobuf [auto]
[INFO]    com.google.api.grpc:proto-google-common-protos:jar:2.14.1:runtime -- module proto.google.common.protos (auto)
[INFO]    com.google.protobuf:protobuf-java-util:jar:3.21.12:runtime -- module com.google.protobuf.util [auto]
[INFO]    com.google.guava:guava:jar:31.1-jre:compile -- module com.google.common [auto]
[INFO]    com.google.guava:failureaccess:jar:1.0.1:compile -- module failureaccess (auto)
[INFO]    com.google.guava:listenablefuture:jar:9999.0-empty-to-avoid-conflict-with-guava:compile -- module listenablefuture (auto)
[INFO]    org.checkerframework:checker-qual:jar:3.12.0:compile -- module org.checkerframework.checker.qual [auto]
[INFO]    com.google.errorprone:error_prone_annotations:jar:2.5.1:compile -- module com.google.errorprone.annotations [auto]
[INFO]    com.google.j2objc:j2objc-annotations:jar:1.3:compile -- module j2objc.annotations (auto)
[INFO]    com.google.code.findbugs:jsr305:jar:3.0.2:compile -- module jsr305 (auto)
[INFO]    io.grpc:grpc-protobuf:jar:1.53.0:runtime -- module grpc.protobuf (auto)
[INFO]    io.grpc:grpc-api:jar:1.53.0:runtime -- module grpc.api (auto)
[INFO]    io.grpc:grpc-context:jar:1.53.0:compile -- module grpc.context (auto)
[INFO]    io.grpc:grpc-protobuf-lite:jar:1.53.0:runtime -- module grpc.protobuf.lite (auto)
[INFO]    io.grpc:grpc-stub:jar:1.53.0:runtime -- module grpc.stub (auto)
[INFO]    commons-codec:commons-codec:jar:1.15:compile -- module org.apache.commons.codec [auto]
[INFO]    com.google.code.gson:gson:jar:2.10.1:compile -- module com.google.gson
[INFO]    org.bouncycastle:bcutil-jdk18on:jar:1.72:runtime -- module org.bouncycastle.util
[INFO]    org.bouncycastle:bcprov-jdk18on:jar:1.72:runtime -- module org.bouncycastle.provider
[INFO]    org.bouncycastle:bcpkix-jdk18on:jar:1.72:runtime -- module org.bouncycastle.pkix
[INFO]    com.google.oauth-client:google-oauth-client:jar:1.34.1:compile -- module com.google.api.client.auth [auto]
[INFO]    com.google.oauth-client:google-oauth-client-java6:jar:1.34.1:compile -- module com.google.api.client.extensions.java6.auth [auto]
[INFO]    io.grpc:grpc-netty-shaded:jar:1.53.0:runtime -- module grpc.netty.shaded (auto)
[INFO]    io.perfmark:perfmark-api:jar:0.25.0:runtime -- module io.perfmark [auto]
[INFO]    io.grpc:grpc-core:jar:1.53.0:runtime -- module grpc.core (auto)
[INFO]    com.google.android:annotations:jar:4.1.1.4:runtime -- module annotations (auto)
[INFO]    org.codehaus.mojo:animal-sniffer-annotations:jar:1.21:runtime -- module animal.sniffer.annotations (auto)
[INFO]    org.apache.maven.plugins:maven-gpg-plugin:jar:3.1.0:compile -- module maven.gpg.plugin (auto)
[INFO]    org.apache.maven.shared:maven-artifact-transfer:jar:0.13.1:compile -- module org.apache.maven.shared.artifact.transfer [auto]
[INFO]    org.apache.maven.shared:maven-common-artifact-filters:jar:3.1.0:compile -- module maven.common.artifact.filters (auto)
[INFO]    org.sonatype.sisu:sisu-inject-plexus:jar:1.4.2:compile -- module sisu.inject.plexus (auto)
[INFO]    org.sonatype.sisu:sisu-inject-bean:jar:1.4.2:compile -- module sisu.inject.bean (auto)
[INFO]    org.sonatype.sisu:sisu-guice:jar:noaop:2.1.7:compile -- module sisu.guice (auto)
[INFO]    org.codehaus.plexus:plexus-utils:jar:3.4.2:compile -- module plexus.utils (auto)
[INFO]    org.sonatype.plexus:plexus-sec-dispatcher:jar:1.4:compile -- module plexus.sec.dispatcher (auto)
[INFO]    org.sonatype.plexus:plexus-cipher:jar:1.4:compile -- module plexus.cipher (auto)
[INFO]    com.google.oauth-client:google-oauth-client-jetty:jar:1.34.1:compile -- module com.google.api.client.extensions.jetty.auth [auto]
[INFO]    com.google.http-client:google-http-client:jar:1.42.3:compile -- module com.google.api.client [auto]
[INFO]    io.opencensus:opencensus-api:jar:0.31.1:compile -- module opencensus.api (auto)
[INFO]    io.opencensus:opencensus-contrib-http-util:jar:0.31.1:compile -- module opencensus.contrib.http.util (auto)
[INFO]    com.google.http-client:google-http-client-apache-v2:jar:1.42.3:compile -- module com.google.api.client.http.apache.v2 [auto]
[INFO]    org.apache.httpcomponents:httpclient:jar:4.5.13:compile -- module org.apache.httpcomponents.httpclient [auto]
[INFO]    org.apache.httpcomponents:httpcore:jar:4.4.15:compile -- module org.apache.httpcomponents.httpcore [auto]
[INFO]    commons-io:commons-io:jar:2.11.0:compile -- module org.apache.commons.io [auto]
[INFO]    commons-validator:commons-validator:jar:1.7:compile -- module commons.validator (auto)
[INFO]    commons-beanutils:commons-beanutils:jar:1.9.4:compile -- module commons.beanutils (auto)
[INFO]    commons-digester:commons-digester:jar:2.1:compile -- module commons.digester (auto)
[INFO]    commons-logging:commons-logging:jar:1.2:compile -- module commons.logging (auto)
[INFO]    commons-collections:commons-collections:jar:3.2.2:compile -- module commons.collections (auto)
[INFO]    org.apache.maven.shared:maven-jarsigner:jar:3.0.0:compile -- module maven.jarsigner (auto)
[INFO]    org.apache.maven.shared:maven-shared-utils:jar:3.2.1:compile -- module maven.shared.utils (auto)
[INFO]    org.codehaus.plexus:plexus-component-annotations:jar:1.7.1:compile -- module plexus.component.annotations (auto)
[INFO]    org.apache.maven:maven-plugin-api:jar:3.8.4:provided -- module maven.plugin.api (auto)
[INFO]    org.eclipse.sisu:org.eclipse.sisu.plexus:jar:0.3.5:provided -- module org.eclipse.sisu.plexus (auto)
[INFO]    javax.annotation:javax.annotation-api:jar:1.2:provided -- module javax.annotation.api (auto)
[INFO]    org.codehaus.plexus:plexus-classworlds:jar:2.6.0:compile -- module plexus.classworlds (auto)
[INFO]    org.apache.maven:maven-core:jar:3.8.4:provided -- module maven.core (auto)
[INFO]    org.apache.maven:maven-settings:jar:3.8.4:provided -- module maven.settings (auto)
[INFO]    org.apache.maven:maven-settings-builder:jar:3.8.4:provided -- module maven.settings.builder (auto)
[INFO]    org.codehaus.plexus:plexus-sec-dispatcher:jar:2.0:provided -- module plexus.sec.dispatcher (auto)
[INFO]    org.codehaus.plexus:plexus-cipher:jar:2.0:provided -- module plexus.cipher (auto)
[INFO]    org.apache.maven:maven-builder-support:jar:3.8.4:provided -- module maven.builder.support (auto)
[INFO]    org.apache.maven:maven-repository-metadata:jar:3.8.4:provided -- module maven.repository.metadata (auto)
[INFO]    org.apache.maven:maven-model-builder:jar:3.8.4:provided -- module maven.model.builder (auto)
[INFO]    org.apache.maven:maven-resolver-provider:jar:3.8.4:provided -- module maven.resolver.provider (auto)
[INFO]    org.apache.maven.resolver:maven-resolver-impl:jar:1.6.3:provided -- module org.apache.maven.resolver.impl [auto]
[INFO]    org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided -- module org.apache.maven.resolver [auto]
[INFO]    org.apache.maven.resolver:maven-resolver-spi:jar:1.6.3:provided -- module org.apache.maven.resolver.spi [auto]
[INFO]    org.apache.maven.resolver:maven-resolver-util:jar:1.6.3:provided -- module org.apache.maven.resolver.util [auto]
[INFO]    org.eclipse.sisu:org.eclipse.sisu.inject:jar:0.3.5:provided -- module org.eclipse.sisu.inject (auto)
[INFO]    com.google.inject:guice:jar:no_aop:4.2.2:provided -- module com.google.guice [auto]
[INFO]    aopalliance:aopalliance:jar:1.0:provided -- module aopalliance (auto)
[INFO]    javax.inject:javax.inject:jar:1:provided -- module javax.inject (auto)
[INFO]    org.codehaus.plexus:plexus-interpolation:jar:1.26:provided -- module plexus.interpolation (auto)
[INFO]    org.apache.commons:commons-lang3:jar:3.8.1:provided -- module org.apache.commons.lang3 [auto]
[INFO]    org.slf4j:slf4j-api:jar:1.7.32:compile -- module org.slf4j [auto]
[INFO]    org.apache.maven:maven-artifact:jar:3.8.4:provided -- module maven.artifact (auto)
[INFO]    org.apache.maven:maven-model:jar:3.8.4:provided -- module maven.model (auto)
[INFO]    org.apache.maven.plugin-tools:maven-plugin-annotations:jar:3.6.4:provided -- module maven.plugin.annotations (auto)
[INFO]    junit:junit:jar:4.13.2:test -- module junit [auto]
[INFO]    org.hamcrest:hamcrest-core:jar:1.3:test -- module hamcrest.core (auto)
[INFO]    org.apache.maven.plugin-testing:maven-plugin-testing-harness:jar:3.3.0:test -- module maven.plugin.testing.harness (auto)
[INFO]    org.codehaus.plexus:plexus-archiver:jar:2.2:test -- module plexus.archiver (auto)
[INFO]    org.codehaus.plexus:plexus-container-default:jar:1.0-alpha-9-stable-1:test
[INFO]    classworlds:classworlds:jar:1.1-alpha-2:test -- module classworlds (auto)
[INFO]    org.codehaus.plexus:plexus-io:jar:2.0.4:test -- module plexus.io (auto)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.955 s
[INFO] Finished at: 2023-07-23T16:16:48+02:00
[INFO] ------------------------------------------------------------------------
```

## `dependency:tree -Dverbose`

```
$ mvn dependency:tree -Dverbose
[INFO] Scanning for projects...
[INFO] Inspecting build with total of 1 modules...
[INFO] Installing Nexus Staging features:
[INFO]   ... total of 1 executions of maven-deploy-plugin replaced with nexus-staging-maven-plugin
[INFO] 
[INFO] -----------------< dev.sigstore:sigstore-maven-plugin >-----------------
[INFO] Building sigstore Maven Plugin 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] ----------------------------[ maven-plugin ]----------------------------
[INFO] 
[INFO] --- dependency:3.6.0:tree (default-cli) @ sigstore-maven-plugin ---
[INFO] dev.sigstore:sigstore-maven-plugin:maven-plugin:1.0-SNAPSHOT
[INFO] +- dev.sigstore:sigstore-java:jar:0.4.0:compile
[INFO] |  +- (com.google.http-client:google-http-client-apache-v2:jar:1.42.3:runtime - version managed from 1.42.3; omitted for duplicate)
[INFO] |  +- com.google.http-client:google-http-client-gson:jar:1.42.3:compile (version managed from 1.42.3)
[INFO] |  |  +- (com.google.http-client:google-http-client:jar:1.42.3:compile - version managed from 1.42.3; omitted for duplicate)
[INFO] |  |  \- (com.google.code.gson:gson:jar:2.10.1:compile - version managed from 2.10; omitted for duplicate)
[INFO] |  +- io.github.erdtman:java-json-canonicalization:jar:1.1:runtime
[INFO] |  +- dev.sigstore:protobuf-specs:jar:0.1.0:runtime
[INFO] |  |  +- com.google.protobuf:protobuf-java:jar:3.21.12:runtime (version managed from 3.21.9)
[INFO] |  |  \- com.google.api.grpc:proto-google-common-protos:jar:2.14.1:runtime (version managed from 2.11.0)
[INFO] |  |     \- (com.google.protobuf:protobuf-java:jar:3.21.12:runtime - version managed from 3.21.12; omitted for duplicate)
[INFO] |  +- com.google.protobuf:protobuf-java-util:jar:3.21.12:runtime (version managed from 3.22.0)
[INFO] |  |  +- (com.google.protobuf:protobuf-java:jar:3.21.12:runtime - version managed from 3.21.12; omitted for duplicate)
[INFO] |  |  +- com.google.guava:guava:jar:31.1-jre:compile (version managed from 31.1-android)
[INFO] |  |  |  +- com.google.guava:failureaccess:jar:1.0.1:compile
[INFO] |  |  |  +- com.google.guava:listenablefuture:jar:9999.0-empty-to-avoid-conflict-with-guava:compile
[INFO] |  |  |  +- (com.google.code.findbugs:jsr305:jar:3.0.2:compile - omitted for duplicate)
[INFO] |  |  |  +- org.checkerframework:checker-qual:jar:3.12.0:compile
[INFO] |  |  |  +- (com.google.errorprone:error_prone_annotations:jar:2.11.0:compile - omitted for conflict with 2.5.1)
[INFO] |  |  |  \- (com.google.j2objc:j2objc-annotations:jar:1.3:compile - omitted for duplicate)
[INFO] |  |  +- com.google.errorprone:error_prone_annotations:jar:2.5.1:compile
[INFO] |  |  +- com.google.j2objc:j2objc-annotations:jar:1.3:compile
[INFO] |  |  +- com.google.code.findbugs:jsr305:jar:3.0.2:compile
[INFO] |  |  \- (com.google.code.gson:gson:jar:2.10.1:runtime - version managed from 2.8.9; omitted for duplicate)
[INFO] |  +- io.grpc:grpc-protobuf:jar:1.53.0:runtime (version managed from 1.53.0)
[INFO] |  |  +- io.grpc:grpc-api:jar:1.53.0:runtime (version managed from 1.53.0)
[INFO] |  |  |  +- io.grpc:grpc-context:jar:1.53.0:compile (version managed from 1.53.0; scope not updated to compile)
[INFO] |  |  |  +- (com.google.code.findbugs:jsr305:jar:3.0.2:runtime - omitted for duplicate)
[INFO] |  |  |  +- (com.google.errorprone:error_prone_annotations:jar:2.14.0:runtime - omitted for conflict with 2.5.1)
[INFO] |  |  |  \- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |  |  +- (com.google.code.findbugs:jsr305:jar:3.0.2:runtime - omitted for duplicate)
[INFO] |  |  +- (com.google.protobuf:protobuf-java:jar:3.21.12:runtime - version managed from 3.21.7; omitted for duplicate)
[INFO] |  |  +- (com.google.api.grpc:proto-google-common-protos:jar:2.14.1:runtime - version managed from 2.9.0; omitted for duplicate)
[INFO] |  |  +- io.grpc:grpc-protobuf-lite:jar:1.53.0:runtime (version managed from 1.53.0)
[INFO] |  |  |  +- (io.grpc:grpc-api:jar:1.53.0:runtime - version managed from 1.53.0; omitted for duplicate)
[INFO] |  |  |  +- (com.google.code.findbugs:jsr305:jar:3.0.2:runtime - omitted for duplicate)
[INFO] |  |  |  \- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |  |  \- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |  +- io.grpc:grpc-stub:jar:1.53.0:runtime (version managed from 1.53.0)
[INFO] |  |  +- (io.grpc:grpc-api:jar:1.53.0:runtime - version managed from 1.53.0; omitted for duplicate)
[INFO] |  |  +- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |  |  \- (com.google.errorprone:error_prone_annotations:jar:2.14.0:runtime - omitted for conflict with 2.5.1)
[INFO] |  +- commons-codec:commons-codec:jar:1.15:compile (scope not updated to compile)
[INFO] |  +- com.google.code.gson:gson:jar:2.10.1:compile (version managed from 2.10.1)
[INFO] |  +- org.bouncycastle:bcutil-jdk18on:jar:1.72:runtime
[INFO] |  |  \- org.bouncycastle:bcprov-jdk18on:jar:1.72:runtime
[INFO] |  +- org.bouncycastle:bcpkix-jdk18on:jar:1.72:runtime
[INFO] |  |  +- (org.bouncycastle:bcprov-jdk18on:jar:1.72:runtime - omitted for duplicate)
[INFO] |  |  \- (org.bouncycastle:bcutil-jdk18on:jar:1.72:runtime - omitted for duplicate)
[INFO] |  +- com.google.oauth-client:google-oauth-client:jar:1.34.1:compile (version managed from 1.34.1)
[INFO] |  |  +- (com.google.http-client:google-http-client:jar:1.42.3:compile - version managed from 1.42.0; omitted for duplicate)
[INFO] |  |  +- (com.google.http-client:google-http-client-gson:jar:1.42.3:compile - version managed from 1.42.0; omitted for duplicate)
[INFO] |  |  \- (com.google.guava:guava:jar:31.1-jre:compile - version managed from 31.1-android; omitted for duplicate)
[INFO] |  +- (com.google.oauth-client:google-oauth-client-jetty:jar:1.34.1:runtime - version managed from 1.34.1; omitted for duplicate)
[INFO] |  +- com.google.oauth-client:google-oauth-client-java6:jar:1.34.1:compile (version managed from 1.34.1)
[INFO] |  |  +- (com.google.oauth-client:google-oauth-client:jar:1.34.1:compile - version managed from 1.34.1; omitted for duplicate)
[INFO] |  |  \- (com.google.http-client:google-http-client:jar:1.42.3:compile - version managed from 1.42.0; omitted for duplicate)
[INFO] |  \- io.grpc:grpc-netty-shaded:jar:1.53.0:runtime (version managed from 1.53.0)
[INFO] |     +- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |     +- (com.google.errorprone:error_prone_annotations:jar:2.14.0:runtime - omitted for conflict with 2.5.1)
[INFO] |     +- io.perfmark:perfmark-api:jar:0.25.0:runtime
[INFO] |     \- io.grpc:grpc-core:jar:1.53.0:runtime (version managed from [1.53.0])
[INFO] |        +- (io.grpc:grpc-api:jar:1.53.0:runtime - version managed from [1.53.0]; omitted for duplicate)
[INFO] |        +- (com.google.code.gson:gson:jar:2.10.1:runtime - version managed from 2.9.0; omitted for duplicate)
[INFO] |        +- com.google.android:annotations:jar:4.1.1.4:runtime
[INFO] |        +- org.codehaus.mojo:animal-sniffer-annotations:jar:1.21:runtime
[INFO] |        +- (com.google.errorprone:error_prone_annotations:jar:2.14.0:runtime - omitted for conflict with 2.5.1)
[INFO] |        +- (com.google.guava:guava:jar:31.1-jre:runtime - version managed from 31.1-android; omitted for duplicate)
[INFO] |        \- (io.perfmark:perfmark-api:jar:0.25.0:runtime - omitted for duplicate)
[INFO] +- org.apache.maven.plugins:maven-gpg-plugin:jar:3.1.0:compile
[INFO] |  +- org.apache.maven.shared:maven-artifact-transfer:jar:0.13.1:compile
[INFO] |  |  +- (org.apache.maven:maven-core:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  +- (org.apache.maven:maven-artifact:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  +- (org.codehaus.plexus:plexus-component-annotations:jar:2.0.0:compile - omitted for conflict with 1.7.1)
[INFO] |  |  +- org.apache.maven.shared:maven-common-artifact-filters:jar:3.1.0:compile
[INFO] |  |  |  +- (org.apache.maven:maven-artifact:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  |  +- (org.apache.maven:maven-model:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  |  +- (org.apache.maven:maven-core:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  |  +- (org.apache.maven:maven-plugin-api:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] |  |  |  +- org.sonatype.sisu:sisu-inject-plexus:jar:1.4.2:compile
[INFO] |  |  |  |  +- (org.codehaus.plexus:plexus-component-annotations:jar:1.5.4:compile - omitted for conflict with 1.7.1)
[INFO] |  |  |  |  +- (org.codehaus.plexus:plexus-classworlds:jar:2.2.3:compile - omitted for conflict with 2.6.0)
[INFO] |  |  |  |  +- (org.codehaus.plexus:plexus-utils:jar:2.0.5:compile - omitted for conflict with 3.4.2)
[INFO] |  |  |  |  \- org.sonatype.sisu:sisu-inject-bean:jar:1.4.2:compile
[INFO] |  |  |  |     \- org.sonatype.sisu:sisu-guice:jar:noaop:2.1.7:compile
[INFO] |  |  |  \- (org.apache.maven.shared:maven-shared-utils:jar:3.1.0:compile - omitted for conflict with 3.2.1)
[INFO] |  |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:compile - omitted for conflict with 3.4.2)
[INFO] |  |  \- (org.slf4j:slf4j-api:jar:1.7.5:compile - omitted for conflict with 1.7.32)
[INFO] |  +- org.codehaus.plexus:plexus-utils:jar:3.4.2:compile
[INFO] |  \- org.sonatype.plexus:plexus-sec-dispatcher:jar:1.4:compile
[INFO] |     +- (org.codehaus.plexus:plexus-utils:jar:1.5.5:compile - omitted for conflict with 3.4.2)
[INFO] |     \- org.sonatype.plexus:plexus-cipher:jar:1.4:compile
[INFO] +- com.google.oauth-client:google-oauth-client-jetty:jar:1.34.1:compile
[INFO] |  +- (com.google.oauth-client:google-oauth-client-java6:jar:1.34.1:compile - version managed from 1.34.1; omitted for duplicate)
[INFO] |  \- com.google.http-client:google-http-client:jar:1.42.3:compile (version managed from 1.42.0)
[INFO] |     +- (org.apache.httpcomponents:httpclient:jar:4.5.13:compile - omitted for duplicate)
[INFO] |     +- (org.apache.httpcomponents:httpcore:jar:4.4.15:compile - omitted for duplicate)
[INFO] |     +- (com.google.code.findbugs:jsr305:jar:3.0.2:compile - omitted for duplicate)
[INFO] |     +- (com.google.errorprone:error_prone_annotations:jar:2.16:compile - omitted for conflict with 2.5.1)
[INFO] |     +- (com.google.guava:guava:jar:31.1-jre:compile - version managed from 30.1.1-android; omitted for duplicate)
[INFO] |     +- (com.google.j2objc:j2objc-annotations:jar:1.3:compile - omitted for duplicate)
[INFO] |     +- io.opencensus:opencensus-api:jar:0.31.1:compile
[INFO] |     |  \- (io.grpc:grpc-context:jar:1.53.0:compile - version managed from 1.27.2; omitted for duplicate)
[INFO] |     \- io.opencensus:opencensus-contrib-http-util:jar:0.31.1:compile
[INFO] |        +- (io.opencensus:opencensus-api:jar:0.31.1:compile - omitted for duplicate)
[INFO] |        \- (com.google.guava:guava:jar:31.1-jre:compile - version managed from 29.0-android; omitted for duplicate)
[INFO] +- com.google.http-client:google-http-client-apache-v2:jar:1.42.3:compile
[INFO] |  +- (com.google.http-client:google-http-client:jar:1.42.3:compile - version managed from 1.42.3; omitted for duplicate)
[INFO] |  +- org.apache.httpcomponents:httpclient:jar:4.5.13:compile
[INFO] |  |  +- (org.apache.httpcomponents:httpcore:jar:4.4.13:compile - omitted for conflict with 4.4.15)
[INFO] |  |  +- (commons-logging:commons-logging:jar:1.2:compile - omitted for duplicate)
[INFO] |  |  \- (commons-codec:commons-codec:jar:1.11:compile - omitted for conflict with 1.15)
[INFO] |  \- org.apache.httpcomponents:httpcore:jar:4.4.15:compile
[INFO] +- commons-io:commons-io:jar:2.11.0:compile
[INFO] +- commons-validator:commons-validator:jar:1.7:compile
[INFO] |  +- commons-beanutils:commons-beanutils:jar:1.9.4:compile
[INFO] |  |  +- (commons-logging:commons-logging:jar:1.2:compile - omitted for duplicate)
[INFO] |  |  \- (commons-collections:commons-collections:jar:3.2.2:compile - omitted for duplicate)
[INFO] |  +- commons-digester:commons-digester:jar:2.1:compile
[INFO] |  +- commons-logging:commons-logging:jar:1.2:compile
[INFO] |  \- commons-collections:commons-collections:jar:3.2.2:compile
[INFO] +- org.apache.maven.shared:maven-jarsigner:jar:3.0.0:compile
[INFO] |  +- org.apache.maven.shared:maven-shared-utils:jar:3.2.1:compile
[INFO] |  |  \- (commons-io:commons-io:jar:2.5:compile - omitted for conflict with 2.11.0)
[INFO] |  +- org.codehaus.plexus:plexus-component-annotations:jar:1.7.1:compile (scope not updated to compile)
[INFO] |  \- (org.apache.maven:maven-core:jar:3.0:compile - omitted for conflict with 3.8.4)
[INFO] +- org.apache.maven:maven-plugin-api:jar:3.8.4:provided (scope not updated to compile)
[INFO] |  +- (org.apache.maven:maven-model:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  +- (org.apache.maven:maven-artifact:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  +- org.eclipse.sisu:org.eclipse.sisu.plexus:jar:0.3.5:provided
[INFO] |  |  +- javax.annotation:javax.annotation-api:jar:1.2:provided
[INFO] |  |  +- (org.eclipse.sisu:org.eclipse.sisu.inject:jar:0.3.5:provided - omitted for duplicate)
[INFO] |  |  +- (org.codehaus.plexus:plexus-component-annotations:jar:1.5.5:provided - omitted for conflict with 1.7.1)
[INFO] |  |  +- (org.codehaus.plexus:plexus-classworlds:jar:2.5.2:provided - omitted for conflict with 2.6.0)
[INFO] |  |  \- (org.codehaus.plexus:plexus-utils:jar:3.0.24:provided - omitted for conflict with 3.4.2)
[INFO] |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  \- org.codehaus.plexus:plexus-classworlds:jar:2.6.0:compile
[INFO] +- org.apache.maven:maven-core:jar:3.8.4:provided (scope not updated to compile)
[INFO] |  +- (org.apache.maven:maven-model:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven:maven-settings:jar:3.8.4:provided
[INFO] |  |  \- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  +- org.apache.maven:maven-settings-builder:jar:3.8.4:provided
[INFO] |  |  +- (org.apache.maven:maven-builder-support:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  |  +- (org.codehaus.plexus:plexus-interpolation:jar:1.26:provided - omitted for duplicate)
[INFO] |  |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  |  +- (org.apache.maven:maven-settings:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  \- org.codehaus.plexus:plexus-sec-dispatcher:jar:2.0:provided
[INFO] |  |     +- (org.codehaus.plexus:plexus-utils:jar:3.4.1:provided - omitted for conflict with 3.4.2)
[INFO] |  |     +- org.codehaus.plexus:plexus-cipher:jar:2.0:provided
[INFO] |  |     |  \- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  |     \- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven:maven-builder-support:jar:3.8.4:provided
[INFO] |  +- org.apache.maven:maven-repository-metadata:jar:3.8.4:provided
[INFO] |  |  \- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  +- (org.apache.maven:maven-artifact:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  +- (org.apache.maven:maven-plugin-api:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven:maven-model-builder:jar:3.8.4:provided
[INFO] |  |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  |  +- (org.codehaus.plexus:plexus-interpolation:jar:1.26:provided - omitted for duplicate)
[INFO] |  |  +- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven:maven-model:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven:maven-artifact:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven:maven-builder-support:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  \- (org.eclipse.sisu:org.eclipse.sisu.inject:jar:0.3.5:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven:maven-resolver-provider:jar:3.8.4:provided
[INFO] |  |  +- (org.apache.maven:maven-model:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven:maven-model-builder:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven:maven-repository-metadata:jar:3.8.4:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-spi:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-util:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-impl:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  |  \- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven.resolver:maven-resolver-impl:jar:1.6.3:provided
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-spi:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.maven.resolver:maven-resolver-util:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  |  +- (org.apache.commons:commons-lang3:jar:3.8.1:provided - omitted for duplicate)
[INFO] |  |  \- (org.slf4j:slf4j-api:jar:1.7.30:provided - omitted for conflict with 1.7.32)
[INFO] |  +- org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided
[INFO] |  +- org.apache.maven.resolver:maven-resolver-spi:jar:1.6.3:provided
[INFO] |  |  \- (org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  +- org.apache.maven.resolver:maven-resolver-util:jar:1.6.3:provided
[INFO] |  |  \- (org.apache.maven.resolver:maven-resolver-api:jar:1.6.3:provided - omitted for duplicate)
[INFO] |  +- (org.apache.maven.shared:maven-shared-utils:jar:3.3.4:provided - omitted for conflict with 3.2.1)
[INFO] |  +- (org.eclipse.sisu:org.eclipse.sisu.plexus:jar:0.3.5:provided - omitted for duplicate)
[INFO] |  +- org.eclipse.sisu:org.eclipse.sisu.inject:jar:0.3.5:provided
[INFO] |  +- com.google.inject:guice:jar:no_aop:4.2.2:provided
[INFO] |  |  +- (javax.inject:javax.inject:jar:1:provided - omitted for duplicate)
[INFO] |  |  +- aopalliance:aopalliance:jar:1.0:provided
[INFO] |  |  \- (com.google.guava:guava:jar:31.1-jre:provided - version managed from 25.1-android; omitted for duplicate)
[INFO] |  +- javax.inject:javax.inject:jar:1:provided
[INFO] |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  +- (org.codehaus.plexus:plexus-classworlds:jar:2.6.0:provided - omitted for duplicate)
[INFO] |  +- org.codehaus.plexus:plexus-interpolation:jar:1.26:provided
[INFO] |  +- (org.codehaus.plexus:plexus-component-annotations:jar:2.1.0:provided - omitted for conflict with 1.7.1)
[INFO] |  +- org.apache.commons:commons-lang3:jar:3.8.1:provided
[INFO] |  \- org.slf4j:slf4j-api:jar:1.7.32:compile (scope not updated to compile)
[INFO] +- org.apache.maven:maven-artifact:jar:3.8.4:provided (scope not updated to compile)
[INFO] |  +- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] |  \- (org.apache.commons:commons-lang3:jar:3.8.1:provided - omitted for duplicate)
[INFO] +- org.apache.maven:maven-model:jar:3.8.4:provided (scope not updated to compile)
[INFO] |  \- (org.codehaus.plexus:plexus-utils:jar:3.3.0:provided - omitted for conflict with 3.4.2)
[INFO] +- org.apache.maven.plugin-tools:maven-plugin-annotations:jar:3.6.4:provided
[INFO] +- junit:junit:jar:4.13.2:test
[INFO] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] \- org.apache.maven.plugin-testing:maven-plugin-testing-harness:jar:3.3.0:test
[INFO]    +- (commons-io:commons-io:jar:2.2:test - omitted for conflict with 2.11.0)
[INFO]    \- org.codehaus.plexus:plexus-archiver:jar:2.2:test
[INFO]       +- org.codehaus.plexus:plexus-container-default:jar:1.0-alpha-9-stable-1:test
[INFO]       |  +- (junit:junit:jar:3.8.1:test - omitted for conflict with 4.13.2)
[INFO]       |  +- (org.codehaus.plexus:plexus-utils:jar:1.0.4:test - omitted for conflict with 3.4.2)
[INFO]       |  \- classworlds:classworlds:jar:1.1-alpha-2:test
[INFO]       +- (org.codehaus.plexus:plexus-utils:jar:3.0.7:test - omitted for conflict with 3.4.2)
[INFO]       \- org.codehaus.plexus:plexus-io:jar:2.0.4:test
[INFO]          \- (org.codehaus.plexus:plexus-utils:jar:3.0:test - omitted for conflict with 3.4.2)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.885 s
[INFO] Finished at: 2023-07-23T16:22:21+02:00
[INFO] ------------------------------------------------------------------------
```

## `help:effective-pom -Dverbose`

```
[INFO] Scanning for projects...
[INFO] Inspecting build with total of 1 modules...
[INFO] Installing Nexus Staging features:
[INFO]   ... total of 1 executions of maven-deploy-plugin replaced with nexus-staging-maven-plugin
[INFO] 
[INFO] -----------------< dev.sigstore:sigstore-maven-plugin >-----------------
[INFO] Building sigstore Maven Plugin 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] ----------------------------[ maven-plugin ]----------------------------
[INFO] 
[INFO] --- help:3.4.0:effective-pom (default-cli) @ sigstore-maven-plugin ---
[INFO] 
Effective POMs, after inheritance, interpolation, and profiles are applied:

<?xml version="1.0" encoding="UTF-8"?>
<!-- ====================================================================== -->
<!--                                                                        -->
<!-- Generated by Maven Help Plugin                                         -->
<!-- See: https://maven.apache.org/plugins/maven-help-plugin/               -->
<!--                                                                        -->
<!-- ====================================================================== -->
<!-- ====================================================================== -->
<!--                                                                        -->
<!-- Effective POM for project                                              -->
<!-- 'dev.sigstore:sigstore-maven-plugin:maven-plugin:1.0-SNAPSHOT'         -->
<!--                                                                        -->
<!-- ====================================================================== -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 20 -->
  <groupId>dev.sigstore</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 22 -->
  <artifactId>sigstore-maven-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 23 -->
  <version>1.0-SNAPSHOT</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 24 -->
  <packaging>maven-plugin</packaging>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 25 -->
  <name>sigstore Maven Plugin</name>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 27 -->
  <url>https://sigstore.github.io/sigstore-maven-plugin/</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 29 -->
  <organization>
    <name>sigstore</name>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 32 -->
    <url>https://sigstore.dev/</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 33 -->
  </organization>
  <licenses>
    <license>
      <name>The Apache License, Version 2.0</name>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 38 -->
      <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 39 -->
    </license>
  </licenses>
  <prerequisites>
    <maven>3.8.4</maven>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 44 -->
  </prerequisites>
  <scm>
    <connection>scm:git:https://github.com/sigstore/sigstore-maven-plugin.git</connection>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 48 -->
    <developerConnection>scm:git:https://github.com/sigstore/sigstore-maven-plugin.git</developerConnection>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 49 -->
    <tag>main</tag>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 51 -->
    <url>https://github.com/sigstore/sigstore-maven-plugin/tree/main</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 50 -->
  </scm>
  <issueManagement>
    <system>GitHub</system>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 54 -->
    <url>https://github.com/sigstore/sigstore-maven-plugin/issues/</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 55 -->
  </issueManagement>
  <distributionManagement>
    <repository>
      <id>ossrh</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 64 -->
      <url>https://s01.oss.sonatype.org/service/local/staging/deploy/maven2/</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 65 -->
    </repository>
    <snapshotRepository>
      <id>ossrh</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 60 -->
      <url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 61 -->
    </snapshotRepository>
    <site>
      <id>gh-pages</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 68 -->
      <url>scm:git:ssh://git@github.com/sigstore/sigstore-maven-plugin.git</url>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 69 -->
    </site>
  </distributionManagement>
  <properties>
    <maven.compiler.release>11</maven.compiler.release>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 75 -->
    <maven.version>3.8.4</maven.version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 76 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 74 -->
  </properties>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>dev.sigstore</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 82 -->
        <artifactId>sigstore-java</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 83 -->
        <version>0.4.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 84 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 43 -->
        <artifactId>grpc-gcp</artifactId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 44 -->
        <version>1.4.1</version>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.code.gson</groupId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 48 -->
        <artifactId>gson</artifactId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 49 -->
        <version>2.10.1</version>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 84 -->
        <artifactId>google-cloud-core</artifactId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 85 -->
        <version>2.11.0</version>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 86 -->
        <type>test-jar</type>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 87 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 90 -->
        <artifactId>google-cloud-core</artifactId>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 91 -->
        <version>2.11.0</version>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 92 -->
        <classifier>tests</classifier>  <!-- com.google.cloud:first-party-dependencies:3.3.0, line 93 -->
      </dependency>
      <dependency>
        <groupId>com.google.auto.value</groupId>  <!-- com.google.cloud:google-cloud-shared-config:1.5.5, line 546 -->
        <artifactId>auto-value-annotations</artifactId>  <!-- com.google.cloud:google-cloud-shared-config:1.5.5, line 547 -->
        <version>1.10.1</version>  <!-- com.google.cloud:google-cloud-shared-config:1.5.5, line 548 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 61 -->
        <artifactId>api-common</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 62 -->
        <version>2.6.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 63 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 73 -->
        <artifactId>grpc-google-common-protos</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 74 -->
        <version>2.14.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 75 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 78 -->
        <artifactId>proto-google-common-protos</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 79 -->
        <version>2.14.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 80 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 83 -->
        <artifactId>proto-google-iam-v1</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 84 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 85 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 88 -->
        <artifactId>proto-google-iam-v2</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 89 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 90 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 93 -->
        <artifactId>proto-google-iam-v2beta</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 94 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 95 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 100 -->
        <artifactId>grpc-google-iam-v1</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 101 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 102 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 105 -->
        <artifactId>grpc-google-iam-v2</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 106 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 107 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 110 -->
        <artifactId>grpc-google-iam-v2beta</artifactId>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 111 -->
        <version>1.9.1</version>  <!-- com.google.api:gapic-generator-java-bom:2.15.1, line 112 -->
      </dependency>
      <dependency>
        <groupId>com.google.auth</groupId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 53 -->
        <artifactId>google-auth-library-credentials</artifactId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 54 -->
        <version>1.16.0</version>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.auth</groupId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 58 -->
        <artifactId>google-auth-library-oauth2-http</artifactId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 59 -->
        <version>1.16.0</version>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.auth</groupId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 63 -->
        <artifactId>google-auth-library-appengine</artifactId>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 64 -->
        <version>1.16.0</version>  <!-- com.google.auth:google-auth-library-bom:1.16.0, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.guava</groupId>  <!-- com.google.guava:guava-bom:31.1-jre, line 42 -->
        <artifactId>guava</artifactId>  <!-- com.google.guava:guava-bom:31.1-jre, line 43 -->
        <version>31.1-jre</version>  <!-- com.google.guava:guava-bom:31.1-jre, line 44 -->
      </dependency>
      <dependency>
        <groupId>com.google.guava</groupId>  <!-- com.google.guava:guava-bom:31.1-jre, line 47 -->
        <artifactId>guava-gwt</artifactId>  <!-- com.google.guava:guava-bom:31.1-jre, line 48 -->
        <version>31.1-jre</version>  <!-- com.google.guava:guava-bom:31.1-jre, line 49 -->
      </dependency>
      <dependency>
        <groupId>com.google.guava</groupId>  <!-- com.google.guava:guava-bom:31.1-jre, line 52 -->
        <artifactId>guava-testlib</artifactId>  <!-- com.google.guava:guava-bom:31.1-jre, line 53 -->
        <version>31.1-jre</version>  <!-- com.google.guava:guava-bom:31.1-jre, line 54 -->
      </dependency>
      <dependency>
        <groupId>com.google.protobuf</groupId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 60 -->
        <artifactId>protobuf-java</artifactId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 61 -->
        <version>3.21.12</version>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 62 -->
      </dependency>
      <dependency>
        <groupId>com.google.protobuf</groupId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 65 -->
        <artifactId>protobuf-java-util</artifactId>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 66 -->
        <version>3.21.12</version>  <!-- com.google.protobuf:protobuf-bom:3.21.12, line 67 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 35 -->
        <artifactId>grpc-all</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 36 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 37 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 40 -->
        <artifactId>grpc-alts</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 41 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 42 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 45 -->
        <artifactId>grpc-api</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 46 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 47 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 50 -->
        <artifactId>grpc-auth</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 51 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 52 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 55 -->
        <artifactId>grpc-benchmarks</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 56 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 57 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 60 -->
        <artifactId>grpc-census</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 61 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 62 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 65 -->
        <artifactId>grpc-context</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 66 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 67 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 70 -->
        <artifactId>grpc-core</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 71 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 72 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 75 -->
        <artifactId>grpc-gcp-observability</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 76 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 77 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 80 -->
        <artifactId>grpc-googleapis</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 81 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 82 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 85 -->
        <artifactId>grpc-grpclb</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 86 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 87 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 90 -->
        <artifactId>grpc-interop-testing</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 91 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 92 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 95 -->
        <artifactId>grpc-netty</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 96 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 97 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 100 -->
        <artifactId>grpc-netty-shaded</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 101 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 102 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 105 -->
        <artifactId>grpc-okhttp</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 106 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 107 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 110 -->
        <artifactId>grpc-protobuf</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 111 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 112 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 115 -->
        <artifactId>grpc-protobuf-lite</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 116 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 117 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 120 -->
        <artifactId>grpc-rls</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 121 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 122 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 125 -->
        <artifactId>grpc-services</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 126 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 127 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 130 -->
        <artifactId>grpc-servlet</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 131 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 132 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 135 -->
        <artifactId>grpc-servlet-jakarta</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 136 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 137 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 140 -->
        <artifactId>grpc-stub</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 141 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 142 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 145 -->
        <artifactId>grpc-testing</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 146 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 147 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 150 -->
        <artifactId>grpc-testing-proto</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 151 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 152 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 155 -->
        <artifactId>grpc-xds</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 156 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 157 -->
      </dependency>
      <dependency>
        <groupId>io.grpc</groupId>  <!-- io.grpc:grpc-bom:1.53.0, line 160 -->
        <artifactId>protoc-gen-grpc-java</artifactId>  <!-- io.grpc:grpc-bom:1.53.0, line 161 -->
        <version>1.53.0</version>  <!-- io.grpc:grpc-bom:1.53.0, line 162 -->
        <type>pom</type>  <!-- io.grpc:grpc-bom:1.53.0, line 163 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 43 -->
        <artifactId>gax</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 44 -->
        <version>2.23.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 48 -->
        <artifactId>gax</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 49 -->
        <version>2.23.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 50 -->
        <classifier>testlib</classifier>  <!-- com.google.api:gax-bom:2.23.1, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 54 -->
        <artifactId>gax-grpc</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 55 -->
        <version>2.23.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 59 -->
        <artifactId>gax-grpc</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 60 -->
        <version>2.23.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 61 -->
        <classifier>testlib</classifier>  <!-- com.google.api:gax-bom:2.23.1, line 62 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 65 -->
        <artifactId>gax-httpjson</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 66 -->
        <version>0.108.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 67 -->
      </dependency>
      <dependency>
        <groupId>com.google.api</groupId>  <!-- com.google.api:gax-bom:2.23.1, line 70 -->
        <artifactId>gax-httpjson</artifactId>  <!-- com.google.api:gax-bom:2.23.1, line 71 -->
        <version>0.108.1</version>  <!-- com.google.api:gax-bom:2.23.1, line 72 -->
        <classifier>testlib</classifier>  <!-- com.google.api:gax-bom:2.23.1, line 73 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-core</artifactId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 29 -->
        <artifactId>google-cloud-core-grpc</artifactId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 34 -->
        <artifactId>google-cloud-core-http</artifactId>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-core-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 64 -->
        <artifactId>google-http-client</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 65 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 69 -->
        <artifactId>google-http-client-android</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 70 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 74 -->
        <artifactId>google-http-client-apache-v2</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 75 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 79 -->
        <artifactId>google-http-client-appengine</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 80 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 81 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 84 -->
        <artifactId>google-http-client-findbugs</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 85 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 86 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 89 -->
        <artifactId>google-http-client-gson</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 90 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 91 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 94 -->
        <artifactId>google-http-client-jackson2</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 95 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 96 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 99 -->
        <artifactId>google-http-client-protobuf</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 100 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 101 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 104 -->
        <artifactId>google-http-client-test</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 105 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 106 -->
      </dependency>
      <dependency>
        <groupId>com.google.http-client</groupId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 109 -->
        <artifactId>google-http-client-xml</artifactId>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 110 -->
        <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 111 -->
      </dependency>
      <dependency>
        <groupId>com.google.oauth-client</groupId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 64 -->
        <artifactId>google-oauth-client</artifactId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 65 -->
        <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.oauth-client</groupId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 69 -->
        <artifactId>google-oauth-client-appengine</artifactId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 70 -->
        <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.oauth-client</groupId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 74 -->
        <artifactId>google-oauth-client-java6</artifactId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 75 -->
        <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.oauth-client</groupId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 79 -->
        <artifactId>google-oauth-client-jetty</artifactId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 80 -->
        <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 81 -->
      </dependency>
      <dependency>
        <groupId>com.google.oauth-client</groupId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 84 -->
        <artifactId>google-oauth-client-servlet</artifactId>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 85 -->
        <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 86 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 64 -->
        <artifactId>google-api-client</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 65 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 69 -->
        <artifactId>google-api-client-android</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 70 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 74 -->
        <artifactId>google-api-client-appengine</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 75 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 79 -->
        <artifactId>google-api-client-assembly</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 80 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 81 -->
        <type>pom</type>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 82 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 85 -->
        <artifactId>google-api-client-gson</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 86 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 87 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 90 -->
        <artifactId>google-api-client-jackson2</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 91 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 92 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 95 -->
        <artifactId>google-api-client-protobuf</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 96 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 97 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 100 -->
        <artifactId>google-api-client-servlet</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 101 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 102 -->
      </dependency>
      <dependency>
        <groupId>com.google.api-client</groupId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 105 -->
        <artifactId>google-api-client-xml</artifactId>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 106 -->
        <version>2.2.0</version>  <!-- com.google.api-client:google-api-client-bom:2.2.0, line 107 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 175 -->
        <artifactId>google-cloud-bigquery</artifactId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 176 -->
        <version>2.23.0</version>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 177 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 215 -->
        <artifactId>google-cloud-logging-logback</artifactId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 216 -->
        <version>0.130.5-alpha</version>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 217 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 220 -->
        <artifactId>google-cloud-nio</artifactId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 221 -->
        <version>0.126.6</version>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 222 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 246 -->
        <artifactId>google-cloud-spanner-jdbc</artifactId>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 247 -->
        <version>2.9.7</version>  <!-- com.google.cloud:google-cloud-bom:0.190.0, line 248 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1005 -->
        <artifactId>google-cloud-dns</artifactId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1006 -->
        <version>2.9.0</version>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1007 -->
      </dependency>
      <dependency>
        <groupId>io.grafeas</groupId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1010 -->
        <artifactId>grafeas</artifactId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1011 -->
        <version>2.12.0</version>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1012 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1015 -->
        <artifactId>google-cloud-notification</artifactId>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1016 -->
        <version>0.129.0-beta</version>  <!-- com.google.cloud:gapic-libraries-bom:1.5.0, line 1017 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-accessapproval</artifactId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-accessapproval-v1</artifactId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 30 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 34 -->
        <artifactId>proto-google-cloud-accessapproval-v1</artifactId>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 35 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-accessapproval-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 28 -->
        <artifactId>google-identity-accesscontextmanager</artifactId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 29 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 33 -->
        <artifactId>grpc-google-identity-accesscontextmanager-v1</artifactId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 34 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 38 -->
        <artifactId>proto-google-identity-accesscontextmanager-v1</artifactId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 39 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 43 -->
        <artifactId>proto-google-identity-accesscontextmanager-type</artifactId>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 44 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-identity-accesscontextmanager-bom:1.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 28 -->
        <artifactId>google-cloud-aiplatform</artifactId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 29 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 33 -->
        <artifactId>grpc-google-cloud-aiplatform-v1</artifactId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 34 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 38 -->
        <artifactId>grpc-google-cloud-aiplatform-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 39 -->
        <version>0.28.0</version>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 43 -->
        <artifactId>proto-google-cloud-aiplatform-v1</artifactId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 44 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 48 -->
        <artifactId>proto-google-cloud-aiplatform-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 49 -->
        <version>0.28.0</version>  <!-- com.google.cloud:google-cloud-aiplatform-bom:3.12.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.analytics</groupId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 28 -->
        <artifactId>google-analytics-admin</artifactId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 29 -->
        <version>0.21.0</version>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 33 -->
        <artifactId>grpc-google-analytics-admin-v1alpha</artifactId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 34 -->
        <version>0.21.0</version>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 38 -->
        <artifactId>grpc-google-analytics-admin-v1beta</artifactId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 39 -->
        <version>0.21.0</version>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 43 -->
        <artifactId>proto-google-analytics-admin-v1alpha</artifactId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 44 -->
        <version>0.21.0</version>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 48 -->
        <artifactId>proto-google-analytics-admin-v1beta</artifactId>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 49 -->
        <version>0.21.0</version>  <!-- com.google.analytics:google-analytics-admin-bom:0.21.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.analytics</groupId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 28 -->
        <artifactId>google-analytics-data</artifactId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 29 -->
        <version>0.22.0</version>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 33 -->
        <artifactId>grpc-google-analytics-data-v1beta</artifactId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 34 -->
        <version>0.22.0</version>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 38 -->
        <artifactId>grpc-google-analytics-data-v1alpha</artifactId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 39 -->
        <version>0.22.0</version>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 43 -->
        <artifactId>proto-google-analytics-data-v1beta</artifactId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 44 -->
        <version>0.22.0</version>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 48 -->
        <artifactId>proto-google-analytics-data-v1alpha</artifactId>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 49 -->
        <version>0.22.0</version>  <!-- com.google.analytics:google-analytics-data-bom:0.22.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 28 -->
        <artifactId>google-cloud-analyticshub</artifactId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 29 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 33 -->
        <artifactId>grpc-google-cloud-analyticshub-v1</artifactId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 34 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 38 -->
        <artifactId>proto-google-cloud-analyticshub-v1</artifactId>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 39 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-analyticshub-bom:0.8.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-api-gateway</artifactId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-api-gateway-v1</artifactId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-api-gateway-v1</artifactId>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-api-gateway-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-apigee-connect</artifactId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-apigee-connect-v1</artifactId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-apigee-connect-v1</artifactId>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-connect-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-apigee-registry</artifactId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-apigee-registry-v1</artifactId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 38 -->
        <artifactId>proto-google-cloud-apigee-registry-v1</artifactId>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-apigee-registry-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-apikeys</artifactId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-apikeys-v2</artifactId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-apikeys-v2</artifactId>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-apikeys-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-appengine-admin</artifactId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-appengine-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-appengine-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-appengine-admin-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.area120</groupId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 28 -->
        <artifactId>google-area120-tables</artifactId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 29 -->
        <version>0.15.0</version>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 33 -->
        <artifactId>grpc-google-area120-tables-v1alpha1</artifactId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 38 -->
        <artifactId>proto-google-area120-tables-v1alpha1</artifactId>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 39 -->
        <version>0.15.0</version>  <!-- com.google.area120:google-area120-tables-bom:0.15.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-artifact-registry</artifactId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-artifact-registry-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 34 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 38 -->
        <artifactId>grpc-google-cloud-artifact-registry-v1</artifactId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 43 -->
        <artifactId>proto-google-cloud-artifact-registry-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 44 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 48 -->
        <artifactId>proto-google-cloud-artifact-registry-v1</artifactId>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 49 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-artifact-registry-bom:1.10.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 24 -->
        <artifactId>google-cloud-asset</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 25 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 29 -->
        <artifactId>grpc-google-cloud-asset-v1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 30 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 34 -->
        <artifactId>grpc-google-cloud-asset-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 35 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 39 -->
        <artifactId>grpc-google-cloud-asset-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 40 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 44 -->
        <artifactId>grpc-google-cloud-asset-v1p5beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 45 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 49 -->
        <artifactId>grpc-google-cloud-asset-v1p7beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 50 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 54 -->
        <artifactId>proto-google-cloud-asset-v1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 55 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 59 -->
        <artifactId>proto-google-cloud-asset-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 60 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 64 -->
        <artifactId>proto-google-cloud-asset-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 65 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 69 -->
        <artifactId>proto-google-cloud-asset-v1p5beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 70 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 74 -->
        <artifactId>proto-google-cloud-asset-v1p7beta1</artifactId>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 75 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-asset-bom:3.15.0, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-assured-workloads</artifactId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-assured-workloads-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 34 -->
        <version>0.23.0</version>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 38 -->
        <artifactId>grpc-google-cloud-assured-workloads-v1</artifactId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 43 -->
        <artifactId>proto-google-cloud-assured-workloads-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 44 -->
        <version>0.23.0</version>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 48 -->
        <artifactId>proto-google-cloud-assured-workloads-v1</artifactId>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 49 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-assured-workloads-bom:2.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-automl</artifactId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-automl-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 30 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-automl-v1</artifactId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-automl-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 40 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-automl-v1</artifactId>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-automl-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-bare-metal-solution</artifactId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-bare-metal-solution-v2</artifactId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 38 -->
        <artifactId>proto-google-cloud-bare-metal-solution-v2</artifactId>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-bare-metal-solution-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-batch</artifactId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-batch-v1</artifactId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 38 -->
        <artifactId>grpc-google-cloud-batch-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 43 -->
        <artifactId>proto-google-cloud-batch-v1</artifactId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 44 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 48 -->
        <artifactId>proto-google-cloud-batch-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 49 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-batch-bom:0.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-beyondcorp-appconnections</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-beyondcorp-appconnections-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-beyondcorp-appconnections-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnections-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-beyondcorp-appconnectors</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-beyondcorp-appconnectors-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-beyondcorp-appconnectors-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appconnectors-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-beyondcorp-appgateways</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-beyondcorp-appgateways-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-beyondcorp-appgateways-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-appgateways-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-beyondcorp-clientconnectorservices</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-beyondcorp-clientconnectorservices-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-beyondcorp-clientconnectorservices-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientconnectorservices-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-beyondcorp-clientgateways</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-beyondcorp-clientgateways-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-beyondcorp-clientgateways-v1</artifactId>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-beyondcorp-clientgateways-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 24 -->
        <artifactId>google-cloud-bigqueryconnection</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 25 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 29 -->
        <artifactId>grpc-google-cloud-bigqueryconnection-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 30 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 34 -->
        <artifactId>grpc-google-cloud-bigqueryconnection-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 35 -->
        <version>0.21.0</version>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 39 -->
        <artifactId>proto-google-cloud-bigqueryconnection-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 40 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 44 -->
        <artifactId>proto-google-cloud-bigqueryconnection-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 45 -->
        <version>0.21.0</version>  <!-- com.google.cloud:google-cloud-bigqueryconnection-bom:2.13.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 28 -->
        <artifactId>google-cloud-bigquery-data-exchange</artifactId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 29 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 33 -->
        <artifactId>grpc-google-cloud-bigquery-data-exchange-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 34 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 38 -->
        <artifactId>proto-google-cloud-bigquery-data-exchange-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 39 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-bigquery-data-exchange-bom:2.6.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 28 -->
        <artifactId>google-cloud-bigquerydatapolicy</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 29 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 33 -->
        <artifactId>grpc-google-cloud-bigquerydatapolicy-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 34 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 38 -->
        <artifactId>grpc-google-cloud-bigquerydatapolicy-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 39 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 43 -->
        <artifactId>proto-google-cloud-bigquerydatapolicy-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 44 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 48 -->
        <artifactId>proto-google-cloud-bigquerydatapolicy-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 49 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatapolicy-bom:0.8.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-bigquerydatatransfer</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-bigquerydatatransfer-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 34 -->
        <artifactId>proto-google-cloud-bigquerydatatransfer-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-bigquerydatatransfer-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 28 -->
        <artifactId>google-cloud-bigquerymigration</artifactId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 29 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 33 -->
        <artifactId>grpc-google-cloud-bigquerymigration-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 34 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 38 -->
        <artifactId>grpc-google-cloud-bigquerymigration-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 39 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 43 -->
        <artifactId>proto-google-cloud-bigquerymigration-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 44 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 48 -->
        <artifactId>proto-google-cloud-bigquerymigration-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 49 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-bigquerymigration-bom:0.14.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-bigqueryreservation</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-bigqueryreservation-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 30 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 34 -->
        <artifactId>proto-google-cloud-bigqueryreservation-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 35 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-bigqueryreservation-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-billingbudgets</artifactId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-billingbudgets-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 30 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-billingbudgets-v1</artifactId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-billingbudgets-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 40 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-billingbudgets-v1</artifactId>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billingbudgets-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-billing</artifactId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-billing-v1</artifactId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 34 -->
        <artifactId>proto-google-cloud-billing-v1</artifactId>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-billing-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-binary-authorization</artifactId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-binary-authorization-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 38 -->
        <artifactId>grpc-google-cloud-binary-authorization-v1</artifactId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 43 -->
        <artifactId>proto-google-cloud-binary-authorization-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 44 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 48 -->
        <artifactId>proto-google-cloud-binary-authorization-v1</artifactId>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 49 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-binary-authorization-bom:1.10.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 28 -->
        <artifactId>google-cloud-certificate-manager</artifactId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 29 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 33 -->
        <artifactId>grpc-google-cloud-certificate-manager-v1</artifactId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 34 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 38 -->
        <artifactId>proto-google-cloud-certificate-manager-v1</artifactId>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 39 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-certificate-manager-bom:0.14.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 28 -->
        <artifactId>google-cloud-channel</artifactId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 29 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 33 -->
        <artifactId>grpc-google-cloud-channel-v1</artifactId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 34 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 38 -->
        <artifactId>proto-google-cloud-channel-v1</artifactId>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 39 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-channel-bom:3.15.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 24 -->
        <artifactId>google-cloud-build</artifactId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 25 -->
        <version>3.13.0</version>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 29 -->
        <artifactId>grpc-google-cloud-build-v1</artifactId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 30 -->
        <version>3.13.0</version>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 34 -->
        <artifactId>grpc-google-cloud-build-v2</artifactId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 35 -->
        <version>3.13.0</version>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 39 -->
        <artifactId>proto-google-cloud-build-v1</artifactId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 40 -->
        <version>3.13.0</version>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 44 -->
        <artifactId>proto-google-cloud-build-v2</artifactId>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 45 -->
        <version>3.13.0</version>  <!-- com.google.cloud:google-cloud-build-bom:3.13.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 28 -->
        <artifactId>google-cloud-cloudcommerceconsumerprocurement</artifactId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 29 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 33 -->
        <artifactId>grpc-google-cloud-cloudcommerceconsumerprocurement-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 34 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 38 -->
        <artifactId>proto-google-cloud-cloudcommerceconsumerprocurement-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 39 -->
        <version>0.9.0</version>  <!-- com.google.cloud:google-cloud-cloudcommerceconsumerprocurement-bom:0.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 24 -->
        <artifactId>google-cloud-compute</artifactId>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 25 -->
        <version>1.21.0</version>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 29 -->
        <artifactId>proto-google-cloud-compute-v1</artifactId>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 30 -->
        <version>1.21.0</version>  <!-- com.google.cloud:google-cloud-compute-bom:1.21.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-contact-center-insights</artifactId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-contact-center-insights-v1</artifactId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-contact-center-insights-v1</artifactId>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-contact-center-insights-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-containeranalysis</artifactId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-containeranalysis-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 30 -->
        <version>0.102.0</version>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 34 -->
        <artifactId>grpc-google-cloud-containeranalysis-v1</artifactId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 35 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 39 -->
        <artifactId>proto-google-cloud-containeranalysis-v1</artifactId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 40 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 44 -->
        <artifactId>proto-google-cloud-containeranalysis-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 45 -->
        <version>0.102.0</version>  <!-- com.google.cloud:google-cloud-containeranalysis-bom:2.12.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 24 -->
        <artifactId>google-cloud-container</artifactId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 25 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 29 -->
        <artifactId>grpc-google-cloud-container-v1</artifactId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 30 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 34 -->
        <artifactId>grpc-google-cloud-container-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 35 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 39 -->
        <artifactId>proto-google-cloud-container-v1</artifactId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 40 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 44 -->
        <artifactId>proto-google-cloud-container-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 45 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-container-bom:2.14.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 28 -->
        <artifactId>google-cloud-contentwarehouse</artifactId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 29 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 33 -->
        <artifactId>grpc-google-cloud-contentwarehouse-v1</artifactId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 34 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 38 -->
        <artifactId>proto-google-cloud-contentwarehouse-v1</artifactId>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 39 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-contentwarehouse-bom:0.7.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 24 -->
        <artifactId>google-cloud-datacatalog</artifactId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 25 -->
        <version>1.17.0</version>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 29 -->
        <artifactId>grpc-google-cloud-datacatalog-v1</artifactId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 30 -->
        <version>1.17.0</version>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 34 -->
        <artifactId>grpc-google-cloud-datacatalog-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 35 -->
        <version>0.54.0</version>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 39 -->
        <artifactId>proto-google-cloud-datacatalog-v1</artifactId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 40 -->
        <version>1.17.0</version>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 44 -->
        <artifactId>proto-google-cloud-datacatalog-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 45 -->
        <version>0.54.0</version>  <!-- com.google.cloud:google-cloud-datacatalog-bom:1.17.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 28 -->
        <artifactId>google-cloud-dataflow</artifactId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 29 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 33 -->
        <artifactId>grpc-google-cloud-dataflow-v1beta3</artifactId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 38 -->
        <artifactId>proto-google-cloud-dataflow-v1beta3</artifactId>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 39 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-dataflow-bom:0.15.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 28 -->
        <artifactId>google-cloud-dataform</artifactId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 29 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 33 -->
        <artifactId>grpc-google-cloud-dataform-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 34 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 38 -->
        <artifactId>grpc-google-cloud-dataform-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 39 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 43 -->
        <artifactId>proto-google-cloud-dataform-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 44 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 48 -->
        <artifactId>proto-google-cloud-dataform-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 49 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-dataform-bom:0.10.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-data-fusion</artifactId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-data-fusion-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 38 -->
        <artifactId>grpc-google-cloud-data-fusion-v1</artifactId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 43 -->
        <artifactId>proto-google-cloud-data-fusion-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 44 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 48 -->
        <artifactId>proto-google-cloud-data-fusion-v1</artifactId>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 49 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-data-fusion-bom:1.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 24 -->
        <artifactId>google-cloud-datalabeling</artifactId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 25 -->
        <version>0.131.0</version>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 29 -->
        <artifactId>grpc-google-cloud-datalabeling-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 30 -->
        <version>0.96.0</version>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 34 -->
        <artifactId>proto-google-cloud-datalabeling-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 35 -->
        <version>0.96.0</version>  <!-- com.google.cloud:google-cloud-datalabeling-bom:0.131.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 28 -->
        <artifactId>google-cloud-datalineage</artifactId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 29 -->
        <version>0.3.0</version>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 33 -->
        <artifactId>grpc-google-cloud-datalineage-v1</artifactId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 34 -->
        <version>0.3.0</version>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 38 -->
        <artifactId>proto-google-cloud-datalineage-v1</artifactId>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 39 -->
        <version>0.3.0</version>  <!-- com.google.cloud:google-cloud-datalineage-bom:0.3.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 28 -->
        <artifactId>google-cloud-dataplex</artifactId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 33 -->
        <artifactId>grpc-google-cloud-dataplex-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 34 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 38 -->
        <artifactId>proto-google-cloud-dataplex-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 39 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-dataplex-bom:1.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 24 -->
        <artifactId>google-cloud-dataproc</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 25 -->
        <version>4.8.0</version>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 29 -->
        <artifactId>grpc-google-cloud-dataproc-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 30 -->
        <version>4.8.0</version>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 34 -->
        <artifactId>proto-google-cloud-dataproc-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 35 -->
        <version>4.8.0</version>  <!-- com.google.cloud:google-cloud-dataproc-bom:4.8.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 28 -->
        <artifactId>google-cloud-dataproc-metastore</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 29 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 33 -->
        <artifactId>grpc-google-cloud-dataproc-metastore-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 34 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 38 -->
        <artifactId>grpc-google-cloud-dataproc-metastore-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 39 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 43 -->
        <artifactId>grpc-google-cloud-dataproc-metastore-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 44 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 48 -->
        <artifactId>proto-google-cloud-dataproc-metastore-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 49 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 53 -->
        <artifactId>proto-google-cloud-dataproc-metastore-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 54 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 58 -->
        <artifactId>proto-google-cloud-dataproc-metastore-v1</artifactId>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 59 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-dataproc-metastore-bom:2.12.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-datastream</artifactId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-datastream-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 38 -->
        <artifactId>grpc-google-cloud-datastream-v1</artifactId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 43 -->
        <artifactId>proto-google-cloud-datastream-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 44 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 48 -->
        <artifactId>proto-google-cloud-datastream-v1</artifactId>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 49 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-datastream-bom:1.10.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-debugger-client</artifactId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-debugger-client-v2</artifactId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 38 -->
        <artifactId>proto-google-cloud-debugger-client-v2</artifactId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 43 -->
        <artifactId>proto-google-devtools-source-protos</artifactId>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 44 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-debugger-client-bom:1.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 28 -->
        <artifactId>google-cloud-deploy</artifactId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 33 -->
        <artifactId>grpc-google-cloud-deploy-v1</artifactId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 34 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 38 -->
        <artifactId>proto-google-cloud-deploy-v1</artifactId>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 39 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-deploy-bom:1.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 28 -->
        <artifactId>google-cloud-dialogflow-cx</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 29 -->
        <version>0.22.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 33 -->
        <artifactId>grpc-google-cloud-dialogflow-cx-v3beta1</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 34 -->
        <version>0.22.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 38 -->
        <artifactId>grpc-google-cloud-dialogflow-cx-v3</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 39 -->
        <version>0.22.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 43 -->
        <artifactId>proto-google-cloud-dialogflow-cx-v3beta1</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 44 -->
        <version>0.22.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 48 -->
        <artifactId>proto-google-cloud-dialogflow-cx-v3</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 49 -->
        <version>0.22.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-cx-bom:0.22.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 24 -->
        <artifactId>google-cloud-dialogflow</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 25 -->
        <version>4.17.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 29 -->
        <artifactId>grpc-google-cloud-dialogflow-v2beta1</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 30 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 34 -->
        <artifactId>grpc-google-cloud-dialogflow-v2</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 35 -->
        <version>4.17.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 39 -->
        <artifactId>proto-google-cloud-dialogflow-v2</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 40 -->
        <version>4.17.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 44 -->
        <artifactId>proto-google-cloud-dialogflow-v2beta1</artifactId>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 45 -->
        <version>0.115.0</version>  <!-- com.google.cloud:google-cloud-dialogflow-bom:4.17.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 28 -->
        <artifactId>google-cloud-discoveryengine</artifactId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 29 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 33 -->
        <artifactId>grpc-google-cloud-discoveryengine-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 34 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 38 -->
        <artifactId>proto-google-cloud-discoveryengine-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 39 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-discoveryengine-bom:0.7.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 28 -->
        <artifactId>google-cloud-distributedcloudedge</artifactId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 29 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 33 -->
        <artifactId>grpc-google-cloud-distributedcloudedge-v1</artifactId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 34 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 38 -->
        <artifactId>proto-google-cloud-distributedcloudedge-v1</artifactId>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 39 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-distributedcloudedge-bom:0.8.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 24 -->
        <artifactId>google-cloud-dlp</artifactId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 25 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 29 -->
        <artifactId>grpc-google-cloud-dlp-v2</artifactId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 30 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 34 -->
        <artifactId>proto-google-cloud-dlp-v2</artifactId>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 35 -->
        <version>3.15.0</version>  <!-- com.google.cloud:google-cloud-dlp-bom:3.15.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 28 -->
        <artifactId>google-cloud-dms</artifactId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 29 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 33 -->
        <artifactId>grpc-google-cloud-dms-v1</artifactId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 34 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 38 -->
        <artifactId>proto-google-cloud-dms-v1</artifactId>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 39 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-dms-bom:2.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 22 -->
        <artifactId>google-cloud-document-ai</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 23 -->
        <version>2.15.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 24 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 27 -->
        <artifactId>grpc-google-cloud-document-ai-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 28 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 29 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 32 -->
        <artifactId>grpc-google-cloud-document-ai-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 33 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 34 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 37 -->
        <artifactId>grpc-google-cloud-document-ai-v1beta3</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 38 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 39 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 42 -->
        <artifactId>grpc-google-cloud-document-ai-v1</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 43 -->
        <version>2.15.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 44 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 47 -->
        <artifactId>proto-google-cloud-document-ai-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 48 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 49 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 52 -->
        <artifactId>proto-google-cloud-document-ai-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 53 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 54 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 57 -->
        <artifactId>proto-google-cloud-document-ai-v1beta3</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 58 -->
        <version>0.27.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 59 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 62 -->
        <artifactId>proto-google-cloud-document-ai-v1</artifactId>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 63 -->
        <version>2.15.0</version>  <!-- com.google.cloud:google-cloud-document-ai-bom:2.15.0, line 64 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 28 -->
        <artifactId>google-cloud-domains</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 29 -->
        <version>1.8.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 33 -->
        <artifactId>grpc-google-cloud-domains-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 34 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 38 -->
        <artifactId>grpc-google-cloud-domains-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 39 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 43 -->
        <artifactId>grpc-google-cloud-domains-v1</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 44 -->
        <version>1.8.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 48 -->
        <artifactId>proto-google-cloud-domains-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 49 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 53 -->
        <artifactId>proto-google-cloud-domains-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 54 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 58 -->
        <artifactId>proto-google-cloud-domains-v1</artifactId>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 59 -->
        <version>1.8.0</version>  <!-- com.google.cloud:google-cloud-domains-bom:1.8.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 28 -->
        <artifactId>google-cloud-enterpriseknowledgegraph</artifactId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 29 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 33 -->
        <artifactId>grpc-google-cloud-enterpriseknowledgegraph-v1</artifactId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 34 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 38 -->
        <artifactId>proto-google-cloud-enterpriseknowledgegraph-v1</artifactId>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 39 -->
        <version>0.7.0</version>  <!-- com.google.cloud:google-cloud-enterpriseknowledgegraph-bom:0.7.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 24 -->
        <artifactId>google-cloud-errorreporting</artifactId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 25 -->
        <version>0.132.0-beta</version>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 29 -->
        <artifactId>grpc-google-cloud-error-reporting-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 30 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 34 -->
        <artifactId>proto-google-cloud-error-reporting-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 35 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-errorreporting-bom:0.132.0-beta, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-essential-contacts</artifactId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-essential-contacts-v1</artifactId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-essential-contacts-v1</artifactId>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-essential-contacts-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-eventarc</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-eventarc-v1</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 38 -->
        <artifactId>proto-google-cloud-eventarc-v1</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-eventarc-publishing</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-eventarc-publishing-v1</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 38 -->
        <artifactId>proto-google-cloud-eventarc-publishing-v1</artifactId>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-eventarc-publishing-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 28 -->
        <artifactId>google-cloud-filestore</artifactId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 29 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 33 -->
        <artifactId>grpc-google-cloud-filestore-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 34 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 38 -->
        <artifactId>grpc-google-cloud-filestore-v1</artifactId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 39 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 43 -->
        <artifactId>proto-google-cloud-filestore-v1</artifactId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 44 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 48 -->
        <artifactId>proto-google-cloud-filestore-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 49 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-filestore-bom:1.12.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 28 -->
        <artifactId>google-cloud-functions</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 29 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 33 -->
        <artifactId>grpc-google-cloud-functions-v1</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 34 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 38 -->
        <artifactId>grpc-google-cloud-functions-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 39 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 43 -->
        <artifactId>grpc-google-cloud-functions-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 44 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 48 -->
        <artifactId>grpc-google-cloud-functions-v2</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 49 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 53 -->
        <artifactId>proto-google-cloud-functions-v1</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 54 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 58 -->
        <artifactId>proto-google-cloud-functions-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 59 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 63 -->
        <artifactId>proto-google-cloud-functions-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 64 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 68 -->
        <artifactId>proto-google-cloud-functions-v2</artifactId>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 69 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-functions-bom:2.13.0, line 70 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-game-servers</artifactId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-game-servers-v1</artifactId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-game-servers-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 35 -->
        <version>0.36.0</version>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-game-servers-v1</artifactId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 40 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-game-servers-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 45 -->
        <version>0.36.0</version>  <!-- com.google.cloud:google-cloud-game-servers-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 28 -->
        <artifactId>google-cloud-gke-backup</artifactId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 29 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 33 -->
        <artifactId>grpc-google-cloud-gke-backup-v1</artifactId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 34 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 38 -->
        <artifactId>proto-google-cloud-gke-backup-v1</artifactId>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 39 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-backup-bom:0.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 28 -->
        <artifactId>google-cloud-gke-connect-gateway</artifactId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 29 -->
        <version>0.12.0</version>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 33 -->
        <artifactId>grpc-google-cloud-gke-connect-gateway-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 34 -->
        <version>0.12.0</version>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 38 -->
        <artifactId>proto-google-cloud-gke-connect-gateway-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 39 -->
        <version>0.12.0</version>  <!-- com.google.cloud:google-cloud-gke-connect-gateway-bom:0.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-gkehub</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-gkehub-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 34 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 38 -->
        <artifactId>grpc-google-cloud-gkehub-v1</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 43 -->
        <artifactId>grpc-google-cloud-gkehub-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 44 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 48 -->
        <artifactId>grpc-google-cloud-gkehub-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 49 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 53 -->
        <artifactId>grpc-google-cloud-gkehub-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 54 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 58 -->
        <artifactId>proto-google-cloud-gkehub-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 59 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 63 -->
        <artifactId>proto-google-cloud-gkehub-v1</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 64 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 68 -->
        <artifactId>proto-google-cloud-gkehub-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 69 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 70 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 73 -->
        <artifactId>proto-google-cloud-gkehub-v1alpha2</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 74 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 75 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 78 -->
        <artifactId>proto-google-cloud-gkehub-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 79 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-gkehub-bom:1.11.0, line 80 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 28 -->
        <artifactId>google-cloud-gke-multi-cloud</artifactId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 29 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 33 -->
        <artifactId>grpc-google-cloud-gke-multi-cloud-v1</artifactId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 34 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 38 -->
        <artifactId>proto-google-cloud-gke-multi-cloud-v1</artifactId>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 39 -->
        <version>0.10.0</version>  <!-- com.google.cloud:google-cloud-gke-multi-cloud-bom:0.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-gsuite-addons</artifactId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-gsuite-addons-v1</artifactId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-gsuite-addons-v1</artifactId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 43 -->
        <artifactId>proto-google-apps-script-type-protos</artifactId>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 44 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-gsuite-addons-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 28 -->
        <artifactId>google-iam-admin</artifactId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 29 -->
        <version>3.6.0</version>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 33 -->
        <artifactId>grpc-google-iam-admin-v1</artifactId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 34 -->
        <version>3.6.0</version>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 38 -->
        <artifactId>proto-google-iam-admin-v1</artifactId>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 39 -->
        <version>3.6.0</version>  <!-- com.google.cloud:google-iam-admin-bom:3.6.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-iamcredentials</artifactId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-iamcredentials-v1</artifactId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 34 -->
        <artifactId>proto-google-cloud-iamcredentials-v1</artifactId>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iamcredentials-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-iam-policy-bom:1.9.0, line 28 -->
        <artifactId>google-iam-policy</artifactId>  <!-- com.google.cloud:google-iam-policy-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-iam-policy-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-ids</artifactId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-ids-v1</artifactId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 34 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 38 -->
        <artifactId>proto-google-cloud-ids-v1</artifactId>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-ids-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-iot</artifactId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-iot-v1</artifactId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 34 -->
        <artifactId>proto-google-cloud-iot-v1</artifactId>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-iot-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 24 -->
        <artifactId>google-cloud-kms</artifactId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 25 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 29 -->
        <artifactId>grpc-google-cloud-kms-v1</artifactId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 30 -->
        <version>0.105.0</version>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 34 -->
        <artifactId>proto-google-cloud-kms-v1</artifactId>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 35 -->
        <version>0.105.0</version>  <!-- com.google.cloud:google-cloud-kms-bom:2.14.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-language</artifactId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-language-v1</artifactId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 30 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 34 -->
        <artifactId>grpc-google-cloud-language-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 35 -->
        <version>0.99.0</version>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 39 -->
        <artifactId>proto-google-cloud-language-v1</artifactId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 40 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 44 -->
        <artifactId>proto-google-cloud-language-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 45 -->
        <version>0.99.0</version>  <!-- com.google.cloud:google-cloud-language-bom:2.12.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 28 -->
        <artifactId>google-cloud-life-sciences</artifactId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 29 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 33 -->
        <artifactId>grpc-google-cloud-life-sciences-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 34 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 38 -->
        <artifactId>proto-google-cloud-life-sciences-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 39 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-life-sciences-bom:0.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 28 -->
        <artifactId>google-cloud-managed-identities</artifactId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 33 -->
        <artifactId>grpc-google-cloud-managed-identities-v1</artifactId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 34 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 38 -->
        <artifactId>proto-google-cloud-managed-identities-v1</artifactId>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 39 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-managed-identities-bom:1.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 24 -->
        <artifactId>google-cloud-mediatranslation</artifactId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 25 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 29 -->
        <artifactId>grpc-google-cloud-mediatranslation-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 30 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 34 -->
        <artifactId>proto-google-cloud-mediatranslation-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 35 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-mediatranslation-bom:0.17.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-memcache</artifactId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-memcache-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 34 -->
        <version>0.18.0</version>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 38 -->
        <artifactId>grpc-google-cloud-memcache-v1</artifactId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 43 -->
        <artifactId>proto-google-cloud-memcache-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 44 -->
        <version>0.18.0</version>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 48 -->
        <artifactId>proto-google-cloud-memcache-v1</artifactId>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 49 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-memcache-bom:2.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 24 -->
        <artifactId>google-cloud-monitoring-dashboard</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 25 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 29 -->
        <artifactId>grpc-google-cloud-monitoring-dashboard-v1</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 30 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 34 -->
        <artifactId>proto-google-cloud-monitoring-dashboard-v1</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 35 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-monitoring-dashboard-bom:2.13.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 24 -->
        <artifactId>google-cloud-monitoring</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 25 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 29 -->
        <artifactId>grpc-google-cloud-monitoring-v3</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 30 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 34 -->
        <artifactId>proto-google-cloud-monitoring-v3</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 35 -->
        <version>3.12.0</version>  <!-- com.google.cloud:google-cloud-monitoring-bom:3.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 28 -->
        <artifactId>google-cloud-monitoring-metricsscope</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 29 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 33 -->
        <artifactId>grpc-google-cloud-monitoring-metricsscope-v1</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 34 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 38 -->
        <artifactId>proto-google-cloud-monitoring-metricsscope-v1</artifactId>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 39 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-monitoring-metricsscope-bom:0.5.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-networkconnectivity</artifactId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-networkconnectivity-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 34 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 38 -->
        <artifactId>grpc-google-cloud-networkconnectivity-v1</artifactId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 43 -->
        <artifactId>proto-google-cloud-networkconnectivity-v1alpha1</artifactId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 44 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 48 -->
        <artifactId>proto-google-cloud-networkconnectivity-v1</artifactId>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 49 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-networkconnectivity-bom:1.10.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 28 -->
        <artifactId>google-cloud-network-management</artifactId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 29 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 33 -->
        <artifactId>grpc-google-cloud-network-management-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 34 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 38 -->
        <artifactId>grpc-google-cloud-network-management-v1</artifactId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 39 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 43 -->
        <artifactId>proto-google-cloud-network-management-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 44 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 48 -->
        <artifactId>proto-google-cloud-network-management-v1</artifactId>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 49 -->
        <version>1.12.0</version>  <!-- com.google.cloud:google-cloud-network-management-bom:1.12.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 28 -->
        <artifactId>google-cloud-network-security</artifactId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 29 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 33 -->
        <artifactId>grpc-google-cloud-network-security-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 34 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 38 -->
        <artifactId>grpc-google-cloud-network-security-v1</artifactId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 39 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 43 -->
        <artifactId>proto-google-cloud-network-security-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 44 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 48 -->
        <artifactId>proto-google-cloud-network-security-v1</artifactId>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 49 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-network-security-bom:0.14.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 28 -->
        <artifactId>google-cloud-notebooks</artifactId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 33 -->
        <artifactId>grpc-google-cloud-notebooks-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 34 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 38 -->
        <artifactId>grpc-google-cloud-notebooks-v1</artifactId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 39 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 43 -->
        <artifactId>proto-google-cloud-notebooks-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 44 -->
        <version>0.16.0</version>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 48 -->
        <artifactId>proto-google-cloud-notebooks-v1</artifactId>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 49 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-notebooks-bom:1.9.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 28 -->
        <artifactId>google-cloud-optimization</artifactId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 29 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 33 -->
        <artifactId>grpc-google-cloud-optimization-v1</artifactId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 34 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 38 -->
        <artifactId>proto-google-cloud-optimization-v1</artifactId>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 39 -->
        <version>1.9.0</version>  <!-- com.google.cloud:google-cloud-optimization-bom:1.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-orchestration-airflow</artifactId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-orchestration-airflow-v1</artifactId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 38 -->
        <artifactId>grpc-google-cloud-orchestration-airflow-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 39 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 43 -->
        <artifactId>proto-google-cloud-orchestration-airflow-v1</artifactId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 44 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 48 -->
        <artifactId>proto-google-cloud-orchestration-airflow-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 49 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-orchestration-airflow-bom:1.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 27 -->
        <artifactId>google-cloud-orgpolicy</artifactId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 28 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 29 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 32 -->
        <artifactId>grpc-google-cloud-orgpolicy-v2</artifactId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 33 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 34 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 37 -->
        <artifactId>proto-google-cloud-orgpolicy-v1</artifactId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 38 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 39 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 42 -->
        <artifactId>proto-google-cloud-orgpolicy-v2</artifactId>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 43 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-orgpolicy-bom:2.11.0, line 44 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 24 -->
        <artifactId>google-cloud-os-config</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 25 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 29 -->
        <artifactId>grpc-google-cloud-os-config-v1</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 30 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 34 -->
        <artifactId>grpc-google-cloud-os-config-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 35 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 39 -->
        <artifactId>grpc-google-cloud-os-config-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 40 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 44 -->
        <artifactId>proto-google-cloud-os-config-v1</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 45 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 49 -->
        <artifactId>proto-google-cloud-os-config-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 50 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 54 -->
        <artifactId>proto-google-cloud-os-config-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 55 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-os-config-bom:2.13.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 24 -->
        <artifactId>google-cloud-os-login</artifactId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 25 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 29 -->
        <artifactId>grpc-google-cloud-os-login-v1</artifactId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 30 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 34 -->
        <artifactId>proto-google-cloud-os-login-v1</artifactId>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 35 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-os-login-bom:2.10.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 24 -->
        <artifactId>google-cloud-phishingprotection</artifactId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 25 -->
        <version>0.42.0</version>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 29 -->
        <artifactId>grpc-google-cloud-phishingprotection-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 30 -->
        <version>0.42.0</version>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 34 -->
        <artifactId>proto-google-cloud-phishingprotection-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 35 -->
        <version>0.42.0</version>  <!-- com.google.cloud:google-cloud-phishingprotection-bom:0.42.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-policy-troubleshooter</artifactId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-policy-troubleshooter-v1</artifactId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 34 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 38 -->
        <artifactId>proto-google-cloud-policy-troubleshooter-v1</artifactId>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-policy-troubleshooter-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 28 -->
        <artifactId>google-cloud-private-catalog</artifactId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 29 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 33 -->
        <artifactId>grpc-google-cloud-private-catalog-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 34 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 38 -->
        <artifactId>proto-google-cloud-private-catalog-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 39 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-private-catalog-bom:0.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-profiler</artifactId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-profiler-v2</artifactId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 34 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 38 -->
        <artifactId>proto-google-cloud-profiler-v2</artifactId>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-profiler-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 28 -->
        <artifactId>google-cloud-publicca</artifactId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 29 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 33 -->
        <artifactId>grpc-google-cloud-publicca-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 34 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 38 -->
        <artifactId>proto-google-cloud-publicca-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 39 -->
        <version>0.8.0</version>  <!-- com.google.cloud:google-cloud-publicca-bom:0.8.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 24 -->
        <artifactId>google-cloud-recaptchaenterprise</artifactId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 25 -->
        <version>3.8.0</version>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 29 -->
        <artifactId>grpc-google-cloud-recaptchaenterprise-v1</artifactId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 30 -->
        <version>3.8.0</version>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 34 -->
        <artifactId>grpc-google-cloud-recaptchaenterprise-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 35 -->
        <version>0.50.0</version>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 39 -->
        <artifactId>proto-google-cloud-recaptchaenterprise-v1</artifactId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 40 -->
        <version>3.8.0</version>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 44 -->
        <artifactId>proto-google-cloud-recaptchaenterprise-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 45 -->
        <version>0.50.0</version>  <!-- com.google.cloud:google-cloud-recaptchaenterprise-bom:3.8.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 24 -->
        <artifactId>google-cloud-recommendations-ai</artifactId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 25 -->
        <version>0.18.0</version>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 29 -->
        <artifactId>grpc-google-cloud-recommendations-ai-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 30 -->
        <version>0.18.0</version>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 34 -->
        <artifactId>proto-google-cloud-recommendations-ai-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 35 -->
        <version>0.18.0</version>  <!-- com.google.cloud:google-cloud-recommendations-ai-bom:0.18.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 24 -->
        <artifactId>google-cloud-recommender</artifactId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 25 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 29 -->
        <artifactId>grpc-google-cloud-recommender-v1</artifactId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 30 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 34 -->
        <artifactId>grpc-google-cloud-recommender-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 35 -->
        <version>0.25.0</version>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 39 -->
        <artifactId>proto-google-cloud-recommender-v1</artifactId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 40 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 44 -->
        <artifactId>proto-google-cloud-recommender-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 45 -->
        <version>0.25.0</version>  <!-- com.google.cloud:google-cloud-recommender-bom:2.13.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 24 -->
        <artifactId>google-cloud-redis</artifactId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 25 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 29 -->
        <artifactId>grpc-google-cloud-redis-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 30 -->
        <version>0.102.0</version>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 34 -->
        <artifactId>grpc-google-cloud-redis-v1</artifactId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 35 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 39 -->
        <artifactId>proto-google-cloud-redis-v1</artifactId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 40 -->
        <version>2.14.0</version>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 44 -->
        <artifactId>proto-google-cloud-redis-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 45 -->
        <version>0.102.0</version>  <!-- com.google.cloud:google-cloud-redis-bom:2.14.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 28 -->
        <artifactId>google-cloud-resourcemanager</artifactId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 29 -->
        <version>1.13.0</version>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 33 -->
        <artifactId>grpc-google-cloud-resourcemanager-v3</artifactId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 34 -->
        <version>1.13.0</version>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 38 -->
        <artifactId>proto-google-cloud-resourcemanager-v3</artifactId>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 39 -->
        <version>1.13.0</version>  <!-- com.google.cloud:google-cloud-resourcemanager-bom:1.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-resource-settings</artifactId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-resource-settings-v1</artifactId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 38 -->
        <artifactId>proto-google-cloud-resource-settings-v1</artifactId>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-resource-settings-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 28 -->
        <artifactId>google-cloud-retail</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 29 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 33 -->
        <artifactId>grpc-google-cloud-retail-v2</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 34 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 38 -->
        <artifactId>grpc-google-cloud-retail-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 39 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 43 -->
        <artifactId>grpc-google-cloud-retail-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 44 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 48 -->
        <artifactId>proto-google-cloud-retail-v2</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 49 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 53 -->
        <artifactId>proto-google-cloud-retail-v2alpha</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 54 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 58 -->
        <artifactId>proto-google-cloud-retail-v2beta</artifactId>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 59 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-retail-bom:2.13.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-run</artifactId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-run-v2</artifactId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 38 -->
        <artifactId>proto-google-cloud-run-v2</artifactId>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-run-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-scheduler</artifactId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-scheduler-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 30 -->
        <version>0.96.0</version>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-scheduler-v1</artifactId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-scheduler-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 40 -->
        <version>0.96.0</version>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-scheduler-v1</artifactId>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-scheduler-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-secretmanager</artifactId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-secretmanager-v1</artifactId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-secretmanager-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-secretmanager-v1</artifactId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 40 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-secretmanager-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-secretmanager-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 24 -->
        <artifactId>google-cloud-securitycenter</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 25 -->
        <version>2.19.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 29 -->
        <artifactId>grpc-google-cloud-securitycenter-v1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 30 -->
        <version>2.19.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 34 -->
        <artifactId>grpc-google-cloud-securitycenter-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 35 -->
        <version>0.114.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 39 -->
        <artifactId>grpc-google-cloud-securitycenter-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 40 -->
        <version>0.114.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 44 -->
        <artifactId>proto-google-cloud-securitycenter-v1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 45 -->
        <version>2.19.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 49 -->
        <artifactId>proto-google-cloud-securitycenter-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 50 -->
        <version>0.114.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 54 -->
        <artifactId>proto-google-cloud-securitycenter-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 55 -->
        <version>0.114.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-bom:2.19.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 24 -->
        <artifactId>google-cloud-securitycenter-settings</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 25 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 29 -->
        <artifactId>grpc-google-cloud-securitycenter-settings-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 30 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 34 -->
        <artifactId>proto-google-cloud-securitycenter-settings-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 35 -->
        <version>0.14.0</version>  <!-- com.google.cloud:google-cloud-securitycenter-settings-bom:0.14.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 28 -->
        <artifactId>google-cloud-security-private-ca</artifactId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 29 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 33 -->
        <artifactId>grpc-google-cloud-security-private-ca-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 34 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 38 -->
        <artifactId>grpc-google-cloud-security-private-ca-v1</artifactId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 39 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 43 -->
        <artifactId>proto-google-cloud-security-private-ca-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 44 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 48 -->
        <artifactId>proto-google-cloud-security-private-ca-v1</artifactId>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 49 -->
        <version>2.13.0</version>  <!-- com.google.cloud:google-cloud-security-private-ca-bom:2.13.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-service-control</artifactId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-service-control-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 38 -->
        <artifactId>grpc-google-cloud-service-control-v2</artifactId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 43 -->
        <artifactId>proto-google-cloud-service-control-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 44 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 48 -->
        <artifactId>proto-google-cloud-service-control-v2</artifactId>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 49 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-service-control-bom:1.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-servicedirectory</artifactId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-servicedirectory-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 30 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 34 -->
        <artifactId>grpc-google-cloud-servicedirectory-v1</artifactId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 35 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 39 -->
        <artifactId>proto-google-cloud-servicedirectory-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 40 -->
        <version>0.20.0</version>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 44 -->
        <artifactId>proto-google-cloud-servicedirectory-v1</artifactId>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 45 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-servicedirectory-bom:2.12.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 28 -->
        <artifactId>google-cloud-service-management</artifactId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 29 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 33 -->
        <artifactId>grpc-google-cloud-service-management-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 34 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 38 -->
        <artifactId>proto-google-cloud-service-management-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 39 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-service-management-bom:3.9.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-service-usage</artifactId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-service-usage-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 38 -->
        <artifactId>grpc-google-cloud-service-usage-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 43 -->
        <artifactId>proto-google-cloud-service-usage-v1</artifactId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 44 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 48 -->
        <artifactId>proto-google-cloud-service-usage-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 49 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-service-usage-bom:2.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 28 -->
        <artifactId>google-cloud-shell</artifactId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 29 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 33 -->
        <artifactId>grpc-google-cloud-shell-v1</artifactId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 34 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 38 -->
        <artifactId>proto-google-cloud-shell-v1</artifactId>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 39 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-shell-bom:2.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 24 -->
        <artifactId>google-cloud-speech</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 25 -->
        <version>4.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 29 -->
        <artifactId>grpc-google-cloud-speech-v1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 30 -->
        <version>4.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 34 -->
        <artifactId>grpc-google-cloud-speech-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 35 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 39 -->
        <artifactId>grpc-google-cloud-speech-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 40 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 44 -->
        <artifactId>grpc-google-cloud-speech-v2</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 45 -->
        <version>4.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 49 -->
        <artifactId>proto-google-cloud-speech-v1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 50 -->
        <version>4.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 54 -->
        <artifactId>proto-google-cloud-speech-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 55 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 59 -->
        <artifactId>proto-google-cloud-speech-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 60 -->
        <version>2.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 64 -->
        <artifactId>proto-google-cloud-speech-v2</artifactId>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 65 -->
        <version>4.6.0</version>  <!-- com.google.cloud:google-cloud-speech-bom:4.6.0, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-storage-transfer</artifactId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-storage-transfer-v1</artifactId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 38 -->
        <artifactId>proto-google-cloud-storage-transfer-v1</artifactId>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-storage-transfer-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-talent</artifactId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-talent-v4</artifactId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 30 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 34 -->
        <artifactId>grpc-google-cloud-talent-v4beta1</artifactId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 35 -->
        <version>0.55.0</version>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 39 -->
        <artifactId>proto-google-cloud-talent-v4</artifactId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 40 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 44 -->
        <artifactId>proto-google-cloud-talent-v4beta1</artifactId>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 45 -->
        <version>0.55.0</version>  <!-- com.google.cloud:google-cloud-talent-bom:2.12.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-tasks</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-tasks-v2beta3</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 30 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-tasks-v2beta2</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 35 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 39 -->
        <artifactId>grpc-google-cloud-tasks-v2</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 40 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-tasks-v2beta3</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 45 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 49 -->
        <artifactId>proto-google-cloud-tasks-v2beta2</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 50 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 54 -->
        <artifactId>proto-google-cloud-tasks-v2</artifactId>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 55 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-tasks-bom:2.11.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 24 -->
        <artifactId>google-cloud-texttospeech</artifactId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 25 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 29 -->
        <artifactId>grpc-google-cloud-texttospeech-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 30 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 34 -->
        <artifactId>grpc-google-cloud-texttospeech-v1</artifactId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 35 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 39 -->
        <artifactId>proto-google-cloud-texttospeech-v1</artifactId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 40 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 44 -->
        <artifactId>proto-google-cloud-texttospeech-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 45 -->
        <version>0.101.0</version>  <!-- com.google.cloud:google-cloud-texttospeech-bom:2.12.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 28 -->
        <artifactId>google-cloud-tpu</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 29 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 33 -->
        <artifactId>grpc-google-cloud-tpu-v1</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 34 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 38 -->
        <artifactId>grpc-google-cloud-tpu-v2alpha1</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 39 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 43 -->
        <artifactId>grpc-google-cloud-tpu-v2</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 44 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 48 -->
        <artifactId>proto-google-cloud-tpu-v1</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 49 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 53 -->
        <artifactId>proto-google-cloud-tpu-v2alpha1</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 54 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 58 -->
        <artifactId>proto-google-cloud-tpu-v2</artifactId>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 59 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-tpu-bom:2.12.0, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-trace</artifactId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-trace-v1</artifactId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 30 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-trace-v2</artifactId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-trace-v1</artifactId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 40 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-trace-v2</artifactId>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-trace-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-translate</artifactId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-translate-v3beta1</artifactId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 30 -->
        <version>0.93.0</version>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-translate-v3</artifactId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 35 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 39 -->
        <artifactId>proto-google-cloud-translate-v3beta1</artifactId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 40 -->
        <version>0.93.0</version>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-translate-v3</artifactId>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 45 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-translate-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 24 -->
        <artifactId>google-cloud-video-intelligence</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 25 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 29 -->
        <artifactId>grpc-google-cloud-video-intelligence-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 30 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 34 -->
        <artifactId>grpc-google-cloud-video-intelligence-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 35 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 39 -->
        <artifactId>grpc-google-cloud-video-intelligence-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 40 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 44 -->
        <artifactId>grpc-google-cloud-video-intelligence-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 45 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 49 -->
        <artifactId>grpc-google-cloud-video-intelligence-v1p3beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 50 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 54 -->
        <artifactId>proto-google-cloud-video-intelligence-v1p3beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 55 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 59 -->
        <artifactId>proto-google-cloud-video-intelligence-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 60 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 64 -->
        <artifactId>proto-google-cloud-video-intelligence-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 65 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 69 -->
        <artifactId>proto-google-cloud-video-intelligence-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 70 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 74 -->
        <artifactId>proto-google-cloud-video-intelligence-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 75 -->
        <version>0.100.0</version>  <!-- com.google.cloud:google-cloud-video-intelligence-bom:2.10.0, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 28 -->
        <artifactId>google-cloud-live-stream</artifactId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 29 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 33 -->
        <artifactId>grpc-google-cloud-live-stream-v1</artifactId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 34 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 38 -->
        <artifactId>proto-google-cloud-live-stream-v1</artifactId>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 39 -->
        <version>0.13.0</version>  <!-- com.google.cloud:google-cloud-live-stream-bom:0.13.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 28 -->
        <artifactId>google-cloud-video-stitcher</artifactId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 29 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 33 -->
        <artifactId>grpc-google-cloud-video-stitcher-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 34 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 38 -->
        <artifactId>proto-google-cloud-video-stitcher-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 39 -->
        <version>0.11.0</version>  <!-- com.google.cloud:google-cloud-video-stitcher-bom:0.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 28 -->
        <artifactId>google-cloud-video-transcoder</artifactId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 29 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 33 -->
        <artifactId>grpc-google-cloud-video-transcoder-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 34 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 38 -->
        <artifactId>proto-google-cloud-video-transcoder-v1</artifactId>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 39 -->
        <version>1.10.0</version>  <!-- com.google.cloud:google-cloud-video-transcoder-bom:1.10.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 24 -->
        <artifactId>google-cloud-vision</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 25 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 29 -->
        <artifactId>grpc-google-cloud-vision-v1p3beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 30 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 34 -->
        <artifactId>grpc-google-cloud-vision-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 35 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 39 -->
        <artifactId>grpc-google-cloud-vision-v1p4beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 40 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 44 -->
        <artifactId>grpc-google-cloud-vision-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 45 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 49 -->
        <artifactId>grpc-google-cloud-vision-v1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 50 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 54 -->
        <artifactId>proto-google-cloud-vision-v1p4beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 55 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 59 -->
        <artifactId>proto-google-cloud-vision-v1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 60 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 64 -->
        <artifactId>proto-google-cloud-vision-v1p1beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 65 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 69 -->
        <artifactId>proto-google-cloud-vision-v1p3beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 70 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 74 -->
        <artifactId>proto-google-cloud-vision-v1p2beta1</artifactId>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 75 -->
        <version>3.9.0</version>  <!-- com.google.cloud:google-cloud-vision-bom:3.9.0, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 28 -->
        <artifactId>google-cloud-vmmigration</artifactId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 29 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 33 -->
        <artifactId>grpc-google-cloud-vmmigration-v1</artifactId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 34 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 38 -->
        <artifactId>proto-google-cloud-vmmigration-v1</artifactId>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 39 -->
        <version>1.11.0</version>  <!-- com.google.cloud:google-cloud-vmmigration-bom:1.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 28 -->
        <artifactId>google-cloud-vmwareengine</artifactId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 29 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 33 -->
        <artifactId>grpc-google-cloud-vmwareengine-v1</artifactId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 34 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 38 -->
        <artifactId>proto-google-cloud-vmwareengine-v1</artifactId>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 39 -->
        <version>0.5.0</version>  <!-- com.google.cloud:google-cloud-vmwareengine-bom:0.5.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 28 -->
        <artifactId>google-cloud-vpcaccess</artifactId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 29 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 33 -->
        <artifactId>grpc-google-cloud-vpcaccess-v1</artifactId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 34 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 38 -->
        <artifactId>proto-google-cloud-vpcaccess-v1</artifactId>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 39 -->
        <version>2.12.0</version>  <!-- com.google.cloud:google-cloud-vpcaccess-bom:2.12.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 24 -->
        <artifactId>google-cloud-webrisk</artifactId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 25 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 29 -->
        <artifactId>grpc-google-cloud-webrisk-v1</artifactId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 30 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 34 -->
        <artifactId>grpc-google-cloud-webrisk-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 35 -->
        <version>0.47.0</version>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 39 -->
        <artifactId>proto-google-cloud-webrisk-v1</artifactId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 40 -->
        <version>2.10.0</version>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 44 -->
        <artifactId>proto-google-cloud-webrisk-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 45 -->
        <version>0.47.0</version>  <!-- com.google.cloud:google-cloud-webrisk-bom:2.10.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 24 -->
        <artifactId>google-cloud-websecurityscanner</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 25 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 26 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 29 -->
        <artifactId>grpc-google-cloud-websecurityscanner-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 30 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 31 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 34 -->
        <artifactId>grpc-google-cloud-websecurityscanner-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 35 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 36 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 39 -->
        <artifactId>grpc-google-cloud-websecurityscanner-v1</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 40 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 41 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 44 -->
        <artifactId>proto-google-cloud-websecurityscanner-v1alpha</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 45 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 46 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 49 -->
        <artifactId>proto-google-cloud-websecurityscanner-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 50 -->
        <version>0.98.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 51 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 54 -->
        <artifactId>proto-google-cloud-websecurityscanner-v1</artifactId>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 55 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-websecurityscanner-bom:2.11.0, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-workflow-executions</artifactId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-workflow-executions-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 34 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 38 -->
        <artifactId>grpc-google-cloud-workflow-executions-v1</artifactId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 43 -->
        <artifactId>proto-google-cloud-workflow-executions-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 44 -->
        <version>0.15.0</version>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 48 -->
        <artifactId>proto-google-cloud-workflow-executions-v1</artifactId>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 49 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflow-executions-bom:2.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 28 -->
        <artifactId>google-cloud-workflows</artifactId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 29 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 30 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 33 -->
        <artifactId>grpc-google-cloud-workflows-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 34 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 35 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 38 -->
        <artifactId>grpc-google-cloud-workflows-v1</artifactId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 39 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 40 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 43 -->
        <artifactId>proto-google-cloud-workflows-v1beta</artifactId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 44 -->
        <version>0.17.0</version>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 45 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 48 -->
        <artifactId>proto-google-cloud-workflows-v1</artifactId>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 49 -->
        <version>2.11.0</version>  <!-- com.google.cloud:google-cloud-workflows-bom:2.11.0, line 50 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 53 -->
        <artifactId>google-cloud-bigquerystorage</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 54 -->
        <version>2.32.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 58 -->
        <artifactId>grpc-google-cloud-bigquerystorage-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 59 -->
        <version>0.156.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 63 -->
        <artifactId>grpc-google-cloud-bigquerystorage-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 64 -->
        <version>0.156.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 68 -->
        <artifactId>grpc-google-cloud-bigquerystorage-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 69 -->
        <version>2.32.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 70 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 73 -->
        <artifactId>proto-google-cloud-bigquerystorage-v1beta1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 74 -->
        <version>0.156.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 75 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 78 -->
        <artifactId>proto-google-cloud-bigquerystorage-v1beta2</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 79 -->
        <version>0.156.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 80 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 83 -->
        <artifactId>proto-google-cloud-bigquerystorage-v1</artifactId>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 84 -->
        <version>2.32.1</version>  <!-- com.google.cloud:google-cloud-bigquerystorage-bom:2.32.1, line 85 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 64 -->
        <artifactId>google-cloud-bigtable</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 65 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 69 -->
        <artifactId>google-cloud-bigtable-emulator</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 70 -->
        <version>0.156.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 71 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 74 -->
        <artifactId>google-cloud-bigtable-emulator-core</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 75 -->
        <version>0.156.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 76 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 79 -->
        <artifactId>grpc-google-cloud-bigtable-admin-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 80 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 81 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 84 -->
        <artifactId>grpc-google-cloud-bigtable-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 85 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 86 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 89 -->
        <artifactId>proto-google-cloud-bigtable-admin-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 90 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 91 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 94 -->
        <artifactId>proto-google-cloud-bigtable-v2</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 95 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 96 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 99 -->
        <artifactId>google-cloud-bigtable-stats</artifactId>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 100 -->
        <version>2.19.2</version>  <!-- com.google.cloud:google-cloud-bigtable-bom:2.19.2, line 101 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 53 -->
        <artifactId>google-cloud-datastore</artifactId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 54 -->
        <version>2.13.5</version>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 58 -->
        <artifactId>grpc-google-cloud-datastore-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 59 -->
        <version>2.13.5</version>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 63 -->
        <artifactId>proto-google-cloud-datastore-v1</artifactId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 64 -->
        <version>0.104.5</version>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 68 -->
        <artifactId>proto-google-cloud-datastore-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 69 -->
        <version>2.13.5</version>  <!-- com.google.cloud:google-cloud-datastore-bom:2.13.5, line 70 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 53 -->
        <artifactId>google-cloud-firestore</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 54 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 58 -->
        <artifactId>google-cloud-firestore-admin</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 59 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 63 -->
        <artifactId>grpc-google-cloud-firestore-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 64 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 68 -->
        <artifactId>grpc-google-cloud-firestore-v1</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 69 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 70 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 73 -->
        <artifactId>proto-google-cloud-firestore-admin-v1</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 74 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 75 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 78 -->
        <artifactId>proto-google-cloud-firestore-v1</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 79 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 80 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 83 -->
        <artifactId>proto-google-cloud-firestore-bundle-v1</artifactId>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 84 -->
        <version>3.8.1</version>  <!-- com.google.cloud:google-cloud-firestore-bom:3.8.1, line 85 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 54 -->
        <artifactId>google-cloud-logging</artifactId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 55 -->
        <version>3.14.4</version>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 59 -->
        <artifactId>grpc-google-cloud-logging-v2</artifactId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 60 -->
        <version>0.103.4</version>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 64 -->
        <artifactId>proto-google-cloud-logging-v2</artifactId>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 65 -->
        <version>0.103.4</version>  <!-- com.google.cloud:google-cloud-logging-bom:3.14.4, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 53 -->
        <artifactId>google-cloud-pubsub</artifactId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 54 -->
        <version>1.123.3</version>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 58 -->
        <artifactId>grpc-google-cloud-pubsub-v1</artifactId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 59 -->
        <version>1.105.3</version>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 63 -->
        <artifactId>proto-google-cloud-pubsub-v1</artifactId>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 64 -->
        <version>1.105.3</version>  <!-- com.google.cloud:google-cloud-pubsub-bom:1.123.3, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 53 -->
        <artifactId>google-cloud-pubsublite</artifactId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 54 -->
        <version>1.11.1</version>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 55 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 58 -->
        <artifactId>grpc-google-cloud-pubsublite-v1</artifactId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 59 -->
        <version>1.11.1</version>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 60 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 63 -->
        <artifactId>proto-google-cloud-pubsublite-v1</artifactId>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 64 -->
        <version>1.11.1</version>  <!-- com.google.cloud:google-cloud-pubsublite-bom:1.11.1, line 65 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 54 -->
        <artifactId>google-cloud-spanner</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 55 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 56 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 59 -->
        <artifactId>google-cloud-spanner-executor</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 60 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 61 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 64 -->
        <artifactId>google-cloud-spanner</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 65 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 67 -->
        <type>test-jar</type>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 66 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 70 -->
        <artifactId>grpc-google-cloud-spanner-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 71 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 72 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 75 -->
        <artifactId>grpc-google-cloud-spanner-admin-instance-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 76 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 77 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 80 -->
        <artifactId>grpc-google-cloud-spanner-admin-database-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 81 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 82 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 85 -->
        <artifactId>proto-google-cloud-spanner-admin-instance-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 86 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 87 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 90 -->
        <artifactId>proto-google-cloud-spanner-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 91 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 92 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 95 -->
        <artifactId>proto-google-cloud-spanner-admin-database-v1</artifactId>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 96 -->
        <version>6.36.1</version>  <!-- com.google.cloud:google-cloud-spanner-bom:6.36.1, line 97 -->
      </dependency>
      <dependency>
        <groupId>com.google.cloud</groupId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 70 -->
        <artifactId>google-cloud-storage</artifactId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 71 -->
        <version>2.19.0</version>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 72 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 75 -->
        <artifactId>gapic-google-cloud-storage-v2</artifactId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 76 -->
        <version>2.19.0-alpha</version>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 77 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 80 -->
        <artifactId>grpc-google-cloud-storage-v2</artifactId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 81 -->
        <version>2.19.0-alpha</version>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 82 -->
      </dependency>
      <dependency>
        <groupId>com.google.api.grpc</groupId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 85 -->
        <artifactId>proto-google-cloud-storage-v2</artifactId>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 86 -->
        <version>2.19.0-alpha</version>  <!-- com.google.cloud:google-cloud-storage-bom:2.19.0, line 87 -->
      </dependency>
    </dependencies>
  </dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>dev.sigstore</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 97 -->
      <artifactId>sigstore-java</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 98 -->
      <version>0.4.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 84 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 101 -->
      <artifactId>maven-gpg-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 102 -->
      <version>3.1.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 103 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>com.google.oauth-client</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 108 -->
      <artifactId>google-oauth-client-jetty</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 109 -->
      <version>1.34.1</version>  <!-- com.google.oauth-client:google-oauth-client-bom:1.34.1, line 81 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>com.google.http-client</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 112 -->
      <artifactId>google-http-client-apache-v2</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 113 -->
      <version>1.42.3</version>  <!-- com.google.http-client:google-http-client-bom:1.42.3, line 76 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 116 -->
      <artifactId>commons-io</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 117 -->
      <version>2.11.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 118 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>commons-validator</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 121 -->
      <artifactId>commons-validator</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 122 -->
      <version>1.7</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 123 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.shared</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 126 -->
      <artifactId>maven-jarsigner</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 127 -->
      <version>3.0.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 128 -->
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 132 -->
      <artifactId>maven-plugin-api</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 133 -->
      <version>3.8.4</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 134 -->
      <scope>provided</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 135 -->
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 138 -->
      <artifactId>maven-core</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 139 -->
      <version>3.8.4</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 140 -->
      <scope>provided</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 141 -->
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 144 -->
      <artifactId>maven-artifact</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 145 -->
      <version>3.8.4</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 146 -->
      <scope>provided</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 147 -->
    </dependency>
    <dependency>
      <groupId>org.apache.maven</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 150 -->
      <artifactId>maven-model</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 151 -->
      <version>3.8.4</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 152 -->
      <scope>provided</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 153 -->
    </dependency>
    <dependency>
      <groupId>org.apache.maven.plugin-tools</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 156 -->
      <artifactId>maven-plugin-annotations</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 157 -->
      <version>3.6.4</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 158 -->
      <scope>provided</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 159 -->
    </dependency>
    <dependency>
      <groupId>junit</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 163 -->
      <artifactId>junit</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 164 -->
      <version>4.13.2</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 165 -->
      <scope>test</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 166 -->
    </dependency>
    <dependency>
      <groupId>org.apache.maven.plugin-testing</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 169 -->
      <artifactId>maven-plugin-testing-harness</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 170 -->
      <version>3.3.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 171 -->
      <scope>test</scope>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 172 -->
    </dependency>
  </dependencies>
  <repositories>
    <repository>
      <snapshots>
        <enabled>false</enabled>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 33 -->
      </snapshots>
      <id>central</id>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 28 -->
      <name>Central Repository</name>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 29 -->
      <url>https://repo.maven.apache.org/maven2</url>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 30 -->
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <releases>
        <updatePolicy>never</updatePolicy>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 48 -->
      </releases>
      <snapshots>
        <enabled>false</enabled>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 45 -->
      </snapshots>
      <id>central</id>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 40 -->
      <name>Central Repository</name>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 41 -->
      <url>https://repo.maven.apache.org/maven2</url>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 42 -->
    </pluginRepository>
  </pluginRepositories>
  <build>
    <sourceDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/src/main/java</sourceDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 58 -->
    <scriptSourceDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/src/main/scripts</scriptSourceDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 59 -->
    <testSourceDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/src/test/java</testSourceDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 60 -->
    <outputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/classes</outputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 55 -->
    <testOutputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/test-classes</testOutputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 57 -->
    <resources>
      <resource>
        <directory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/src/main/resources</directory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 63 -->
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/src/test/resources</directory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 68 -->
      </testResource>
    </testResources>
    <directory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target</directory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 54 -->
    <finalName>sigstore-maven-plugin-1.0-SNAPSHOT</finalName>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 56 -->
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 76 -->
          <version>3.1.0</version>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 77 -->
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 80 -->
          <version>3.6.0</version>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 81 -->
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 84 -->
          <version>3.6.0</version>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 85 -->
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 88 -->
          <version>3.0.1</version>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 89 -->
        </plugin>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 182 -->
          <version>3.1.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 183 -->
        </plugin>
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 187 -->
          <version>3.2.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 188 -->
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 191 -->
          <version>3.11.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 192 -->
        </plugin>
        <plugin>
          <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 195 -->
          <version>3.9.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 196 -->
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 199 -->
          <version>3.0.0-M5</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 200 -->
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 203 -->
          <version>3.3.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 204 -->
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 207 -->
          <version>3.0.0-M1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 208 -->
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 211 -->
          <version>3.0.0-M1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 212 -->
        </plugin>
        <plugin>
          <artifactId>maven-invoker-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 215 -->
          <version>3.6.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 216 -->
        </plugin>
        <plugin>
          <artifactId>maven-source-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 219 -->
          <version>3.2.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 220 -->
        </plugin>
        <plugin>
          <artifactId>maven-javadoc-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 223 -->
          <version>3.3.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 224 -->
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 228 -->
          <version>3.4.2</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 229 -->
        </plugin>
        <plugin>
          <artifactId>maven-site-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 233 -->
          <version>3.12.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 234 -->
          <configuration>
            <skipDeploy>true</skipDeploy>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 236 -->
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.sonatype.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 243 -->
        <artifactId>nexus-staging-maven-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 244 -->
        <version>1.6.13</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 245 -->
        <extensions>true</extensions>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 246 -->
        <executions>
          <execution>
            <id>injected-nexus-deploy</id>
            <phase>deploy</phase>
            <goals>
              <goal>deploy</goal>
            </goals>
            <configuration>
              <serverId>ossrh</serverId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 248 -->
              <nexusUrl>https://s01.oss.sonatype.org/</nexusUrl>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 249 -->
              <autoReleaseAfterClose>true</autoReleaseAfterClose>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 250 -->
            </configuration>
          </execution>
        </executions>
        <configuration>
          <serverId>ossrh</serverId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 248 -->
          <nexusUrl>https://s01.oss.sonatype.org/</nexusUrl>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 249 -->
          <autoReleaseAfterClose>true</autoReleaseAfterClose>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 250 -->
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 255 -->
        <version>3.9.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 196 -->
        <executions>
          <execution>
            <id>default-addPluginArtifactMetadata</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>package</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>addPluginArtifactMetadata</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
            <configuration>
              <skipErrorNoDescriptorsFound>true</skipErrorNoDescriptorsFound>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 257 -->
            </configuration>
          </execution>
          <execution>
            <id>default-descriptor</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>process-classes</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>descriptor</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
            <configuration>
              <skipErrorNoDescriptorsFound>true</skipErrorNoDescriptorsFound>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 257 -->
            </configuration>
          </execution>
          <execution>
            <id>mojo-descriptor</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 261 -->
            <goals>
              <goal>descriptor</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 263 -->
            </goals>
            <configuration>
              <skipErrorNoDescriptorsFound>true</skipErrorNoDescriptorsFound>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 257 -->
            </configuration>
          </execution>
          <execution>
            <id>help-goal</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 267 -->
            <goals>
              <goal>helpmojo</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 269 -->
            </goals>
            <configuration>
              <skipErrorNoDescriptorsFound>true</skipErrorNoDescriptorsFound>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 257 -->
            </configuration>
          </execution>
        </executions>
        <configuration>
          <skipErrorNoDescriptorsFound>true</skipErrorNoDescriptorsFound>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 257 -->
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-source-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 276 -->
        <version>3.2.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 220 -->
        <executions>
          <execution>
            <id>attach-sources</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 279 -->
            <goals>
              <goal>jar-no-fork</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 281 -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-javadoc-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 288 -->
        <version>3.3.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 224 -->
        <executions>
          <execution>
            <id>attach-javadocs</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 291 -->
            <goals>
              <goal>jar</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 293 -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-scm-publish-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 300 -->
        <version>3.1.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 301 -->
        <executions>
          <execution>
            <id>scm-publish</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 308 -->
            <phase>site-deploy</phase>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 309 -->
            <goals>
              <goal>publish-scm</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 311 -->
            </goals>
            <configuration>
              <scmBranch>gh-pages</scmBranch>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 303 -->
              <content>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</content>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 304 -->
            </configuration>
          </execution>
        </executions>
        <configuration>
          <scmBranch>gh-pages</scmBranch>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 303 -->
          <content>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</content>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 304 -->
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-clean-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 182 -->
        <version>3.1.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 183 -->
        <executions>
          <execution>
            <id>default-clean</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>clean</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>clean</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-resources-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 187 -->
        <version>3.2.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 188 -->
        <executions>
          <execution>
            <id>default-testResources</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>process-test-resources</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>testResources</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
          <execution>
            <id>default-resources</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>process-resources</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>resources</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-jar-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 203 -->
        <version>3.3.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 204 -->
        <executions>
          <execution>
            <id>default-jar</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>package</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>jar</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 191 -->
        <version>3.11.0</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 192 -->
        <executions>
          <execution>
            <id>default-compile</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>compile</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>compile</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
          <execution>
            <id>default-testCompile</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>test-compile</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>testCompile</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-surefire-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 199 -->
        <version>3.0.0-M5</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 200 -->
        <executions>
          <execution>
            <id>default-test</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>test</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>test</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-install-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 207 -->
        <version>3.0.0-M1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 208 -->
        <executions>
          <execution>
            <id>default-install</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>install</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>install</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-deploy-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 211 -->
        <version>3.0.0-M1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 212 -->
      </plugin>
      <plugin>
        <artifactId>maven-site-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 233 -->
        <version>3.12.1</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 234 -->
        <executions>
          <execution>
            <id>default-site</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>site</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>site</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
            <configuration>
              <skipDeploy>true</skipDeploy>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 236 -->
              <outputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</outputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 96 -->
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 322 -->
                  <artifactId>maven-project-info-reports-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 323 -->
                  <reportSets>
                    <reportSet>
                      <id>default</id>  <!-- org.apache.maven:maven-model-builder:3.9.3:reporting-converter -->
                      <reports>
                        <report>index</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 327 -->
                        <report>summary</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 328 -->
                        <report>dependency-info</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 329 -->
                        <report>team</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 330 -->
                        <report>scm</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 331 -->
                        <report>issue-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 332 -->
                        <report>dependency-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 333 -->
                        <report>dependencies</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 334 -->
                        <report>plugin-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 335 -->
                        <report>plugins</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 336 -->
                        <report>distribution-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 337 -->
                      </reports>
                    </reportSet>
                  </reportSets>
                </reportPlugin>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 343 -->
                  <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 344 -->
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
          <execution>
            <id>default-deploy</id>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <phase>site-deploy</phase>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            <goals>
              <goal>deploy</goal>  <!-- org.apache.maven:maven-core:3.9.3:default-lifecycle-bindings -->
            </goals>
            <configuration>
              <skipDeploy>true</skipDeploy>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 236 -->
              <outputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</outputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 96 -->
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 322 -->
                  <artifactId>maven-project-info-reports-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 323 -->
                  <reportSets>
                    <reportSet>
                      <id>default</id>  <!-- org.apache.maven:maven-model-builder:3.9.3:reporting-converter -->
                      <reports>
                        <report>index</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 327 -->
                        <report>summary</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 328 -->
                        <report>dependency-info</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 329 -->
                        <report>team</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 330 -->
                        <report>scm</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 331 -->
                        <report>issue-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 332 -->
                        <report>dependency-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 333 -->
                        <report>dependencies</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 334 -->
                        <report>plugin-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 335 -->
                        <report>plugins</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 336 -->
                        <report>distribution-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 337 -->
                      </reports>
                    </reportSet>
                  </reportSets>
                </reportPlugin>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 343 -->
                  <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 344 -->
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
        </executions>
        <configuration>
          <skipDeploy>true</skipDeploy>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 236 -->
          <outputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</outputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 96 -->
          <reportPlugins>
            <reportPlugin>
              <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 322 -->
              <artifactId>maven-project-info-reports-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 323 -->
              <reportSets>
                <reportSet>
                  <id>default</id>  <!-- org.apache.maven:maven-model-builder:3.9.3:reporting-converter -->
                  <reports>
                    <report>index</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 327 -->
                    <report>summary</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 328 -->
                    <report>dependency-info</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 329 -->
                    <report>team</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 330 -->
                    <report>scm</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 331 -->
                    <report>issue-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 332 -->
                    <report>dependency-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 333 -->
                    <report>dependencies</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 334 -->
                    <report>plugin-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 335 -->
                    <report>plugins</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 336 -->
                    <report>distribution-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 337 -->
                  </reports>
                </reportSet>
              </reportSets>
            </reportPlugin>
            <reportPlugin>
              <groupId>org.apache.maven.plugins</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 343 -->
              <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 344 -->
            </reportPlugin>
          </reportPlugins>
        </configuration>
      </plugin>
    </plugins>
  </build>
  <reporting>
    <outputDirectory>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/site</outputDirectory>  <!-- org.apache.maven:maven-model-builder:3.9.3:super-pom, line 96 -->
    <plugins>
      <plugin>
        <artifactId>maven-project-info-reports-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 323 -->
        <reportSets>
          <reportSet>
            <reports>
              <report>index</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 327 -->
              <report>summary</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 328 -->
              <report>dependency-info</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 329 -->
              <report>team</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 330 -->
              <report>scm</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 331 -->
              <report>issue-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 332 -->
              <report>dependency-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 333 -->
              <report>dependencies</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 334 -->
              <report>plugin-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 335 -->
              <report>plugins</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 336 -->
              <report>distribution-management</report>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 337 -->
            </reports>
          </reportSet>
        </reportSets>
      </plugin>
      <plugin>
        <artifactId>maven-plugin-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 344 -->
      </plugin>
    </plugins>
  </reporting>
  <profiles>
    <profile>
      <id>run-its</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 351 -->
      <build>
        <plugins>
          <plugin>
            <artifactId>maven-invoker-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 357 -->
            <executions>
              <execution>
                <id>integration-test</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 374 -->
                <goals>
                  <goal>install</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 376 -->
                  <goal>integration-test</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 377 -->
                  <goal>verify</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 378 -->
                </goals>
              </execution>
            </executions>
            <configuration>
              <debug>true</debug>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 359 -->
              <cloneProjectsTo>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/it</cloneProjectsTo>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 360 -->
              <pomIncludes>
                <pomInclude>*/pom.xml</pomInclude>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 362 -->
              </pomIncludes>
              <postBuildHookScript>verify</postBuildHookScript>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 364 -->
              <localRepositoryPath>/home/herve/dev/maven/misc/sigstore/sigstore-maven-plugin/target/local-repo</localRepositoryPath>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 365 -->
              <settingsFile>src/it/settings.xml</settingsFile>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 366 -->
              <goals>
                <goal>clean</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 368 -->
                <goal>verify</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 369 -->
              </goals>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
    <profile>
      <id>dev</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 387 -->
      <build>
        <plugins>
          <plugin>
            <groupId>dev.sigstore</groupId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 391 -->
            <artifactId>sigstore-maven-plugin</artifactId>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 392 -->
            <version>1.0-SNAPSHOT</version>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 393 -->
            <executions>
              <execution>
                <id>sign</id>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 396 -->
                <goals>
                  <goal>sign</goal>  <!-- dev.sigstore:sigstore-maven-plugin:1.0-SNAPSHOT, line 398 -->
                </goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
</project>


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.645 s
[INFO] Finished at: 2023-07-23T16:24:45+02:00
[INFO] ------------------------------------------------------------------------
```

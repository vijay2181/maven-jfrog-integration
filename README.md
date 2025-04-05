# JFrog Artifactory integration demo with Maven dependency management:

- craete a maven repository in jfrog

<img width="1727" alt="Screenshot 2025-04-05 at 9 46 35 PM" src="https://github.com/user-attachments/assets/9581d981-b41d-4b28-83c8-55b18103a68c" />

- click on manage repositories
- create a maven local repository

<img width="1629" alt="Screenshot 2025-04-05 at 10 22 00 PM" src="https://github.com/user-attachments/assets/3ca2f051-7418-4f61-a389-835c2040a38d" />


<img width="1638" alt="Screenshot 2025-04-05 at 10 22 49 PM" src="https://github.com/user-attachments/assets/4b21b040-a18a-4a2c-8bbf-d4b93a70c649" />


<img width="776" alt="Screenshot 2025-04-05 at 10 23 13 PM" src="https://github.com/user-attachments/assets/11c135b5-5812-4ec5-a42a-a44fd5a2510e" />

- click on setup maven client


<img width="783" alt="Screenshot 2025-04-05 at 10 23 52 PM" src="https://github.com/user-attachments/assets/7e3fc7f0-87d2-41ad-8c56-671292a3c0c2" />


- here provide your jfrog password and click on create token, you will get token, copy the token
- now click on deploy tab, you will getbelwo xml file

```
<distributionManagement>
    <repository>
        <id>central</id>
        <name>a0qniolqoezpm-artifactory-primary-0-releases</name>
        <url>https://testproject1234.jfrog.io/artifactory/maven-local</url>
    </repository>
    <snapshotRepository>
        <id>snapshots</id>
        <name>a0qniolqoezpm-artifactory-primary-0-snapshots</name>
        <url>https://testproject1234.jfrog.io/artifactory/maven-local</url>
    </snapshotRepository>
</distributionManagement>
```

```
install packages:
=================
sudo apt update
sudo apt install default-jdk
sudo apt install maven
java -version
mvn -version



Adding commons-io-2.18.0.jar to JFrog Artifactory Before Use
uploading jar to jfrog maven repo:
===================================
- first get the jar
cd /root

wget https://repo1.maven.org/maven2/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar


install jfrog cli:
------------------
curl -fL https://install-cli.jfrog.io | sh


jf c add artifactory-demo \
  --url=https://testproject1234.jfrog.io \
  --user=jfrog \
  --password=Test@jfrog

jf rt upload commons-io-2.18.0.jar maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar



root@8e9c567a9eed:~/maven-project# jf c add artifactory-demo \
  --url=https://testproject1234.jfrog.io \
  --user=jfrog \
  --password=Test@jfrog
  
root@8e9c567a9eed:~/maven-project# ls
commons-io-2.18.0.jar

root@8e9c567a9eed:~/maven-project# jf rt upload commons-io-2.18.0.jar maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar
00:20:29 [🔵Info] Log path: /root/.jfrog/logs/jfrog-cli.2025-04-06.00-20-29.6193.log
00:20:30 [🔵Info] These files were uploaded:

📦 maven-local
└── 📁 commons-io
    └── 📁 commons-io
        └── 📁 2.18.0
            └── 📄 commons-io-2.18.0.jar


{
  "status": "success",
  "totals": {
    "success": 1,
    "failure": 0
  }
}

- verification

root@8e9c567a9eed:~/maven-project# jf rt search maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar
00:23:58 [🔵Info] Searching artifacts...
00:23:59 [🔵Info] Found 1 artifact.
[
  {
    "path": "maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar",
    "type": "file",
    "size": 538910,
    "created": "2025-04-05T17:20:23.625Z",
    "modified": "2025-04-05T17:20:23.625Z",
    "sha1": "44084ef756763795b31c578403dd028ff4a22950",
    "sha256": "f3ca0f8d63c40e23a56d54101c60d5edee136b42d84bfb85bc7963093109cf8b",
    "md5": "8cce74ccf461cd6502ae04c908eca917"
  }
]

jf rt curl -XGET "/api/storage/maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar"

- you can also download the uploaded jar file

root@8e9c567a9eed:~/maven-project# jf rt download maven-local/commons-io/commons-io/2.18.0/commons-io-2.18.0.jar downloaded.jar
00:25:15 [🔵Info] Log path: /root/.jfrog/logs/jfrog-cli.2025-04-06.00-25-15.6214.log
{
  "status": "success",
  "totals": {
    "success": 1,
    "failure": 0
  }
}
root@8e9c567a9eed:~/maven-project# ls
commons-io  commons-io-2.18.0.jar
root@8e9c567a9eed:~/maven-project# cd commons-io
root@8e9c567a9eed:~/maven-project/commons-io# ls
commons-io
root@8e9c567a9eed:~/maven-project/commons-io# cd commons-io/
root@8e9c567a9eed:~/maven-project/commons-io/commons-io# ls
2.18.0
root@8e9c567a9eed:~/maven-project/commons-io/commons-io# cd 2.18.0/
root@8e9c567a9eed:~/maven-project/commons-io/commons-io/2.18.0# ls
downloaded.jar
root@8e9c567a9eed:~/maven-project/commons-io/commons-io/2.18.0# 




INSTALL MAVEN DEPENDENCIES FOR PROJECT FRIST IN JFROG:
=======================================================
#!/bin/bash

# Artifactory configuration
ARTIFACTORY_URL="https://testproject1234.jfrog.io/artifactory"
USERNAME="jfrog"
PASSWORD="Test@jfrog"

# Delete existing repositories if they exist
curl -u "$USERNAME:$PASSWORD" -X DELETE "$ARTIFACTORY_URL/api/repositories/maven-central" || true
curl -u "$USERNAME:$PASSWORD" -X DELETE "$ARTIFACTORY_URL/api/repositories/jcenter" || true
curl -u "$USERNAME:$PASSWORD" -X DELETE "$ARTIFACTORY_URL/api/repositories/maven-virtual" || true

# Create remote repositories
echo "Creating maven-central remote repository..."
curl -u "$USERNAME:$PASSWORD" -X PUT \
  -H "Content-Type: application/json" \
  -d '{
    "key": "maven-central",
    "rclass": "remote",
    "packageType": "maven",
    "url": "https://repo.maven.apache.org/maven2/"
  }' \
  "$ARTIFACTORY_URL/api/repositories/maven-central"

echo "Creating jcenter remote repository..."
curl -u "$USERNAME:$PASSWORD" -X PUT \
  -H "Content-Type: application/json" \
  -d '{
    "key": "jcenter",
    "rclass": "remote",
    "packageType": "maven",
    "url": "https://jcenter.bintray.com/"
  }' \
  "$ARTIFACTORY_URL/api/repositories/jcenter"

# Create virtual repository
echo "Creating maven-virtual virtual repository..."
curl -u "$USERNAME:$PASSWORD" -X PUT \
  -H "Content-Type: application/json" \
  -d '{
    "key": "maven-virtual",
    "rclass": "virtual",
    "packageType": "maven",
    "repositories": ["maven-local", "maven-central", "jcenter"],
    "defaultDeploymentRepo": "maven-local"
  }' \
  "$ARTIFACTORY_URL/api/repositories/maven-virtual"
echo "Artifactory repositories successfully configured!"



root@8e9c567a9eed:~/maven-project# curl -u "$USERNAME:$PASSWORD" "$ARTIFACTORY_URL/api/repositories"
[ {
  "key" : "docker-trial",
  "description" : "",
  "type" : "LOCAL",
  "url" : "https://testproject1234.jfrog.io/artifactory/docker-trial",
  "packageType" : "Docker"
}, {
  "key" : "maven-local",
  "description" : "maven-local repo",
  "type" : "LOCAL",
  "url" : "https://testproject1234.jfrog.io/artifactory/maven-local",
  "packageType" : "Maven"
}, {
  "key" : "test-docker-docker-local",
  "type" : "LOCAL",
  "url" : "https://testproject1234.jfrog.io/artifactory/test-docker-docker-local",
  "packageType" : "Docker"
}, {
  "key" : "tf-trial",
  "description" : "",
  "type" : "LOCAL",
  "url" : "https://testproject1234.jfrog.io/artifactory/tf-trial",
  "packageType" : "TerraformBackend"
}, {
  "key" : "jcenter",
  "description" : "",
  "type" : "REMOTE",
  "url" : "https://jcenter.bintray.com/",
  "packageType" : "Maven"
}, {
  "key" : "maven-central",
  "description" : "",
  "type" : "REMOTE",
  "url" : "https://repo.maven.apache.org/maven2/",
  "packageType" : "Maven"
}, {
  "key" : "test-docker-docker-remote",
  "type" : "REMOTE",
  "url" : "https://registry-1.docker.io/",
  "packageType" : "Docker"
}, {
  "key" : "maven-virtual",
  "description" : "",
  "type" : "VIRTUAL",
  "url" : "https://testproject1234.jfrog.io/artifactory/maven-virtual",
  "packageType" : "Maven"






add settings.xml:
=================
# Create a fresh settings.xml with proper authentication
cat > ~/.m2/settings.xml <<'EOL'
<settings>
  <servers>
    <server>
      <id>artifactory</id>
      <username>jfrog</username>
      <password>Test@jfrog</password>
    </server>
  </servers>
  
  <mirrors>
    <mirror>
      <id>artifactory</id>
      <name>JFrog Virtual</name>
      <url>https://testproject1234.jfrog.io/artifactory/maven-virtual</url>
      <mirrorOf>external:*</mirrorOf>
    </mirror>
  </mirrors>
</settings>
EOL


- here
All requests to external repositories (like Maven Central) are automatically redirected to:
https://testproject1234.jfrog.io/artifactory/maven-virtual

<mirrorOf>external:*</mirrorOf> Specifics
external:* means this only applies to repositories not marked as <blocked> or <local>
Your Artifactory becomes the single entry point for all dependency downloads
Internal/private repositories remain unaffected



rm -rf ~/.m2/repository


Manually Upload extra Critical Maven Plugins
============================================
# Create a temporary directory
mkdir -p ~/maven-plugins
cd ~/maven-plugins

# List of essential plugins and versions
plugins=(
  "maven-clean-plugin:2.5"
  "maven-resources-plugin:2.6"
  "maven-compiler-plugin:3.8.1"
  "maven-surefire-plugin:2.12.4"
  "maven-install-plugin:2.4"
  "maven-deploy-plugin:2.7"
  "maven-site-plugin:3.3"
)

# Download and upload each plugin
for plugin in "${plugins[@]}"; do
  PLUGIN_ARTIFACT=${plugin%:*}
  PLUGIN_VERSION=${plugin#*:}
  PLUGIN_GROUP="org/apache/maven/plugins"
  
  echo "Processing $PLUGIN_ARTIFACT:$PLUGIN_VERSION"
  
  # Download from Maven Central
  wget https://repo1.maven.org/maven2/${PLUGIN_GROUP}/${PLUGIN_ARTIFACT}/${PLUGIN_VERSION}/${PLUGIN_ARTIFACT}-${PLUGIN_VERSION}.pom
  wget https://repo1.maven.org/maven2/${PLUGIN_GROUP}/${PLUGIN_ARTIFACT}/${PLUGIN_VERSION}/${PLUGIN_ARTIFACT}-${PLUGIN_VERSION}.jar

  # Upload to Artifactory
  jf rt upload ${PLUGIN_ARTIFACT}-${PLUGIN_VERSION}.pom maven-local/${PLUGIN_GROUP}/${PLUGIN_ARTIFACT}/${PLUGIN_VERSION}/
  jf rt upload ${PLUGIN_ARTIFACT}-${PLUGIN_VERSION}.jar maven-local/${PLUGIN_GROUP}/${PLUGIN_ARTIFACT}/${PLUGIN_VERSION}/
done

# Clean up
cd ~
rm -rf ~/maven-plugins



Verify Virtual Repository Configuration
jf rt curl -XGET /api/repositories/maven-virtual | jq .repositories


Test with a Simple Command
mvn -U help:effective-settings



project:
========
mvn archetype:generate -DgroupId=com.example -DartifactId=my-artifactory-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

mvn archetype:generate -DgroupId=com.mycompany -DartifactId=artifactory-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd artifactory-demo

root@8e9c567a9eed:~/artifactory-demo# pwd
/root/artifactory-demo

- chnage default pom.xml to below

pom.xml:
=========
<project>
  <modelVersion>4.0.0</modelVersion>
  
  <groupId>com.mycompany</groupId>
  <artifactId>jfrog-demo</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
  </properties>
  
  <repositories>
    <repository>
      <id>artifactory</id>
      <name>JFrog Artifactory</name>
      <url>https://testproject1234.jfrog.io/artifactory/maven-local</url>
      <releases><enabled>true</enabled></releases>
      <snapshots><enabled>true</enabled></snapshots>
    </repository>
  </repositories>
  
  <pluginRepositories>
    <pluginRepository>
      <id>artifactory</id>
      <name>JFrog Artifactory</name>
      <url>https://testproject1234.jfrog.io/artifactory/maven-local</url>
    </pluginRepository>
  </pluginRepositories>
  
  <dependencies>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.18.0</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
      </plugin>
    </plugins>
  </build>
</project>

- here in pom.xml we are getting commons-io dependency jar from jfrog maven-local repository
mvn dependency:resolve
mvn clean install


root@8e9c567a9eed:~/artifactory-demo# mvn clean install
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.mycompany:jfrog-demo >----------------------
[INFO] Building jfrog-demo 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ jfrog-demo ---
[INFO] Deleting /root/artifactory-demo/target
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ jfrog-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /root/artifactory-demo/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ jfrog-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /root/artifactory-demo/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ jfrog-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /root/artifactory-demo/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ jfrog-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /root/artifactory-demo/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ jfrog-demo ---
[INFO] Surefire report directory: /root/artifactory-demo/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.mycompany.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.073 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ jfrog-demo ---
[INFO] Building jar: /root/artifactory-demo/target/jfrog-demo-1.0-SNAPSHOT.jar
[INFO] 
[INFO] --- maven-install-plugin:2.4:install (default-install) @ jfrog-demo ---
[INFO] Installing /root/artifactory-demo/target/jfrog-demo-1.0-SNAPSHOT.jar to /root/.m2/repository/com/mycompany/jfrog-demo/1.0-SNAPSHOT/jfrog-demo-1.0-SNAPSHOT.jar
[INFO] Installing /root/artifactory-demo/pom.xml to /root/.m2/repository/com/mycompany/jfrog-demo/1.0-SNAPSHOT/jfrog-demo-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.753 s
[INFO] Finished at: 2025-04-06T01:25:21+07:00
[INFO] ------------------------------------------------------------------------
root@8e9c567a9eed:~/artifactory-demo# 




To verify that the commons-io dependency was resolved from your JFrog Artifactory maven-local repository, you can use these methods:
===============================================
mvn dependency:tree -Dverbose

root@8e9c567a9eed:~/artifactory-demo# mvn dependency:tree -Dverbose
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.mycompany:jfrog-demo >----------------------
[INFO] Building jfrog-demo 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ jfrog-demo ---
[INFO] com.mycompany:jfrog-demo:jar:1.0-SNAPSHOT
[INFO] +- commons-io:commons-io:jar:2.18.0:compile
[INFO] \- junit:junit:jar:4.13.2:test
[INFO]    \- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.393 s
[INFO] Finished at: 2025-04-06T01:33:45+07:00
[INFO] ------------------------------------------------------------------------


ls -l ~/.m2/repository/commons-io/commons-io/2.18.0/

root@8e9c567a9eed:~/artifactory-demo# ls -l ~/.m2/repository/commons-io/commons-io/2.18.0/
total 556
-rw-r--r-- 1 root root    210 Apr  6 01:13 _remote.repositories
-rw-r--r-- 1 root root 538910 Apr  6 01:13 commons-io-2.18.0.jar
-rw-r--r-- 1 root root  20032 Apr  6 01:13 commons-io-2.18.0.pom
-rw-r--r-- 1 root root     40 Apr  6 01:13 commons-io-2.18.0.pom.sha1



root@8e9c567a9eed:~/artifactory-demo# cat ~/.m2/repository/commons-io/commons-io/2.18.0/_remote.repositories
#NOTE: This is a Maven Resolver internal implementation file, its format can be changed without prior notice.
#Sun Apr 06 01:13:55 CXT 2025
commons-io-2.18.0.jar>artifactory=
commons-io-2.18.0.pom>artifactory=

root@8e9c567a9eed:~/artifactory-demo# curl -u jfrog:Test@jfrog123 \
  "https://testproject1234.jfrog.io/artifactory/api/storage/maven-local/commons-io/commons-io/2.18.0/"
{
  "repo" : "maven-local",
  "path" : "/commons-io/commons-io/2.18.0",
  "created" : "2025-04-05T17:20:24.354Z",
  "createdBy" : "jfrog",
  "lastModified" : "2025-04-05T17:20:24.354Z",
  "modifiedBy" : "jfrog",
  "lastUpdated" : "2025-04-05T17:20:24.354Z",
  "children" : [ {
    "uri" : "/commons-io-2.18.0.jar",
    "folder" : false
  } ],
  "uri" : "https://testproject1234.jfrog.io/artifactory/api/storage/maven-local/commons-io/commons-io/2.18.0"

```




```
To make the script dynamic such that the version is passed, and the rest of the process adjusts accordingly, you can modify the script like this:

Key Points:
Version Input: The version will be passed as an input argument.

Dynamic Plugin Processing: The script will adjust based on the passed version for all plugins.

Updated Script:
bash
Copy
Edit
#!/bin/bash

# Pass the version as an argument, e.g., ./script.sh 3.8.1
PLUGIN_VERSION=$1

# Ensure version is passed
if [ -z "$PLUGIN_VERSION" ]; then
  echo "Please provide a version as an argument (e.g., 3.8.1)"
  exit 1
fi

# Create a temporary directory
mkdir -p ~/maven-plugins
cd ~/maven-plugins

# List of essential plugins without versions
plugins=(
  "maven-clean-plugin"
  "maven-resources-plugin"
  "maven-compiler-plugin"
  "maven-surefire-plugin"
  "maven-install-plugin"
  "maven-deploy-plugin"
  "maven-site-plugin"
)

# Download and upload each plugin dynamically for the given version
for plugin in "${plugins[@]}"; do
  PLUGIN_GROUP="org/apache/maven/plugins"
  
  echo "Processing $plugin:$PLUGIN_VERSION"

  # Download plugin POM and JAR for the given version
  wget https://repo1.maven.org/maven2/${PLUGIN_GROUP}/${plugin}/${PLUGIN_VERSION}/${plugin}-${PLUGIN_VERSION}.pom
  wget https://repo1.maven.org/maven2/${PLUGIN_GROUP}/${plugin}/${PLUGIN_VERSION}/${plugin}-${PLUGIN_VERSION}.jar

  # Upload to Artifactory
  jf rt upload ${plugin}-${PLUGIN_VERSION}.pom maven-local/${PLUGIN_GROUP}/${plugin}/${PLUGIN_VERSION}/
  jf rt upload ${plugin}-${PLUGIN_VERSION}.jar maven-local/${PLUGIN_GROUP}/${plugin}/${PLUGIN_VERSION}/
done

# Clean up
cd ~
rm -rf ~/maven-plugins

# Verify Virtual Repository Configuration
jf rt curl -XGET /api/repositories/maven-virtual | jq .repositories

# Test with a Simple Command
mvn -U help:effective-settings
Key Changes:
Version Passed as Argument: The version is now passed as a command-line argument when running the script (./script.sh 3.8.1). This version is then used for all plugins in the list.

Dynamic Processing: For each plugin in the list, the script fetches the specified version and performs the necessary operations dynamically.

How to Run:
To run the script, simply pass the version you want to use:

bash
Copy
Edit
./script.sh 3.8.1
This will download the plugins for version 3.8.1 and upload them to Artifactory. If you need a different version, just pass that as an argument.
```

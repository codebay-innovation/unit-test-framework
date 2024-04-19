# Issue repository
This is the main repository for report issues related with the Codebay AEM "Unit test Framework"

# Codebay AEM Unit test Framework [![Codebay Innovation](https://www.codebay-innovation.com/components-auto-generator/codebay_logo.png)](https://www.codebay-innovation.com/)

## CodeBay Framework to leverage JUnit Test 5 on AEM 6.5 and AEMaaCS
[![Powered By Codebay Innovation](https://img.shields.io/badge/Powered%20By-Codebay%20Innovation-37c755?labelColor=27d1e0)](https://www.codebay-innovation.com/)
[![Build Status](https://img.shields.io/badge/build-v6.5.1.0005-lightgrey)]()

AEM CodeBay Mock, it's a framework to help bulding Unit Test.

- Built to work with Java 8 or higher
- Ready to work on AEMaaCS
- Ready to work on AEM 6.5

## Table of Contents

- [How To](#how-to)
- [Features](#features)
   - [Mock Content](#mock-content)
   - [Getter and Setter](#getter-and-setter)
   - [Mock XssApi](#mock-xssapi)
   - [Initialize Servlet](#initialize-servlet)
   - [Mock WcmUsePojo](#mock-wcmusepojo)
   - [Query Results](#query-results)
- [License](#license)

## How To

Add the Codebay Repository on you main pom.xml

```xml
<!-- CODEBAY REPOSITORY -->
<repositories>
   ...
   <repository>
      <id>libs-release</id>
      <name>libs-release</name>
      <url>https://artifactory.codeland.it:443/artifactory/libs-release/</url>
      <releases>
         <enabled>true</enabled>
      </releases>
      <snapshots>
         <enabled>false</enabled>
      </snapshots>
   </repository>
   ...
</repositories>
```

Add the dependency on the dependency management section of your main pom.xml

```xml
<dependencyManagement>
   <dependencies>
      ...
      <dependency>
            <groupId>com.codebay.test</groupId>
            <artifactId>aem-mock</artifactId>
            <version>6.5.1.0005</version>
            <scope>test</scope>
      </dependency>
      ...
   </dependencies>
</dependencyManagement>
```

Add the dependency on your core/pom.xml

```xml
<dependencies>
   ...
   <dependency>
         <groupId>com.codebay.test</groupId>
         <artifactId>aem-mock</artifactId>
   </dependency>
   ...
</dependencies>
```

## Features

> Note: The `AemMockContext` it's resolve from the `com.codebay.test.mock.AemMockContext`

### Mock Content:

Allows to mock the content repository with a provided content, from the ui.content module or from test/resources files. Example:

```java
import com.codebay.test.mock.AemMockContext;
import io.wcm.testing.mock.aem.junit5.AemContext;
import io.wcm.testing.mock.aem.junit5.AemContextExtension;

@ExtendWith(AemContextExtension.class)
public class Test {
	private AemContext context = AemMockContext.newAemContext(new String[] {"/content/mysite/us/en","/content/mysite/us"});
}
```

### Getter and Setter:

Allows to invoke getters an setter for the given class to automatically coverage the methods. Example:

```java
AemMockContext.getUtilities().invokeGetterAndSetter(new ObjBean());
```

### Mock XssApi

Fake css api on the context to be able to test where it's used. Example:

```java
AemContext context = AemMockContext.newAemContext(null);
AemMockContext.getUtilities().initFakeXssApi(context);
```

### Initialize Servlet:

Register servlet on the context. Example:

```java
AemContext context = AemMockContext.newAemContext(null);
MyServlet servlet = AemMockContext.getUtilities().initServlet(context, new MyServlet());
servlet.doGet(context.request(), context.response());
```

### Mock WcmUsePojo:

Get a mocked binding to init WcmUsePojo object

```java
AemContext context = AemMockContext.newAemContext(new String[] {"/content/mysite/us/en","/content/mysite/us"});

Map<String, Object> map = new HashMap<>();
map.put("key1", "value1");

WcmTestUsePojo wcm = new WcmTestUsePojo();
wcm.init(AemMockContext.mockBindingWcmUsePojo(context, "/content/mysite/us/en/jcr:content/root/container/title", map));

assertEquals("value1", wcm.get("key1", String.class));
```

### Query Results

Prepare query to return the given results whenever `com.day.cq.search.QueryBuilder` is used

```java
AemContext context = AemMockContext.newAemContext(new String[] {"/content/dam/cf-fragments/test"});
AemMockContext.prepareQueryResults(context, context.resourceResolver().getResource("/content/dam/cf-fragments/test"));
QueryBsn search = new QueryBsn(context.resourceResolver());
Resource testResource = search.searchFragment();
```

## License
CODEBAY INNOVATION
__________________

Copyright (C) 2024 - Codebay Innovation S.L - https://www.codebay-innovation.com - All Rights Reserved

Unauthorized copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, via any medium is strictly prohibited
Proprietary and confidential.

---
layout: post
title:  "Java 15 records"
date:   2021-07-11 16:45:02 +0200
categories: java java15
---
# Java records
Java 15 introduced the record — they reduce the hassle of creating POJOs immensely.

## The test
With the test code, we create an object _Painting_ and assign _Dimensions_, testing the width corresponds with the test data:

```java
import org.example.record.Dimensions;
import org.example.record.Painting;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class PaintingTest {
    @Test
    void testGetDimensions() {
        var painting = new Painting(new Dimensions(12, 22));
        assertEquals(22, painting.dimensions().width());
    }
}

```

## The code

Painting.java
```java
package org.example.record;

public record Painting(Dimensions dimensions) {}

```

Dimensions.java[*](#clarification)
```java
package org.example.record;

public record Dimensions(int height, int width) {}

```
## Summed up
With records, boilerplate class declarations, getters, equals and hashcode are replaced by a single record class declaration.

For more information, see Oracle's [Java 15 Java Language Updates](https://docs.oracle.com/en/java/javase/15/language/records.html).

---
<a href="#clarification"></a>* Ignoring that we haven't specified a unit such as centimetre/inch
---
layout: post
title:  "Java 15 switch changes — case labels"
date:   2021-07-11 16:45:02 +0200
categories: java java15
---
# Java 15 switch changes — case labels

Java 15 adds a number of changes to switch statements, including how case labels are expressed and the `yield` keyword.

## The test
When a game is initialized, the game speed is `1`, when a speed-up is applied by consuming fruit, the speed increases. A message is passed to standard out if the speed-up is "LYCHEE". 

CaseLabelTest.java
```java
import org.example.caseLabels.Game;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.EnumSource;

import static com.github.stefanbirkner.systemlambda.SystemLambda.tapSystemOut;
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.containsString;
import static org.hamcrest.Matchers.greaterThan;
import static org.hamcrest.Matchers.is;

public class CaseLabelTest {
    @ParameterizedTest
    @EnumSource(Game.Fruit.class)
    void gameSpeedUpReturnsIncreasedValueWhenInputIsFruit(Game.Fruit fruit) {
        var game = new Game();
        var originalSpeed = game.speed();
        game.speedUp(fruit);
        var newSpeed = game.speed();
        assertThat(newSpeed, is(greaterThan(originalSpeed)));
    }

    @Test
    void gameSpeedUpPrintsMessageToStandardOutWhenInputIsLychee() throws Exception {
        var game = new Game();
        var actual = tapSystemOut(() -> game.speedUp(Game.Fruit.LYCHEE));
        assertThat(actual, containsString(Game.LYCHEE_MESSAGE));
    }
}

```

## The code
Game.java
```java
package org.example.caseLabels;

public class Game {
    public static final String LYCHEE_MESSAGE = "Someone got Lychee";
    private double speed = 1;

    public double speed() {
        return this.speed;
    }

    public void speedUp(Fruit fruit) {
        var multiplier = switch(fruit) {
            case APPLE, PEAR -> 1.5;
            case BANANA -> 3;
            case ORANGE -> 2.5;
            case LYCHEE -> {
                System.out.println(LYCHEE_MESSAGE);
                yield 7;
            }
        };
        speed = speed * multiplier;
    }

    public enum Fruit {
        APPLE,
        BANANA,
        LYCHEE,
        ORANGE,
        PEAR,
    }
}

```
## Summed up

Case labels provide a simplified syntax in the form `case label1 [, label2, …] -> expression`. In cases of a non-simple expression, braces are added with the `yield <value>`.
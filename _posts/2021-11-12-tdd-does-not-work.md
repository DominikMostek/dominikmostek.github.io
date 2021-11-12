---
layout: post
title: TDD does not work
permalink: 2021-11-12-tdd-does-not-work
---
Test-driven development. For many a holy grail for many a useless technique. Today I would like to take a look at why TDD might be failing for many.

I have written some posts about TDD in the past:
 - [Tests are not a way out of misery]({% post_url 2019-03-28-tests-no-way-from-misery %})
 - [TDD tips]({% post_url 2017-03-06-tdd-intro %})

But today I want to focus on one thing in particular: why it does not work.

First things first, what is TDD? I will go with a quick definition. 
It's a technique of writing code when you write tests before actual implementation. Ok but why? The reason is to have well-designed code.

What is well-designed code? It does not matter actually. 
What does matter is the goal is some design. The test is here just to guide you to it. 

But do you need this guide? Of course not. 
You can achieve the same design, as with TDD, by using your experience, by learning, or just randomly.


That's the reason why TDD might not be the best tool for you. If you are well educated and experienced then you don't need to write the a first to guide you.


But how exactly are the tests guiding you to a better design? By definition it is by forcing you to use your code before you write it. 
The premise is that you will write a better API of your code if you are acting as a consumer of this API first. The problem is that this premise is not always true.

Consider this example [found on Stackoverflow](https://stackoverflow.com/questions/42573000/java-unit-testing-method-that-uses-new-date-for-current-date)

```java
@Test 
public void daysUntilCurrentDate() {
    final long fakeCurrentDateInMillis = new Date(2017, 2, 1).getTime();
    new MockUp<System>() {
        @Mock long currentTimeMillis() { return fakeCurrentDateInMillis; }
    };
    A tested = new A();

    int daysSinceJan30 = tested.getDaysUntil(new Date(2017, 1, 30));

    assertEquals(2, daysSinceJan3O);
}
```
Let's say it was written before class `A` so with the TDD approach. Is `A` well designed? I personally don’t think so, because it's not clear where it gets its dates from, and it's not possible to inject another source of dates. But John, author of this code, thinks it's not a problem a this is the best design possible. Has TDD proved him wrong? Not at all, he just had to write an extra test.

And that is another reason why TDD is not working. It has a prerequisite of knowing good design principles before you write the test. If you don’t write a well-designed test it won’t guide you to a well-designed code and will be just an extra burden.

TDD will work only when you study the code design first and will stop working after you gain so much experience that you can create the same design without writing the guiding test first.







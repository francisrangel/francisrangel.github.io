---
layout: post
title:  "Be the Master of Time"
date:   2016-02-01
categories:
  - coding
  - java
  - object-orientation
---
Hello folks,

In this post I will talk about ways not to have a bad time - pun intended - writing date and time sensitive code.
I will use Java language for the examples, but the concepts can be implemented in your preferred language.

### The problem

The problem being tackled can be represented by this scenario:
you have to build a service that validates passes, so people can access a place.
It can be a party, a museum, a stadium or a concert, for instance.

We will consider that we receive a pass code and that we already retrieved
an object from the persistence layer.

### Domain representation

We are going to use minimal representation. Starting with the Pass, that has just a code and an expiration date.

{% highlight java %}
public class Pass {
    private final String code;
    private final Date expiration;

    public Pass(String code, Date expiration) {...}

    // Getters...
}
{% endhighlight %}

Then the service, that implements only one method:

{% highlight java %}
public class PassService {
    public boolean isValid(Pass pass) {
        return pass.getExpiration().compareTo(new Date()) >= 0;
    }
}
{% endhighlight %}

We use the *compareTo* method, so we can validate that the pass expiration date is larger than or equal today's date.
This means that the service returns true when expiration date is the current or a date in the future.
Meaning that our pass is valid.

### Testing the code

I like to unit test my code and I strongly recommend it.
It has at least two nice benefits:

* document what the software does 
* validate the code still works after changes or refactoring

Let's write some tests for the PassService.

Because dealing with dates can be ugly, I like to use the SimpleDateFormat and write a helper method to instantiate dates:

{% highlight java %}
private DateFormat format = new SimpleDateFormat("MM/dd/yyyy");

private Date date(String stringRepresentation) throws ParseException {
    return format.parse(stringRepresentation);
}
{% endhighlight %}

This way we can create dates like:

{% highlight java %}
date("01/25/2016") // January 25th, 2016
date("10/12/1995") // October 12th, 1995
// ... and so on
{% endhighlight %}

To write the test, usually what we do is to create a Pass object with a future date.
Usually 10 years is enough. After all, this software should not be running 10 years from now anyway!

{% highlight java %}
@Test
public void itValidatesAPass() throws Exception {
    Pass pass = new Pass("ABC123", date("01/10/2026"));

    assertThat(service.isValid(pass), is(true));
}
{% endhighlight %}

Funny thing about this is that I have seen programs outliving developer expectations.
And when that happens, you're going to have a hard time tracking why the test started to fail for no reason.
There's no changes to the test, the service or anything related. That's frustating!

When you find the reason, the first thing you think is: this could have been written in a better way.

### The better way

What I learnt and like to do in these cases is to abstract how I get today's date.
I create a Clock class, like this:

{% highlight java %}
public class Clock {
    public Date today() {
        return new Date();
    }
}
{% endhighlight %}

Then I use [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) to make sure my service receives an instance of Clock
and gets the date from it instead of the system:

{% highlight java %}
public class PassService {
    private final Clock clock;

    public PassService(Clock clock) {
        this.clock = clock;
    }

    public boolean isValid(Pass pass) {
        return pass.getExpiration().compareTo(clock.today()) >= 0;
    }
}
{% endhighlight %}

When testing the service, it receives a fake version of the clock, so I can change today's date.
I will use [Mockito](http://mockito.org/) library to deal with that:

{% highlight java %}
private Clock clock = Mockito.mock(Clock.class);
private PassService service = new PassService(clock);
{% endhighlight %}

Next step is to change my tests to change today's date based on what I need. Our previous test  would look like this:

{% highlight java %}
@Test
public void itValidatesAPassValidUntilToday() throws Exception {
    Pass pass = new Pass("ABC123", date("01/10/2016"));

    doReturn(date("01/10/2016")).when(clock).today();
    assertThat(service.isValid(pass), is(true));
}
{% endhighlight %}

Now I can even easily test other dates, like the day before.

{% highlight java %}
@Test
public void itValidatesAPassValidUntilTomorrow() throws Exception {
    Pass pass = new Pass("ABC123", date("01/10/2016"));

    doReturn(date("01/09/2016")).when(clock).today();
    assertThat(service.isValid(pass), is(true));
}
{% endhighlight %}

Of course we need a test for invalid passes:

{% highlight java %}
@Test
public void itRejectsAPassValidUntilYesterday() throws Exception {
    Pass pass = new Pass("ABC123", date("01/05/2016"));

    doReturn(date("01/06/2016")).when(clock).today();
    assertThat(service.isValid(pass), is(false));
}
{% endhighlight %}

### Conclusion

Tests are now guaranteed to work no matter how old the software is.
Dates are created in an easy and readable way.
We can mock the clock and test lots of scenarios without having to write complex code,
dealing with subtracting from or adding days or years to current date.

The scenario presented here is quite simple, but this idea can be
really good in a more complex software.
Especially if you need to deal with hours, minutes and/or seconds.

I hope this can help and please let me know in the comments if you have any questions!

See you all!


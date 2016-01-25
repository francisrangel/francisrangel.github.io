---
layout: post
title:  "Be the Master of Time"
date:   2015-12-13
categories:
- coding
- java
- object-orientation
---
Hello folks,

In this post I will talk about coding and ways to not have a bad time - pun intended - writing date and time sensitive code. I will use Java language for the examples, but the concepts can be implemented in your preferred language with little modifications.

### The problem

The problem being tackled can be represented by this scenario:
you are working on the system responsible for receiving customer payments, on an amazing company that cares A LOT about customers.

One of the key features is that the system should not allow customers to pay for the service more than once for each month.
Every time a customer pays for the service, the system generates a record with the end date for that payment.

Let's say I am a customer an I pay for the month of January, 2016.
The system will generate a record with end date as 01/31/2016.
This means I should not be able to make another payment before or at this date.

### Domain representation

We are going to use minimal representation. Starting with the Customer, that has just a name.

{% highlight java %}
class Customer {
    String name() { ... }
}
{% endhighlight %}

Then a representation for a payment, containing the end date and the amount paid.
The end date means that the service is still active and we cannot receive another payment before or at it.

{% highlight java %}
class Payment {
    Date endDate() {...}
    Money amount() {...}
}
{% endhighlight %}

The payment service would return a receipt, with the customer and the amount paid.

{% highlight java %}
class Receipt {
    Customer getCustomer() {...}
    Money getAmount() {...}
}

class PaymentService {
    Receipt receivePayment(Customer customer, Money amount) {...}
}
{% endhighlight %}

### Testing the code

I really believe unit testing is a good practice. It helps documenting code and also making sure you don't break things when changing code around. So, I want to unit test my PaymentService. I would like to test it can receive a payment. Something like the following:

{% highlight java %}
@Test
public void receivesAPayment() {
    PaymentService service = new PaymentService();

    Customer customer = new Customer("Francis Rangel");
    Money amount = new Money(BigInteger.TEN);

    Receipt receipt = service.receivePayment(customer, amount);

    assertThat(receipt.getCustomer(), equalTo(customer));
    assertThat(receipt.getAmount(), equalTo(amount));
}
{% endhighlight %}

Now we want to test our business rule, of not receiving another payment before the end date of an active one.

# Falsehoods considered harmful
`2021-02-04`

You've read about falsehoods programmers believe about [names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/), about [time](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time), about [addresses](https://www.mjt.me.uk/posts/falsehoods-programmers-believe-about-addresses/), and [so much more](https://github.com/kdeldycke/awesome-falsehood), you had a good chuckle, you learned something, you moved on. That was ten years ago.

[![Invalid last name](harmful-falsehoods/invalid-name.png)](https://twitter.com/mallory_yu/status/1346196380281405445)

We've all learned something. The world has become more and more digital. Surely, serious business are using proper libraries by now to handle messy data. Otherwise they would get flooded with complaints from customers. Things are good, right?

Enter fraud detection.

## Fraud detection

Let's say you want to buy a physical book online and have it delivered to your mom's house for her birthday. Your mom has a name, her house has an address, the name on her address matches her name, you have a name, you have a bank account, the name on your bank account matches your name. This should be easy.

You log in to the online store, register your bank account, put the book in the cart, complete your order, and receive an order confirmation email. It was easy.

One hour later you receive another email from the online store informing you that the payment method was rejected and that you need to s

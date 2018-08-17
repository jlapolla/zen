==============================================
Defensive programming is the root of all evil!
==============================================

Story Part 1
============

You wrote a fancy little square root function in Python. You were cute and
called it ``sqrt()``. Maybe it looks like this.

.. code:: python

  def sqrt(x):

      # Fancy square root code here!
      # ...
      return result

You're thinking ahead now, and you add a check to make sure ``x >= 0``.

.. code:: python

  def sqrt(x):

      if x < 0:
          raise Exception("domain error")

      # Fancy square root code here!
      # ...
      return result

You think to yourself "Ah, I've saved my clients from the perils of complex
numbers. Yay me!" You commit your code to the repository, and go home for a good
night's sleep.

The next day you waltz into work and find an email from the overseas development
team:

  Hi James,

  I tried your sqrt() function and it didn't do what I expected. Please take a
  look at my code and try it.

  Thanks!
  - Octavius

You open the code in the email attachment.

.. code:: python

  def frobnicate(nm, count):
      # Some frobnicating code here!
      # ...

      y = sqrt(nm)

      # More frobnication!
      # ...

      return y


  # Frobnicate the data.
  frobnicate("Bob", 32);

Hmm, they passed a string to your beautiful ``sqrt()`` function. Well, time to
protect those overseas developers from themselves again. You add another check
to your ``sqrt()`` function.

.. code:: python

  def sqrt(x):

      if x < 0:
          raise Exception("domain error")

      if isinstance(x, str):
          raise Exception("cannot pass a string to sqrt()")

      # Fancy square root code here!
      # ...
      return result

You commit your code and send a reply to Octavius.

  Hi Octavius,

  I took a look, and it looks like you're passing a string to sqrt(). You're not
  supposed to do that. I made it raise an exception in this case.

  Hope that works for you!
  - James

You go home, and sleep.

The next day, you have an email. It's Octavius again.

  Hi James,

  Thanks for that fix yesterday. I have a new problem now though. Please take a
  look at the attachment.

  Thanks!
  - Octavius

You wonder what could possibly go wrong with your beautiful ``sqrt()``
function. You open the code in the email attachment.

.. code:: python

  def barriza(arr):
      # Some barizza code here!
      # ...

      y = sqrt(arr)

      # More barizza stuff!
      # ...

      return y


  # Barizza the data.
  barizza([2, 4, 9]);

Hmm, Octavius passed a list of numbers to ``sqrt()``. I suppose he was expecting
it to return a list of the square roots of each number? Well, I guess that's
kind of reasonable. Well, let's update ``sqrt()`` to handle lists.

.. code:: python

  def sqrt(x):

      if x < 0:
          raise Exception("domain error")

      if isinstance(x, str):
          raise Exception("cannot pass a string to sqrt()")

      if isinstance(x, list):
          return map(sqrt, list)

      # Fancy square root code here!
      # ...
      return result

You go home that night, and tell your wife you got to use the ``map()`` builtin,
and dive into a discussion of the wonders of functional programming. Your wife
dozes off, bored to tears at your technical descriptions.

The next morning you come in ready to work on another function. But you have
another email. You grumble to yourself "Better not be Octavius again."


  Hi James,

  That sqrt() list feature you put in yesterday is really killer! Thanks! But
  for some reason it crashes in the attached file. The backtrace says it crashes
  in your sqrt() function. Please take a look!

  Thanks!
  - Octavius

Holy #*$%!, Octavius, what could it possibly be now!? You open the email
attachment.

.. code:: python

  def floopteedoo(arr):
      # Some floopteedoo code here!
      # ...

      y = sqrt(arr)

      # More floopteedoo stuff!
      # ...

      return y


  arr1 = [2, 4, 9, []]
  arr2 = [16, 64, arr1]
  arr1[3].append(arr2)

  # Floopteedoo the data.
  floopteedoo(arr2);

What the #*$%!? He passed a list with a circular reference to ``sqrt()``! What
was he thinking! I mean, at least passing a flat list kind of made sense, but
what the #*$%! does he expect ``sqrt()`` to return for this argument!?

You think about how you could add a case to ``sqrt()`` to track a set of arrays
you've already visited to detect and break the cycle. But this has gone too far.
It's time to put your foot down! You add some preconditions to ``sqrt()`` in the
comments.

.. code:: python

  # Square root of 'x'.
  #
  # Preconditions:
  #   (isinstance(x, int)) and (x >= 0)
  #   or (isinstance(x, list)) and x does not have a reference cycle
  #
  # Specification:
  #   - If x < 0, raises Exception "domain error".
  #   - If isinstance(x, str), raises Exception "cannot pass a string to
  #     sqrt()".
  #   - If isinstance(x, list), returns a list containing the square roots of
  #     the elements of x
  #   - Else, apply the fancy square root algorithm to x and return the result

  def sqrt(x):

      if x < 0:
          raise Exception("domain error")

      if isinstance(x, str):
          raise Exception("cannot pass a string to sqrt()")

      if isinstance(x, list):
          return map(sqrt, list)

      # Fancy square root code here!
      # ...
      return result

You look at your comment preconditions and behavior specification. It's bullet
proof. You think to yourself "Try to break my sqrt() now, Octavius!"

You send a nastygram to Octavius.

  Octavius,

  Please obey the preconditions.

You go home a little annoyed. The next morning you come in to work... and there
is not an email waiting for you. Thank God. You move on to some other code that
you needed to be working on two days ago, and that would have been finished if
it weren't for Octavius.

Story Part 2
============

A few days go blissfully by. The new guy, Terrance, broke the build on Wednesday
morning right before donut hour. It finally got fixed Thursday night. It's
Friday morning now. It's going to be a productive day.

You innocently peruse your email before you get started on "real work".

  Hey James,

  I noticed you changed sqrt() a few days ago, and it broke some of our tests.
  Can you please get the attached code working again?

  Thanks
  - Octavius

You recall a few days ago that you removed the check for ``isinstance(x, str)``.
That should be safe since it now says in the preconditions that ``x`` cannot be
a string. What could possibly go wrong with that change?

The thought crosses your mind to ignore Octavius entirely and get some useful
work done. You open the attachment anyway.

.. code:: python

  def raise_exception():
      sqrt("Bob")

  def kaloobanitize(alphasaurus_rex):
      # Advanced kaloobanitizing algorithm.
      if triceratops is None:
        raise_exception()
      else:
        return triceratops

This is absolutely mind boggling! He used ``sqrt("Bob")`` to raise an
exception!? You schedule a phone conference for next Tuesday night with the
overseas team. It's time to set Octavius straight.

You go home, and have a wonderful weekend, except the part where your daughter
vomitted oreo cookies all over the back seat. But, kids will be kids. Monday
rolls around, and you're not looking forward to staying late on Tuesday night.
But it must be done.

Finally the hour of the conference call is here. You've played through how the
conversation might go in your head. You thought you had thought of all the
possible idiotic things they might say, and how you could set them straight. But
then Octavius himself hits you with this gem of a sentence.

  Octavius:

  So, in your specification for sqrt(), we noticed it said that if we pass a
  string to sqrt(), it raises an exception. We wanted to reuse that code to
  raise exceptions when we kaloobanitize the dinosaurs. So, we used sqrt("Bob")
  to do that. Then suddenly, you stopped supporting that exception raising
  feature, and our tests broke. Can you bring the exception back, please?

"I never said that in the specification!" you bark back. But they insist. You
look back in the commit history, and lo and behold, there it is.

.. code:: python

  # Square root of 'x'.
  #
  # Preconditions:
  #   (isinstance(x, int)) and (x >= 0)
  #   or (isinstance(x, list)) and x does not have a reference cycle
  #
  # Specification:
  #   - If x < 0, raises Exception "domain error".
  #   - If isinstance(x, str), raises Exception "cannot pass a string to
  #     sqrt()".
  #   - If isinstance(x, list), returns a list containing the square roots of
  #     the elements of x
  #   - Else, apply the fancy square root algorithm to x and return the result

  def sqrt(x):
      ...

You:

  Okay, well I thought it was obvious that throwing the exception wasn't
  supposed to be a "feature" that you should use.

Octavius:

  What do you mean? Obvious? It's in the specification that sqrt() raises an
  exception if we pass a string. We thought that was a feature. How were we
  supposed to know!?

You:

  Look, I'm sorry, but that wasn't supposed to be a feature. You guys are
  going to have to change your code.

Octavius:

  Now hold on a sec. Are you telling me you aren't supporting backwards
  compatibility when you update sqrt()? Well if that's the case, then what
  features CAN we depend on!?

You:

  Look, guys, I'm sorry. Really. It's my bad. I should have communicated better.
  But you have to understand, that's not a feature that sqrt() supports. I'll
  comb through the specification again, and make sure this doesn't happen again.
  Okay?

Octavius gripes that that's not really a thorough answer, and the overseas team
is still unsure what aspects of ``sqrt()`` specification are features and what
aspects are quote-unquote "not features". But you assure them that you'll sort
it out in the morning, and it will all be crystal clear.

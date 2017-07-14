Designing your app with fragments the right way is supremely important. Many developers, after first learning about fragments, try to use them for every reusable component in their application. This is the wrong way to use fragments.

Fragments are intended to encapsulate major components in a reusable way. A major component in this case would be on the level of an entire screen of your application. If you have a significant number of fragments on screen at once, your code will be littered with fragment transactions and unclear responsibility.A better architectural solution for reuse with smaller components is to extract them into a custom view\(a class that subclassesViewor one of its subclasses\).

Use fragments responsibly. A good rule of thumb is to have no more than two or three fragments on the screen at a time \([Figure 7.21](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s07.html#fragment_architecture)\).



**Figure 7.21  Less is more**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_fragment_architecture.png "Less is more")

### THE REASON ALL OUR ACTIVITIES WILL USE FRAGMENTS

From here on, all of the apps in this book will use fragments – no matter how simple. This may seem like overkill. Many of the examples you will see in following chapters could be written without fragments. The UIs could be created and managed from activities, and doing so might even be less code.

However, we believe it is better for you to become comfortable with the pattern you will most likely use in real life.

You might think it would be better to begin a simple app without fragments and add them later, when \(or if\) necessary. There is an idea in Extreme Programming methodology called YAGNI. YAGNI stands for“You Aren’t Gonna Need It,”and it urges you not to write code if you think youmightneed it later. Why? Because YAGNI. It is tempting to say“YAGNI”to fragments.

Unfortunately, adding fragments later can be a minefield. Changing an activity to an activity hosting a UI fragment is not difficult, but there are swarms of annoying gotchas. Keeping some interfaces managed by activities and having others managed by fragments only makes things worse because you have to keep track of this meaningless distinction. It is far easier to write your code using fragments from the beginning and not worry about the pain and annoyance of reworking it later, or having to remember which style of controller you are using in each part of your application.

Therefore, when it comes to fragments, we have a different principle: AUF, or“Always Use Fragments.”You can kill a lot of brain cells deciding whether to use a fragment or an activity, and it is just not worth it. AUF!


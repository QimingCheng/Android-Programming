The singleton pattern, as used in theCrimeLab, is very common in Android. Singletons get a bad rap because they can be misused in a way that makes an app hard to maintain.

Singletons are often used in Android because they outlive a single fragment or activity. A singleton will still exist across rotation and will exist as you move between activities and fragments in your application.

Singletons make a convenient owner of your model objects. Imagine a more complex CriminalIntent application that had many activities and fragments modifying crimes. When one controller modifies a crime, how would you make sure that updated crime was sent over to the other controllers? If theCrimeLabis the owner of crimes and all modifications to crimes pass through it, propagating changes is much easier. As you transition between controllers, you can pass the crime ID as an identifier for a particular crime and have each controller pull the full crime object from theCrimeLabusing that ID.

However, singletons do have a few downsides. For example, while they allow for an easy place to stash data with a longer lifetime than a controller, singletons do have a lifetime. Singletons will be destroyed, along with all of their instance variables, as Android reclaims memory at some point after you switch out of an application. Singletons are not a long-term storage solution. \(Writing the files to disk or sending them to a web server is.\)

Singletons can also make your code hard to unit test. There is not a great way to replace theCrimeLabinstance in this chapter with a mock version of itself because the code is calling a static method directly on theCrimeLabobject. In practice, Android developers usually solve this problem using a tool called adependency injector. This tool allows for objects to be shared as singletons, while still making it possible to replace them when needed.

Singletons also have the potential to be misused. The temptation is to use singletons for everything because they are convenient – you can get to them wherever you are, and store whatever information you need to get at later. But when you do that, you are avoiding answering important questions: Where is this data used? Where is this method important?

A singleton does not answer those questions. So whoever comes after you will open up your singleton and find something that looks like somebody’s disorganized junk drawer. Batteries, zip ties, old photographs? What is all this here for? Make sure that anything in your singleton is truly global and has a strong reason for being there.

On balance, however, singletons are a key component of a well-architected Android app – when used correctly.


This book uses the support library implementation of fragments over the implementation built into the Android OS, which may seem like an unusual choice. After all, the support library implementation of fragments was initially created so that developers could use fragments on old versions of Android that do not support the API. Today, most developers can exclusively work with versions of Android that include support for fragments natively.

We still prefer support fragments. Why? Support fragments are superior because you can update the version of the support library in your application and ship a new version of your app at any time. New releases of the support library come out multiple times a year. When a new feature is added to the fragment API, that feature is also added to the support library fragment API along with any available bug fixes. To use this new goodness, just update the version of the support library in your application.

As an example, official support for fragment nesting \(hosting a fragment in a fragment\) was added in Android 4.2. If you are using the Android OS implementation of fragments and supporting Android 4.0 and newer, you cannot use this API on all devices that your app supports. If you are using the support library, you can update the version of the library in your app and nest fragments until you run out of memory on the device.

There are no significant downsides to using the support library’s fragments. The implementation of fragments is nearly identical in the support library as it is in the OS. The only real downside is that you have to include the support library in your project, and it has a nonzero size. However, it is currently under a megabyte – and you will likely use the support library for some of its other features as well.

We take a practical approach in this book and in our own application development. The support library is king.

If you are strong-willed and do not believe in the advice above, you can use the fragment implementation built into the Android OS.

To use standard library fragments, you would make three changes to the project:

* Subclass the standard libraryActivityclass \(android.app.Activity\) instead ofFragmentActivityorAppCompatActivity. Activities have support for fragments out of the box on API level 11 or higher.

* Subclassandroid.app.Fragmentinstead ofandroid.support.v4.app.Fragment.

* To get theFragmentManager, callgetFragmentManager\(\)instead ofgetSupportFragmentManager\(\).




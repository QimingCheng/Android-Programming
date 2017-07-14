In this chapter, you included the AppCompat library so that you can use support fragments. AppCompat on its own does not include a support fragment implementation. The AppCompat library depends on the support-v4 library, which is where the support fragment implementation lives.

Google provides many different support libraries, including support-v4, appcompat-v7, recyclerview-v7, and many more. The support-v4 library is typically referred to asthesupport library. This was the first support library Google provided to developers. Over time, more and more tools have been added to this library, and it became a grab bag of things with no real focus. At that point, Google decided to develop a suite of support libraries rather than a single library.

The support library \(support-v4\) contains the support implementation of fragments that you used in this chapter. For example, this is where you will find the source ofandroid.support.v4.app.Fragment. The support library also includes anActivitysubclass:FragmentActivity. To use support fragments, your activities must subclassFragmentActivity.

As shown in[Figure 7.22](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s08.html#appcompatactivity_hierarchy),AppCompatActivityis a subclass of thisFragmentActivityclass, which is how you were able to use support fragments in this chapter. If you were using support fragments without the AppCompat library, you would include the support-v4 dependency in your project and you would subclassFragmentActivityin each of your activity classes instead ofAppCompatActivity.



**Figure 7.22  AppCompatActivity class hierarchy**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/appcompatactivity_class_hierarchy.png "AppCompatActivity class hierarchy")

If all of this sounds confusing, that is because it is. But not to worry – most Android developers use these libraries as you did in this chapter: using the AppCompat library rather than the support library directly. You will learn more about the features of the AppCompat library in[Chapter 13](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch13.html).


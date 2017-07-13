In this chapter, you will start building an application named CriminalIntent.CriminalIntent records the details of“office crimes”– things like leaving dirty dishes in the breakroom sink or walking away from an empty shared printer after documents have printed.

With CriminalIntent, you can make a record of a crime including a title, a date, and a photo. You can also identify a suspect from your contacts and lodge a complaint via email, Twitter, Facebook, or another app. After documenting and reporting a crime, you can proceed with your work free of resentment and ready to focus on the business at hand.

CriminalIntent is a complex app that will take 13 chapters to complete. It will have a list-detail interface: The main screen will display a list of recorded crimes, and users will be able to add new crimes or select an existing crime to view and edit its details \([Figure 7.1](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07.html#fig_temporary_finale)\).

**Figure 7.1  CriminalIntent, a list-detail app**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_double_finished.png "CriminalIntent, a list-detail app")

## The Need for UI Flexibility

You might imagine that a list-detail application consists of two activities: one managing the list and the other managing the detail view. Clicking a crime in the list would start an instance of the detail activity. Pressing the Back button would destroy the detail activity and return you to the list, where you could select another crime.

That would work, but what if you wanted more sophisticated presentation and navigation between screens?

* Imagine that your user is running CriminalIntent on a tablet. Tablets and some larger phones have screens large enough to show the list and detail at the same time – at least in landscape orientation \([Figure 7.2](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07.html#fig_ci_phone_v_tablet)\).

**Figure 7.2  Ideal list-detail interface for phone and tablet**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/phone_v_tablet_list.png "Ideal list-detail interface for phone and tablet")

* Imagine the user is viewing a crime on a phone and wants to see the next crime in the list. It would be better if the user could swipe to see the next crime without having to return to the list. Each swipe should update the detail view with information for the next crime.

What these scenarios have in common isUI flexibility:the ability to compose and recompose an activity’s view at runtime depending on what the user or the device requires.

Activities were not built to provide this flexibility. An activity’s views may change at runtime, but the code to control those views must live inside the activity. As a result, activities are tightly coupled to the particular screen being used.


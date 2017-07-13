The steps to create a UI fragment are the same as those you followed to create an activity:

* compose a UI by defining widgets in a layout file

* create the class and set its view to be the layout that you defined

* wire up the widgets inflated from the layout in code

### DEFINING CRIMEFRAGMENT’S LAYOUT

CrimeFragment’s view will display the information contained within an instance ofCrime.

First, define the strings that the user will see inres/values/strings.xml.



**Listing 7.6  Adding strings \(`res/values/strings.xml`\)**

```
<
resources
>
<
string name="app_name"
>
CriminalIntent
<
/string
>
<
string name="crime_title_hint"
>
Enter a title for the crime.
<
/string
>
<
string name="crime_title_label"
>
Title
<
/string
>
<
string name="crime_details_label"
>
Details
<
/string
>
<
string name="crime_solved_label"
>
Solved
<
/string
>
<
/resources
>
```

Next, you will define the UI. The layout forCrimeFragmentwill consist of a verticalLinearLayoutthat contains twoTextViews, anEditText, aButton, and aCheckbox.

To create a layout file, right-click theres/layoutfolder in the project tool window and selectNew→Layout resource file. Name this filefragment\_crime.xmland enterLinearLayoutas the root element. ClickOKand Android Studio will generate the file for you.

When the file opens, navigate to the XML. The wizard has added theLinearLayoutfor you. Add the widgets that make up the fragment’s layout tofragment\_crime.xml.



**Listing 7.7  Layout file for fragment’s view \(`fragment_crime.xml`\)**

```
<
LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    
android:layout_margin="16dp"

    android:orientation="vertical"
>
<
TextView
style="?android:listSeparatorTextViewStyle"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="@string/crime_title_label"/
>
<
EditText
android:id="@+id/crime_title"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="@string/crime_title_hint"/
>
<
TextView
style="?android:listSeparatorTextViewStyle"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="@string/crime_details_label"/
>
<
Button
android:id="@+id/crime_date"
android:layout_width="match_parent"
android:layout_height="wrap_content"/
>
<
CheckBox
android:id="@+id/crime_solved"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="@string/crime_solved_label"/
>
<
/LinearLayout
>
```

Check theDesignview to see a preview of your fragment’s view \([Figure 7.14](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#ss_ci_ui_fragments_updated_layout_preview)\).



**Figure 7.14  Previewing updated crime fragment layout**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ss_ci_ui_fragments_updated_layout_preview.png "Previewing updated crime fragment layout")

\(The updatedfragment\_crime.xmlcode includes new syntax related to view style:`<style="?android:listSeparatorTextViewStyle"`. Fear not. You will learn the meaning behind this syntax in[the section calledStyles, themes, and theme attributes](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch09s03.html#sect-LayoutsAndWidgets-styles-themes-attrs)in[Chapter 9](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch09.html).\)

### CREATING THE CRIMEFRAGMENT CLASS

Right-click thecom.bignerdranch.android.criminalintentpackage and selectNew→Java Class. Name the classCrimeFragmentand clickOKto generate the class.

Now, turn this class into a fragment. UpdateCrimeFragmentto subclass theFragmentclass.



**Listing 7.8  Subclassing the`Fragment`class \(`CrimeFragment.java`\)**

```
public class CrimeFragment 
extends Fragment
 {

}

```

As you subclass theFragmentclass, you will notice that Android Studio finds two classes with theFragmentname. You will seeFragment \(android.app\)andFragment \(android.support.v4.app\). The android.appFragmentis the version of fragments built into the Android OS. You will use the support library version, so be sure to select theandroid.support.v4.app versionof theFragmentclass when you see the dialog, as shown in[Figure 7.15](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#ss_choose_fragment_class).



**Figure 7.15  Choosing the support library’s`Fragment`class**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_choose_support_fragment_class.png "Choosing the support library’s Fragment class")

Your code should match[Listing 7.9](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#pl_support_fragment_import).



**Listing 7.9  Supporting the`Fragment`import \(`CrimeFragment.java`\)**

```
package com.bignerdranch.android.criminalintent;

import android.support.v4.app.Fragment;

public class CrimeFragment extends Fragment {

}

```

If you do not see this dialog or the wrong fragment class was imported, you can manually import the correct class. If you have an import forandroid.app.Fragment, remove that line of code. Import the correctFragmentclass with the Option+Return \(or Alt+Enter\) shortcut. Be sure to select the support version of theFragmentclass.

#### Implementing fragment lifecycle methods

CrimeFragmentis a controller that interacts with model and view objects. Its job is to present the details of a specific crime and update those details as the user changes them.

In GeoQuiz, your activities did most of their controller work in activity lifecycle methods. In CriminalIntent, this work will be done by fragments in fragment lifecycle methods. Many of these methods correspond to theActivitymethods you already know, such asonCreate\(Bundle\).

InCrimeFragment.java, add a member variable for theCrimeinstance and an implementation ofFragment.onCreate\(Bundle\).

Android Studio can provide some assistance when overriding methods. As you define theonCreate\(Bundle\)method, type the first few characters of the method name where you want to place the method. Android Studio will provide a list of suggestions, as shown in[Figure 7.16](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#ss_override_oncreate).



**Figure 7.16  Overriding the`onCreate(Bundle)`method**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_override_oncreate.png "Overriding the onCreate\(Bundle\) method")

Press Return to select theonCreate\(Bundle\)method, and Android Studio will create the method declaration for you. Update your code to create a newCrime, matching[Listing 7.10](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#add_model_object_instance).



**Listing 7.10  Overriding`Fragment.onCreate(Bundle)`\(`CrimeFragment.java`\)**

```
public class CrimeFragment extends Fragment {
    
private Crime mCrime;
@Override
public void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
mCrime = new Crime();
}

}

```

There are a couple of things to notice in this implementation. First,Fragment.onCreate\(Bundle\)is a public method, whereasActivity.onCreate\(Bundle\)is protected.Fragment.onCreate\(Bundle\)and otherFragmentlifecycle methods must be public, because they will be called by whatever activity is hosting the fragment.

Second, similar to an activity, a fragment has a bundle to which it saves and retrieves its state. You can overrideFragment.onSaveInstanceState\(Bundle\)for your own purposes just as you can overrideActivity.onSaveInstanceState\(Bundle\).

Also, note what doesnothappen inFragment.onCreate\(Bundle\):You do not inflate the fragment’s view. You configure thefragment instanceinFragment.onCreate\(Bundle\), but you create and configure thefragment’s viewin another fragment lifecycle method:

```
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
        Bundle savedInstanceState)

```

This method is where you inflate the layout for the fragment’s view and return the inflatedViewto the hosting activity. TheLayoutInflaterandViewGroupparameters are necessary to inflate the layout. TheBundlewill contain data that this method can use to re-create the view from a saved state.

InCrimeFragment.java, add an implementation ofonCreateView\(…\)that inflatesfragment\_crime.xml. You can use the same trick from[Figure 7.16](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#ss_override_oncreate)to fill out the method declaration.



**Listing 7.11  Overriding`onCreateView(…)`\(`CrimeFragment.java`\)**

```
public class CrimeFragment extends Fragment {
    private Crime mCrime;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mCrime = new Crime();
    }

    
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container,
Bundle savedInstanceState) {
View v = inflater.inflate(R.layout.fragment_crime, container, false);
return v;
}

}

```

WithinonCreateView\(…\), you explicitly inflate the fragment’s view by callingLayoutInflater.inflate\(…\)and passing in the layout resource ID.The second parameter is your view’s parent, which is usually needed to configure the widgets properly. The third parameter tells the layout inflater whether to add the inflated view to the view’s parent.You pass in`false`because you will add the view in the activity’s code.

#### Wiring widgets in a fragment

You are now going to hook up theEditText,Checkbox, andButtonin your fragment. TheonCreateView\(…\)method is the place to wire up these widgets.

Start with theEditText. After the view is inflated, get a reference to theEditTextand add a listener.



**Listing 7.12  Wiring up the`EditText`widget \(`CrimeFragment.java`\)**

```
public class CrimeFragment extends Fragment {
    private Crime mCrime;
    
private EditText mTitleField;

    ...
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
            Bundle savedInstanceState) {
        View v = inflater.inflate(R.layout.fragment_crime, container, false);

        
mTitleField = (EditText) v.findViewById(R.id.crime_title);
mTitleField.addTextChangedListener(new TextWatcher() {
@Override
public void beforeTextChanged(
CharSequence s, int start, int count, int after) {
// This space intentionally left blank
}
@Override
public void onTextChanged(
CharSequence s, int start, int before, int count) {
mCrime.setTitle(s.toString());
}
@Override
public void afterTextChanged(Editable s) {
// This one too
}
});


        return v;
    }
}
```

Getting references inFragment.onCreateView\(…\)works nearly the same as inActivity.onCreate\(Bundle\). The only difference is that you callView.findViewById\(int\)on the fragment’s view. TheActivity.findViewById\(int\)method that you used before is a convenience method that callsView.findViewById\(int\)behind the scenes. TheFragmentclass does not have a corresponding convenience method, so you have to call the real thing.

Setting listeners in a fragment works exactly the same as in an activity. In[Listing 7.12](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#update_crime_title), you create an anonymous class that implements the verboseTextWatcherinterface.TextWatcherhas three methods, but you only care about one:onTextChanged\(…\).

InonTextChanged\(…\), you calltoString\(\)on theCharSequencethat is the user’s input. This method returns a string, which you then use to set theCrime’s title.

Next, connect theButtonto display the date of the crime, as shown in[Listing 7.13](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#pl_watson_scene_activity_date).



**Listing 7.13  Setting`Button`text \(`CrimeFragment.java`\)**

```
public class CrimeFragment extends Fragment {
    private Crime mCrime;
    private EditText mTitleField;
    
private Button mDateButton;

    ...
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
            Bundle savedInstanceState) {
        View v = inflater.inflate(R.layout.fragment_crime, container, false);
        ...
        
mDateButton = (Button) v.findViewById(R.id.crime_date);
mDateButton.setText(mCrime.getDate().toString());
mDateButton.setEnabled(false);


        return v;
    }
}
```

Disabling the button ensures that it will not respond in any way to the user pressing it. It also changes its appearance to advertise its disabled state. In[Chapter 12](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch12.html), you will enable the button and allow the user to choose the date of the crime.

Moving on to theCheckBox, get a reference and set a listener that will update themSolvedfield of theCrime, as shown in[Listing 7.14](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#pl_watson_scene_activity_checkbox).



**Listing 7.14  Listening for`CheckBox`changes \(`CrimeFragment.java`\)**

```
public class CrimeFragment extends Fragment {
    private Crime mCrime;
    private EditText mTitleField;
    private Button mDateButton;
    
private CheckBox mSolvedCheckBox;

    ...
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
            Bundle savedInstanceState) {
        View v = inflater.inflate(R.layout.fragment_crime, container, false);
        ...
        
mSolvedCheckBox = (CheckBox)v.findViewById(R.id.crime_solved);
mSolvedCheckBox.setOnCheckedChangeListener(new OnCheckedChangeListener() {
@Override
public void onCheckedChanged(CompoundButton buttonView,
boolean isChecked) {
mCrime.setSolved(isChecked);
}
});


        return v;
    }
}
```

After typing in the code as above, click onOnCheckedChangeListener:

```
    mSolvedCheckBox.setOnCheckedChangeListener(new 
OnCheckedChangeListener
()

```

and use the Option+Return \(Alt+Enter\) shortcut to add the necessary import statement. You will be presented with two options. Choose theandroid.widget.CompoundButtonversion.

Depending on which version of Android Studio you are using, the autocomplete feature may insert`CompoundButton.OnCheckedChangeListener()`instead of leaving the code as`OnCheckedChangeListener()`. Either implementation is fine. But to remain consistent with the solution presented in this book, click on`CompoundButton`and hit Option+Return \(Alt+Enter\).

Select the option toAdd on demand static import for 'android.widget.CompoundButton'\([Figure 7.17](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#ss_ci_ui_fragments_static_import_compound_button)\). This will update the code so it matches[Listing 7.14](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s05.html#pl_watson_scene_activity_checkbox).



**Figure 7.17  Adding on demand static import**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ss_ci_ui_fragments_static_import_compound_button.png "Adding on demand static import")

Your code forCrimeFragmentis now complete. It would be great if you could run CriminalIntent now and play with the code you have written. But you cannot. Fragments cannot put their views on screen on their own. To realize your efforts, you first have to add aCrimeFragmenttoCrimeActivity.


In this chapter, you are going to start on the detail part of CriminalIntent.[Figure 7.4](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_empty_episode_1)shows you what CriminalIntent will look like at the end of this chapter.

**Figure 7.4  CriminalIntent at the end of this chapter**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_end_of_first_chapter.png "CriminalIntent at the end of this chapter")

The screen shown in[Figure 7.4](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_empty_episode_1)will be managed by a UI fragment namedCrimeFragment. An instance ofCrimeFragmentwill behostedby an activity namedCrimeActivity.

For now, think of hosting as the activity providing a spot in its view hierarchy where the fragment can place its view \([Figure 7.5](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_with_fragment)\).A fragment is incapable of getting a view on screen itself. Only when it is placed in an activity’s hierarchy will its view appear.



**Figure 7.5  `CrimeActivity`hosting a`CrimeFragment`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_with_fragment_one_canvas_new.png "CrimeActivity hosting a CrimeFragment")

CriminalIntent will be a large project, and one way to keep your head wrapped around a project is with an object diagram.[Figure 7.6](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_object_diagram_mvc)gives you the big picture of CriminalIntent. You do not have to memorize these objects and their relationships, but it is good to have an idea of where you are heading before you start.

You can see thatCrimeFragmentwill do the sort of work that your activities did in GeoQuiz: create and manage the UI and interact with the model objects.



**Figure 7.6  Object diagram for CriminalIntent \(for this chapter\)**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_object_diagram_mvc.png "Object diagram for CriminalIntent \(for this chapter\)")

Three of the classes shown in[Figure 7.6](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_object_diagram_mvc)are classes that you will write:Crime,CrimeFragment, andCrimeActivity.

An instance ofCrimewill represent a single office crime. In this chapter, a crime will have a title, an ID, a date, and a`boolean`that indicates whether the crime has been solved. The title is a descriptive name, like“Toxic sink dump”or“Someone stole my yogurt!”The ID will uniquely identify an instance ofCrime.

For this chapter, you will keep things very simple and use a single instance ofCrime.CrimeFragmentwill have a member variable \(mCrime\) to hold this isolated incident.

CrimeActivity’s view will consist of aFrameLayoutthat defines the spot where theCrimeFragment’s view will appear.

CrimeFragment’s view will consist of aLinearLayoutwith a few child views inside of it, including anEditText, aButton, and aCheckBox.CrimeFragmentwill have member variables for each of these views and will set listeners on them to update the model layer when there are changes.

### CREATING A NEW PROJECT

Enough talk; time to build a new app. Create a new Android application \(File→New Project...\). Name the application CriminalIntent and make sure the company domain is`android.bignerdranch.com`, as shown in[Figure 7.7](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_new_project_1).



**Figure 7.7  Creating the CriminalIntent application**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_new_project_1.png "Creating the CriminalIntent application")

ClickNextand specify a minimum SDK of API 19: Android 4.4. Also ensure that only thePhone and Tabletapplication type is checked.

ClickNextagain to select the type of activity to add. ChooseEmpty Activityand continue along in the wizard.

In the final step of the New Project wizard, name the activityCrimeActivityand clickFinish\([Figure 7.8](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#fig_ci_new_project_4)\).



**Figure 7.8  Creating`CrimeActivity`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_new_project_4.png "Creating CrimeActivity")

### TWO TYPES OF FRAGMENTS

Fragments were introduced in API level 11 along with the first Android tablets and the sudden need for UI flexibility. You must choose which implementation of fragments that you want use:native fragmentsorsupport fragments.

The native implementation of fragments is built into the device that the user runs your app on. If you support many different versions of Android, each of those Android versions could have a slightly different implementation of fragments \(for example, a bug could be fixed in one version and not the versions prior to it\).The support implementation of fragments is built into a librarythat you include in your application. This means that each device you run your app on will depend on the same implementation of fragments no matter the Android version.

In CriminalIntent, you will use the support implementation of fragments. Detailed reasoning for this decision is laid out at the end of the chapter in[the section calledFor the More Curious: Why Support Fragments Are Superior](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s09.html).

### ADDING DEPENDENCIES IN ANDROID STUDIO

You will use the implementation of fragments that comes with theAppCompatlibrary. The AppCompat library is one of Google’s many compatibility libraries that you will use throughout this book. You will learn much more about the AppCompat library in[Chapter 13](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch13.html).

To use the AppCompat library, it must be included in your list of dependencies. Your project comes with twobuild.gradlefiles, one for the project as a whole and one for your app module. Open thebuild.gradlefile located in your app module.



**Listing 7.1  Gradle dependencies \(`app/build.gradle`\)**

```
apply plugin: 'com.android.application'

android {
    ...
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    ...
    
compile 'com.android.support:appcompat-v7:25.0.1'

    ...
}

```

In the current dependencies section of yourbuild.gradlefile, you should see something similar to[Listing 7.1](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#pl_ci_default_dependencies)that specifies that the project depends on all of the.jarfiles in itslibsdirectory. You will also see dependencies for other libraries that are automatically included when projects are created with Android Studio, most likely including the AppCompat library.

Gradle allows for the specification of dependencies that you have not copied into your project. When your app is compiled, Gradle will find, download, and include the dependencies for you. All you have to do is specify an exact string incantation and Gradle will do the rest.

If you do not have the AppCompat library listed in your dependencies, Android Studio has a tool to help you add the library and come up with this string incantation. Navigate to the project structure for your project \(File→Project Structure...\).

Select the app module on the left and theDependenciestab in the app module. The dependencies for the app module are listed here \([Figure 7.9](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#pl_project_structure)\).



**Figure 7.9  App dependencies**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_project_structure.png "App dependencies")

\(You may have additional dependencies specified. If you do, do not remove them.\)

You should see the AppCompat dependency listed. If you do not, add it with the+button and chooseLibrary dependency. Choose theappcompat-v7library from the list and clickOK\([Figure 7.10](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#pl_dependency_list)\).



**Figure 7.10  A collection of dependencies**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_dependency_list.png "A collection of dependencies")

Navigate back to the editor window showingapp/build.gradle, and you should now see AppCompat included, as shown in[Listing 7.1](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#pl_ci_default_dependencies).

\(If you modify this file manually, outside of the project structure window, you will need to sync your project with the Gradle file to reflect any updates that you have made. This sync asks Gradle to update the build based on your changes by either downloading or removing dependencies. Changes within the project structure window will trigger this sync automatically. To manually perform this sync, navigate toTools→Android→Sync Project with Gradle Files.\)

The dependency string`compile 'com.android.support:appcompat-v7:25.0.0'`uses the Maven coordinates format`groupId:artifactId:version`. \(Maven is a dependency management tool. You can learn more about it atmaven.apache.org/.\)

The`groupId`is the unique identifier for a set of libraries available on the Maven repository. Often the library’s base package name is used as the`groupId`, which is`com.android.support`for the AppCompat library.

The`artifactId`is the name of a specific library within the package. In this case, the name of the library you are referring to is`appcompat-v7`.

Last but not least, the`version`represents the revision number of the library. CriminalIntent depends on the 25.0.0 version of the appcompat-v7 library. Version 25.0.0 is the latest version as of this writing, but any version newer than that should also work for this project. In fact, it is a good idea to use the latest version of the support library so that you can use newer APIs and receive the latest bug fixes. If Android Studio added a newer version of the library for you, do not roll it back to the version shown above.

Now that the AppCompat library is a dependency in the project, make sure that your project uses it. In the project tool window, find and openCrimeActivity.java. Verify thatCrimeActivity’s superclass isAppCompatActivity.



**Listing 7.2  Tweaking template code \(`CrimeActivity.java`\)**

```
public class CrimeActivity extends 
AppCompatActivity
 {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_crime);
    }

}

```

Before proceeding withCrimeActivity, let’s create the model layer for CriminalIntent by writing theCrimeclass.

### CREATING THE CRIME CLASS

In the project tool window, right-click thecom.bignerdranch.android.criminalintentpackage and selectNew→Java Class. Name the classCrimeand clickOK.

InCrime.java, add fields to represent the crime’s ID, title, date, and status and a constructor that initializes the ID and date fields \([Listing 7.3](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s03.html#add_crime_class)\).



**Listing 7.3  Adding to`Crime`class \(`Crime.java`\)**

```
public class Crime {

    
private UUID mId;
private String mTitle;
private Date mDate;
private boolean mSolved;
public Crime() {
mId = UUID.randomUUID();
mDate = new Date();
}

}

```

UUIDis a Java utility class included in the Android framework. It provides an easy way to generate universally unique ID values. In the constructor you generate a random unique ID by calling`UUID.randomUUID()`.

Android Studio may find two classes with the nameDate. Use the Option+Return \(or Alt+Enter\) shortcut to manually import the class. When asked which version of theDateclass to import, choose thejava.util.Dateversion.

Initializing theDatevariable using the defaultDateconstructor setsmDateto the current date. This will be the default date for a crime.

Next, you want to generate a getter for the read-onlymIdand both a getter and setter formTitle,mDate, andmSolved. Right-click after the constructor and selectGenerate...→Getterand select themIdvariable. Then, generate the getter and setter formTitle,mDate, andmSolvedby repeating the process, but selectingGetter and Setterin theGenerate...menu.



**Listing 7.4  Generated getters and setters \(`Crime.java`\)**

```
public class Crime {

    private UUID mId;
    private String mTitle;
    private Date mDate;
    private boolean mSolved;

    public Crime() {
        mId = UUID.randomUUID();
        mDate = new Date();
    }

    
public UUID getId() {
return mId;
}
public String getTitle() {
return mTitle;
}
public void setTitle(String title) {
mTitle = title;
}
public Date getDate() {
return mDate;
}
public void setDate(Date date) {
mDate = date;
}
public boolean isSolved() {
return mSolved;
}
public void setSolved(boolean solved) {
mSolved = solved;
}

}

```

That is all you need for theCrimeclass and for CriminalIntent’s model layer in this chapter.

At this point, you have created the model layer and an activity that is capable of hosting a support fragment. Now you will get into the details of how the activity performs its duties as host.


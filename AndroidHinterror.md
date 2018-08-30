Do not add during buiding apk

android {

...

    lintOptions {
        checkReleaseBuilds false
        abortOnError false
    }
    
...
}

Run lint from the command line to find the REAL ISSUE
If you're using Android Studio or Gradle, you can use the Gradle wrapper to invoke the lint task for your project by entering one of the following commands from the root directory of your project:

On Windows:
gradlew lint
On Linux or Mac:
./gradlew lint

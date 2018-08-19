# Android App Java Coding Style Guide

This doc is only for improving Android App team members communicate and increasing code quality.

[TOC]

## Recommended reading
- [Effective Java](http://www.amazon.com/Effective-Java-Edition-Joshua-Bloch/dp/0321356683)

## Coding style

### Formatting
Recommand short cut:
Mac:
Command + Option + L
Command + Option + O

#### Indent style
Indent size is 2 columns.

    :::java
    // Like this.
    if (x < 0) {
      getUserById(x);
    } else {
      getUserByName(x);
    }

    // Not like this.
    if (x < 0)
      getUserById(x);

    // Also not like this.
    if (x < 0) getUserById(x);

Continuation indent is 4 columns.

    :::java
    // Bad.
    new String("Failed to process request" + request.getId()
        + " for user " + user.getId() + " query: '" + query.getText()
        + "'");

    // Good.
    // Each component of the message is separate and self-contained.
    // Adding or removing a component of the message requires minimal reformatting.
    new String("Failed to process"
        + " request " + request.getId()
        + " for user " + user.getId()
        + " query: '" + query.getText() + "'");

Don't break up a statement.

    :::java
    // Bad.
    final String firstName =
        otherValue;

    // Good.
    final String firstName = otherValue;

Method declaration continuations.

    :::java
    // Bad. Sub-optimal since line breaks are arbitrary and only filling lines.
    String downloadAnInternet(Internet internet, Tubes tubes,
        Blogosphere blogs, Amount<Long, Data> bandwidth) {
      tubes.download(internet);
      ...
    }

    // Acceptable, as the extra newline gives visual separation to the method body.
    public String downloadAnInternet(Internet internet,
                                     Tubes tubes,
                                     Blogosphere blogs,
                                     Amount<Long, Data> bandwidth) {

      tubes.download(internet);
      ...
    }

##### Chained method calls

    :::java
    // Bad.
    Iterable<Module> modules = ImmutableList.<Module>builder().add(new LifecycleModule())
        .add(new AppLauncherModule()).addAll(application.getModules()).build();

    // Good.
    // Method calls are isolated to a line.
    // The proper location for a new method call is unambiguous.
    Iterable<Module> modules = ImmutableList.<Module>builder()
        .add(new LifecycleModule())
        .add(new AppLauncherModule())
        .addAll(application.getModules())
        .build();

#### No tabs
Use tab please and format code.

#### 100 column limit
"You should follow the convention set by the body of code you are working with.
We tend to use 100 columns for a balance between fewer continuation lines but still easily
fitting two editor tabs side-by-side on a reasonably-high resolution display."

#### CamelCase for types, camelCase for variables, UPPER_SNAKE for constants

#### No comments on top
Do not left personal info or any other comments here, please remember we are a team.
This is not anyone's show.

### Field, class, and method declarations

##### Modifier order

    :::java
    // Bad.
    final volatile private String value;

    // Good.
    private final volatile String value;

### Variable naming

#### Extremely short variable names should be reserved for instances like loop indices.

    :::java
    // Bad.
    class User {
      private final int a;
      private final String m;

      ...
    }

    // Good.
    class User {
      private final int ageInYears;
      private final String maidenName;

      ...
    }

#### Include units in variable names

    :::java
    // Bad.
    long pollInterval;
    int fileSize;

    // Good.
    long pollIntervalMs;
    int fileSizeGb.

    // Better.
    Amount<Long, Time> pollInterval;
    Amount<Integer, Data> fileSize;

#### Don't embed metadata in variable names
A variable name should describe the variable's purpose.  Adding extra information like scope and
type is generally a sign of a bad variable name.

Avoid embedding the field type in the field name.

    :::java
    // Bad.
    Map<Integer, User> idToUserMap;
    String valueString;

    // Good.
    Map<Integer, User> usersById;
    String value;

Also avoid embedding scope information in a variable.  Hierarchy-based naming suggests that a class
is too complex and should be broken apart.

    :::java
    // Bad.
    String _value;
    String mValue;

    // Good.
    String value;

### Space pad operators and equals.

    :::java
    // Bad.
    //   - This offers poor visual separation of operations.
    int foo=a+b+1;

    // Good.
    // Use shortcut to format code man!!!!
    int foo = a + b + 1;

### Be explicit about operator precedence
If you expect a specific operation ordering, make it obvious with parenthesis.
*** For business logic math fomular, please create a API call or function, for reuse and review.

    :::java
    // Bad.
    return a << 8 * n + 1 | 0;

    // Good.
    return (a << (8 * n) + 1) | 0;

It's even good to be *really* obvious.

    :::java
    if ((values != null) && (10 > values.size())) {
      ...
    }

### Documentation

The more visible a piece of code is (and by extension - the farther away consumers might be),
the more documentation is needed.

To write comments only when the code is extremely hard to explain how it runs.

#### "Remember your code is key not comments."
Your elementary school teacher was right - you should never start a statement this way.
Likewise, you shouldn't write documentation this way.

    :::java
    // Bad.
    /**
     * This is a class that implements a cache.  It does caching for you.
     */
    class Cache {
      ...
    }

    // Good.
    /**
     * A volatile storage for objects based on a key, which may be invalidated and discarded.
     */
    class Cache {
      ...
    }

#### Documenting a class
Documentation for a class may range from a single sentence
to paragraphs with code examples. Documentation should serve to disambiguate any conceptual
blanks in the API, and make it easier to quickly and *correctly* use your API.
A thorough class doc usually has a one sentence summary and, if necessary,
a more detailed explanation.
API only. No personal info.

    :::java
    /**
     * An RPC equivalent of a unix pipe tee.  Any RPC sent to the tee input is guaranteed to have
     * been sent to both tee outputs before the call returns.
     *
     * @param <T> The type of the tee'd service.
     */
    public class RpcTee<T> {
      ...
    }

#### Be professional
We've all encountered frustration when dealing with other libraries, but ranting about it doesn't
do you any favors.  Suppress the expletives and get to the point.

    :::java
    // Bad.
    // I hate xml/soap so much, why can't it do this for me!?
    try {
      userId = Integer.parseInt(xml.getField("id"));
    } catch (NumberFormatException e) {
      ...
    }

    // Good.
    // TODO(Jim): Tuck field validation away in a library.
    try {
      userId = Integer.parseInt(xml.getField("id"));
    } catch (NumberFormatException e) {
      ...
    }

#### Don't document overriding methods (usually)

    :::java
    interface Database {
      /**
       * Gets the installed version of the database.
       *
       * @return The database version identifier.
       */
      String getVersion();
    }

    // Bad.
    // Overriding method doc doesn't add anything.
    class PostgresDatabase implements Database {
      /**
       * Gets the installed version of the database.
       *
       * @return The database version identifier.
       */
      @Override
      public String getVersion() {
        ...
      }
    }

    // Good.
    class PostgresDatabase implements Database {
      @Override
      public int getVersion();
    }

#### Use javadoc features

##### No author tags
"Code can change hands numerous times in its lifetime, and quite often the original author of a
source file is irrelevant after several iterations.  We find it's better to trust commit
history and `OWNERS` files to determine ownership of a body of code."

### Imports

#### No wildcard imports

    :::java
    // Bad.
    // Where did Foo come from?
    import com.twitter.baz.foo.*;
    import com.twitter.*;

    interface Bar extends Foo {
      ...
    }

    // Good.
    import com.twitter.baz.foo.BazFoo;
    import com.twitter.Foo;

    interface Bar extends Foo {
      ...
    }

### Use annotations wisely

#### @Nullable
By default - disallow `null`.  When a variable, parameter, or method return value may be `null`,
be explicit about it by marking
[@Nullable](http://code.google.com/p/jsr-305/source/browse/trunk/ri/src/main/java/javax/annotation/Nullable.java?r=24).
This is advisable even for fields/methods with private visibility.

    :::java
    class Database {
      @Nullable private Connection connection;

      @Nullable
      Connection getConnection() {
        return connection;
      }

      void setConnection(@Nullable Connection connection) {
        this.connection = connection;
      }
    }


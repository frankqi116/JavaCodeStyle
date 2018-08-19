# Android App Java Coding Style Guide

This doc is only for improving Android App team members communicate and increasing code quality.

## Recommended reading
- [Effective Java](http://www.amazon.com/Effective-Java-Edition-Joshua-Bloch/dp/0321356683)

## Coding style

### Formatting

Recommand short cut:

For Mac:

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

## Best practices

### Defensive programming

#### Preconditions

As a convention, object parameters to public constructors and methods should
always be checked against null, unless null is explicitly allowed.

    :::java
    // Bad.
    class AsyncFileReader {
      void readLater(File file, Closure<String> callback) {
        scheduledExecutor.schedule(new Runnable() {
          @Override public void run() {
            callback.execute(readSync(file));
          }
        }, 1L, TimeUnit.HOURS);
      }
    }

    // Good.
    class AsyncFileReader {
      void readLater(File file, Closure<String> callback) {
        checkNotNull(file);
        checkArgument(file.exists() && file.canRead(), "File must exist and be readable.");
        checkNotNull(callback);

        scheduledExecutor.schedule(new Runnable() {
          @Override public void run() {
            callback.execute(readSync(file));
          }
        }, 1L, TimeUnit.HOURS);
      }
    }

#### Minimize visibility

In a class API, you should support access to any methods and fields that you make accessible.

    :::java
    public class Parser {
      // Bad.
      public Map<String, String> rawFields;

      // Bad.
      public String readConfigLine() {
        ..
      }
    }

    // Good.
    class Parser {
      private final Map<String, String> rawFields;

      private String readConfigLine() {
        ..
      }
    }

#### Be wary of null
Use `@Nullable` where prudent.

#### Clean up with finally

    :::java
    FileInputStream in = null;
    try {
      ...
    } catch (IOException e) {
      ...
    } finally {
      Closeables.closeQuietly(in);
    }

Examples:

    :::java
    // Bad.
    mutex.lock();
    throw new NullPointerException();
    // Mutex is never unlocked.
    mutex.unlock();

    // Good.
    mutex.lock();
    try {
      throw new NullPointerException();
    } finally {
      mutex.unlock();
    }

    // Bad.
    if (receivedBadMessage) {
      conn.sendMessage("Bad request.");
      // Connection is not closed if sendMessage throws.
      conn.close();
    }

    // Good.
    if (receivedBadMessage) {
      try {
        conn.sendMessage("Bad request.");
      } finally {
        conn.close();
      }
    }


### Clean code

#### Disambiguate
Favor readability - if there's an ambiguous and unambiguous route, always favor unambiguous.

    :::java
    // Bad.
    // Depending on the font, it may be difficult to discern 1001 from 100l.
    long count = 100l + n;

    // Good.
    long count = 100L + n;

#### Remove dead code
Delete unused code (imports, fields, parameters, methods, classes).

#### Always use type parameters

We conventionally include type parameters on every declaration where the type is parameterized.
Even if the type is unknown, it's preferable to include a wildcard or wide type.

#### Avoid typecasting
Typecasting is a sign of poor class design, and can often be avoided.  An obvious exception here is
overriding
[equals](http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object)).

#### Exceptions
##### Catch narrow exceptions
Sometimes when using try/catch blocks, it may be tempting to just `catch Exception`, `Error`,
or `Throwable` so you don't have to worry about what type was thrown.  This is usually a bad idea,
as you can end up catching more than you really wanted to deal with.  For example,
`catch Exception` would capture `NullPointerException`, and `catch Throwable` would capture
`OutOfMemoryError`.

    :::java
    // Bad.
    try {
      storage.insertUser(user);
    } catch (Exception e) {
      LOG.error("Failed to insert user.");
    }

    try {
      storage.insertUser(user);
    } catch (StorageException e) {
      LOG.error("Failed to insert user.");
    }

##### Don't swallow exceptions
An empty `catch` block is usually a bad idea, as you have no signal of a problem.  Coupled with

##### When interrupted, reset thread interrupted state
Many blocking operations throw
[InterruptedException](http://docs.oracle.com/javase/7/docs/api/java/lang/InterruptedException.html)
so that you may be awaken for events like a JVM shutdown.  When catching `InterruptedException`,
it is good practice to ensure that the thread interrupted state is preserved.

IBM has a good [article](http://www.ibm.com/developerworks/java/library/j-jtp05236/index.html) on
this topic.

    :::java
    // Bad.
    //   - Surrounding code (or higher-level code) has no idea that the thread was interrupted.
    try {
      lock.tryLock(1L, TimeUnit.SECONDS)
    } catch (InterruptedException e) {
      LOG.info("Interrupted while doing x");
    }

    // Good.
    //   - Interrupted state is preserved.
    try {
      lock.tryLock(1L, TimeUnit.SECONDS)
    } catch (InterruptedException e) {
      LOG.info("Interrupted while doing x");
      Thread.currentThread().interrupt();
    }

##### Throw appropriate exception types
Let your API users obey [catch narrow exceptions](#catch-narrow-exceptions), don't throw Exception.
Even if you are calling another naughty API that throws Exception, at least hide that so it doesn't
bubble up even further.  You should also make an effort to hide implementation details from your
callers when it comes to exceptions.

    :::java
    // Bad.
    interface DataStore {
      String fetchValue(String key) throws Exception;
    }

    // Good.
    // A custom exception type insulates the user from the implementation.
    // Different implementations aren't forced to abuse irrelevant exception types.
    interface DataStore {
      String fetchValue(String key) throws StorageException;

      static class StorageException extends Exception {
        ...
      }
    }

### Use newer/better libraries

#### StringBuilder over StringBuffer
Please just do it. This will help other people.

#### List over Vector

### equals()

### TODOs

#### Leave TODOs early and often
A TODO isn't a bad thing - it's signaling a future developer (possibly yourself) that a
consideration was made, but omitted for various reasons.  It can also serve as a useful signal when
debugging.

Somtimes, we need this to mark somthing for further research and dvelop.

#### Leave no TODO unassigned
TODOs should have owners, otherwise they are unlikely to ever be resolved.

    :::java
    // Bad.
    // TODO: Implement request backoff.

    // Good.
    // TODO(Zhao Ge): Implement request back-end API.

#### In classes


#### In methods
One method does one thing and one thing only. Do not add condition for differnt business logic.
In the extreme, never do this:

    :::java
    void calculate(Subject subject) {
      double weight;
      if (useWeightingService(subject)) {
        try {
          weight = weightingService.weight(subject.id);
        } catch (RemoteException e) {
          throw new LayerSpecificException("Failed to look up weight for " + subject, e)
        }
      } else {
        weight = defaultInitialRate * (1 + onlineLearnedBoost);
      }

      // Use weight here for further calculations
    }

Instead do this:

    :::java
    void calculate(Subject subject) {
      double weight = calculateWeight(subject);

      // Use weight here for further calculations
    }

    private double calculateWeight(Subject subject) throws LayerSpecificException {
      if (useWeightingService(subject)) {
        return fetchSubjectWeight(subject.id)
      } else {
        return currentDefaultRate();
      }
    }

    private double fetchSubjectWeight(long subjectId) {
      try {
        return weightingService.weight(subjectId);
      } catch (RemoteException e) {
        throw new LayerSpecificException("Failed to look up weight for " + subject, e)
      }
    }

    private double currentDefaultRate() {
      defaultInitialRate * (1 + onlineLearnedBoost);
    }

### Don't Repeat Yourself ([DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself))
For a more long-winded discussion on this topic, read
[here](http://c2.com/cgi/wiki?DontRepeatYourself).

#### Extract constants whenever it makes sense

#### Centralize duplicate logic in utility functions

### Avoid unnecessary code

#### Superfluous temporary variables.

    :::java
    // Bad.
    List<String> strings = fetchStrings();
    return strings;

    // Good.
    return fetchStrings();

#### Unneeded assignment.

    :::java
    // Bad.
    String value = null;
    try {
      value = "The value is " + parse(foo);
    } catch (BadException e) {
      throw new IllegalStateException(e);
    }

    // Good
    String value;
    try {
      value = "The value is " + parse(foo);
    } catch (BadException e) {
      throw new IllegalStateException(e);
    }

### The 'fast' implementation

    :::java
    int fastAdd(Iterable<Integer> ints);

    // Why would the caller ever use this when there's a 'fast' add?
    int add(Iterable<Integer> ints);


Thank you.
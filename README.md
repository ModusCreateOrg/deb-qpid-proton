![No longer maintained](https://img.shields.io/badge/Maintenance-OFF-red.svg)
### [DEPRECATED] This repository is no longer maintained
> While this project is fully functional, the dependencies are no longer up to date. You are still welcome to explore, learn, and use the code provided here.
>
> Modus is dedicated to supporting the community with innovative ideas, best-practice patterns, and inspiring open source solutions. Check out the latest [Modus Labs](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated) projects.

[![Modus Labs](https://res.cloudinary.com/modus-labs/image/upload/h_80/v1531492623/labs/logo-black.png)](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated)

---

Proton is a library for speaking AMQP, including:

  + The AMQP Messenger API, a simple but powerful interface to send and receive
    messages over AMQP.
  + The AMQP Protocol Engine, a succinct encapsulation of the full
    AMQP protocol machinery.

Proton is designed for maximum embeddability:

  + minimal dependencies
  + minimal assumptions about application threading model

Proton is designed to scale up and down:

  + transparently supports both simple peer to peer messaging and complex
    globally federated topologies

Proton is multi-lingual:

  + Proton-C - a C implementation with lanuage bindings in Python, Php, Perl,
    Ruby, and Java (via JNI).
  + Proton-J - a pure Java implementation

Please see http://qpid.apache.org/proton for a more info.

==== Build Instructions ====

Proton has two separate build systems reflecting the nature of its
two implementations.

   + Proton-C and the language bindings use CMake.
   + Proton-J uses Maven.

The two build systems are independent of one and other, that is, Proton-C
may be built independently of Proton-J, and vice-versa.

=== Proton-C ===

== Build Instructions (Linux) ==

The following prerequesuites are required to do a full build. If you do not
wish to build a given language binding you can omit the package for that
language:

  # required dependencies
  yum install gcc cmake libuuid-devel

  # dependencies needed for ssl support
  yum install openssl-devel

  # dependencies needed for bindings
  yum install swig python-devel ruby-devel php-devel perl-devel \
              java-1.6.0-openjdk

  # dependencies needed for python docs
  yum install epydoc

From the directory where you found this README file:

  mkdir build
  cd build

  # Set the install prefix. You may need to adjust depending on your
  # system.
  cmake -DCMAKE_INSTALL_PREFIX=/usr ..

  # Omit the docs target if you do not wish to build or install
  # documentation.
  make all docs

  # Note that this step will require root privileges.
  make install

Note that all installed files are stored in the install_manifest.txt
file.

NOTE: The CMAKE_INSTALL_PREFIX does not affect the location for where the language
bindings (Ruby, Perl, PHP, Ruby) are installed. For those elements, the location is
determined by the language itself; i.e., each one is interrogated for the proper
location for extensions.

NOTE: If you want to constrain where the Proton code is installed, you have to use
the DESTDIR argument to make:

  make install DESTDIR=[location]

This will install all components (including the language bindings) in locations
relative to the specified location. So, for example, if the Perl language bindings
would normally install to /usr/lib/perl5/vendor_perl/ then, if you use:

  make install DESTDIR=/opt

the bindings would install to /opt/usr/lib/perl5/vendor_perl/ instead.

NOTE: The paths in libqpid-proton.pc are not updated if, when installing, DESTDIR
is used. The contents of that file are based on the values provided when the
CMake environment was created.

For more on the use of DESTDIR, see the following:

http://www.gnu.org/prep/standards/html_node/DESTDIR.html

== Disable Building The Language Bindings ==

To disable any language bindings, you can disable them individually with:

  cmake -DBUILD_[LANGUAGE]=OFF .

where [LANGUAGE] is one of JAVA, PERL, PHP, PYTHON or RUBY. To disable building all
language bindings then include a separate argument for each.

== Build Instructions (Windows) ==

This describes how to build the Proton library on Windows using Microsoft
Visual C++.

The Proton build uses the cmake tool to generate the Visual Studio project
files.  These project files can then be loaded into Visual Studio and used to
build the Proton library.

These instructions assume use of a command shell.  If you use the Visual
Studio supplied Command Prompt, cmake is even more likely to guess the
intended compiler.

The following packages must be installed:

    Visual Studio 2005 or newer (regular or C++ Express)
    Python (www.python.org)
    Cmake (www.cmake.org)

Optional: to run the python or java driven test suites

    swig (www.swig.org)

    Notes:
        - Be sure to install relevant Microsoft Service Packs and updates
        - python.exe _must_ be in your path
        - cmake.exe _must_ be in your path
        - swig.exe optional (but should be in your path for building test modules)


Step 1: Create a 'build' directory - this must be at the same level as the
        'proton-c' directory.  Example:

     From the directory where you found this README file:
     > mkdir build

Step 2: cd into the build directory

     > cd build

Step 3: Generate the Visual Studio project files using cmake.  The command contains

        1) the name of the compiler you are using (if cmake guesses wrongly)
        2) the path (required) to the _directory_ that contains the top level
           "CMakeLists.txt" file (the parent directory, in this case).
     Example:

     > cmake ..

     If cmake doesn't guess things correctly, useful additional arguments are:

        -G "Visual Studio 10"
        -DSWIG_EXECUTABLE=C:\swigwin-2.0.7\swig.exe

     Refer to the cmake documentation for more information.

Step 4: Load the ALL_BUILD project into Visual Studio

     4a: Run the Microsoft Visual Studio IDE
     4b: From within the IDE, open the ALL_BUILD project file or proton solution
         file - it should be in the 'build' directory you created above.
     4c: select the appropriate configuration.  RelWithDebInfo works best with
         the included CMake/CTest scripts

Step 5: Build the ALL_BUILD project.

Note that if you wish to build debug version of proton for use with swig
bindings on Windows, you must have the appropriate debug target libraries to
link against.

=== Proton-J ===

== Build Instructions (All platforms) ==

The following prerequesuites are required to do a full build.

  + Apache Maven 3.0 (or higher) (http://maven.apache.org/)

From the directory where you found this README file:

  # To compile and package all Java modules (omitting the tests)
  mvn -DskipTests package

  # To install the packages in the local Maven repository (usually ~/.m2/repo)
  mvn -DskipTests install

=== Testing ===

To test Proton, run the system tests (located in the tests subdirectory).
The system tests are applicable to both the Proton-C and Proton-J
implementations.

== Test Instructions (Proton-C only) ==

To run the system tests against Proton-C, from your build directory above:

   # to run all the tests, summary mode
   ctest

   # to run a single test, full output
   ctest -V -R proton-c

== Test Instructions (Proton-J and Proton-C) ==

To run the system tests, execute Maven specifying profile 'proton-j' to
test Proton-J, and 'proton-jni' to test the Proton-C implementation via the
JNI bindings.  (To test Proton-C via the JNI Bindings the JNI Binding must have
been built with Cmake as described above).

  # To test Proton-J
  mvn test -P proton-j

  # To test Proton-C via the JNI Bindings
  mvn test -P proton-jni

  # To produce a nicely formated report containing the test results
  # (in tests/target/site/surefire-report.html)
  mvn surefire-report:report


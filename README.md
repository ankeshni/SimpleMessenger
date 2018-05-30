# CSE 486/586: Distributed Systems

Link:https://www.cse.buffalo.edu/~eblanton/course/cse586/materials/2018S/simple-messenger.pdf

## Programming Assignment 1

## Simple Messenger

# Introduction

In this assignment, you will write a simple messenger app for Android. The purpose of this
assignment is to help you determine whether you have the right background for this course.
_If you can finish this assignment_ **_all by yourself, without help from others,_** _then it is probably the
case that you are prepared for this course._
The application you develop in this assignment will simply allow two Android devices to
send messages to each other. This will help you understand the basics of the Android en-
vironment, refresh your memory on network programming, help ease you into the Android
development tools, and introduce you to our testing environment.
There are four high-level challenges in this assignment:

- _Reading and understanding background material:_ You will need to read API documenta-
    tion, tutorials, and articles online in becoming familiar with Android development.
- _Infrastructure setup:_ You will have to install a number of software packages, configure
    them, and become familiar with their usage. You will also need to get plumbed into
    GitHub Classroom and UB Autograder.
- _Becoming comfortable with Android development:_ You will encounter roadblocks in de-
    veloping for Android, particularly if this is your first time. This is true for any platform
    or operating environment. Fortunately there is excellent documentation available for
    Android Studio and the Android APIs.
- _Working around Android virtual device restrictions:_ The AVD emulator has, in particular,
    restrictions on network communication for virtual devices. You will need to get used
    to this for this assignment and for this course.

# 1 Getting Started

Unless you are already familiar with Android development, I suggest that you start with
someAndroid tutorials. In addition, you should consultthe Android programming assign-
ment web pagefor this course and ensure that you have installed a compatible development
environment ( _e.g._ , Android Studio 3.0.1, Android SDK 19). **Please note** that there are some
known problems running Android Studio in some environments, and there are workarounds
on that page!

(^1) This assignment is based, with permission, on an assignment developed and used by Steve Ko in his version
of CSE 486/586. Large portions of text are used verbatim from that assignment.


The instructions for creating AVD images and configuring the emulator in various tuto-
rials will vary. Note that recent versions of Android Studio _require_ a 64-bit processor with
Intel VT-x or AMD-V virtualization extensions, OpenGL graphics support, and x86 emula-
tor images. This greatly speeds up emulation and debugging, but makes running Android
Studio in a hypervisor or on older hardware difficult.
The first tutorials to look at are:

- TheBuilding Your First Apptutorial is advisable for all students, even those with pre-
    vious Android experience, as a help in validating your Android development environ-
    ment. This tutorial will guide you through installing the necessary software and build-
    ing a simple application. _Note that we are using Android API version 19, and you should_
    _configure your build appropriately!_
- Next, look atManaging the Activity Lifecycle, which will help you understand some
    basic Android concepts that are used in the provided code and that will be required to
    successfully complete this assignment.

You will probably want to review some important Android documentation, as well, such
as:

- Application Fundamentals
- Activities
- Processes and Threads

To be successful _in this and all future programming assignments_ , it is critical that you un-
derstand the Android virtual device (AVD) emulator both for executing and debugging your
application. You will be using the emulator through Android Studio, standalone, and via the
grading scripts for this course. Please take the time to become familiar with it now! You
should review:

- Run Apps on the Android Emulator
- Debug Your App
- Logcat Command-line Tool

# 2 Setting up a Testing Environment

You will need to run two AVDs in order to test your app. Unfortunately, Android does not
provide a flexible networking environment for AVDs, so there are some hurdles to jump over
in order to set up the right environment. The following are the instructions.
These instructions will assume that your Android SDK directory (configured when you
set up Android Studio) is located in$ANDROID_SDK_ROOT, and that this variable is set appropri-
ately in your environment. This is required for things to work properly!


- You need to have the Android SDK and Python 2.x (not 3.x; Python 3.x versions are
    not compatible with the scripts provided.) installed on your machine. If you have not
    installed these, please do it first and proceed to the next step.
- SetANDROID_HOMEto$ANDROID_SDK_ROOTin your environment.
- Add the following directories to your path:
    **-** $ANDROID_SDK_ROOT/tools/bin
    **-** $ANDROID_SDK_ROOT/platform-tools
    **-** $ANDROID_SDK_ROOT/emulator

```
A good reference on how to change$PATHishere.
```
- Download and save these files somewhere convenient:
    **-** create_avd.py
    **-** run_avd.py
    **-** set_redir.py
- At this point, **make sure that your environment and path are set up correctly!**
- Create five AVDs by runningpython create_avd.py 5 $ANDROID_HOME.
    **-** You should run this command only once, and it may take quite some time to com-
       plete.
    **-** You may be asked to agree to a license agreement[y/N]; you will have to agree to
       continue.
    **-** YouwillbeaskedDo you wish to create a custom hardware profile [no]multiple
       times. _Do not enter anything when asked_ , the script will handle this automatically.
- At this point you should have five AVDs, namedavd0throughavd4. You can view and
    manipulate them in Android Studio, but please do not change or delete them as they
    are necessary for our scripts to work.
- Start a single emulator with the commandemulator @avd0. Wait for it to boot entirely,
    then click and hold the power button () until the power off menu appears on the em-
    ulated device. Click power off, then confirm, and wait for the device to shut down
    entirely (the emulator command will exit).
- For all of the programming assignments except this one, _you will need to run all five_
    _AVDs at the same time._ You will need to find a machine that can comfortably run five
    emulators simultaneously.
- In order to test this, runpython run_avd.py 5. The first time you do this, it may take a
    very long time and some emulators may fail to start. If so, wait until they have all made
    as much progress as possible (either booted or visibly frozen, typically), shut down the
    emulators, and try again.


- For this assignment, you will use only two AVDs, so you will start them withpython
    run_avd.py 2.
- After you successfully launch all five AVDs, run the commandpython set_redir 10000
    to create an emulated network connecting the five AVDs. There are some restrictions
    to how this works, discussed later in this document.
- After verifying your development environment, you can shut down the emulators.

# 3 The Simple Messenger App

The graded portion of this project is writing the simple messenger app. You will need to
download the project from GitHub Classroom (you should have received an invitation to our
classroom; if you have not, please contact me ASAP!) and open it in Android Studio.

1. Accept the invitation to GitHub Classroomto create your project repository.
2. Clone the GitHub your repository for SimpleMessenger. The repository URL will be
git@github.com/ub-cse586-s18/SimpleMessenger-$githubid, where$githubidis your Git-
Hub username. You can do this on the command line orin Android Studio.
3. Open the cloned project in Android Studio.
4. Take some time to understand the template code.
- The main Activity is inSimpleMessengerActivity.java.
- Please read both the code and the comments carefully!
5. Add your code to the cloned project, committing to GitHub as you make progress.
There are places in the template marked “TODO”; these are the places where you will
need to add your code.

## 3.1 Requirements

These are the project requirements. _You must follow these instructions exactly. If you do not,
you will receive_ **_no credit_** _for this assignment._ **When we say no credit, we mean it.** It is critical
that you follow directions precisely and accurately in this course.

1. There should be only one app that you develop and need to install for grading, and it
    should use the manifest provided in the template code. If you use the project template
    code, you should satisfy this requirement.
2. There should be only one text box on-screen where the user of the device can enter a
text message to be sent to the other device. If you use the project template code, you
should satisfy this requirement.


3. Each device should display on-screen what it has received. The project template con-
    tains basic code for displaying messages on the screen.
4. You need to use the Java Socket API.
5. All communication should be over TCP.
6. You may assume that the size of a message will never exceed 128 bytes (ASCII charac-
ters).

## 3.2 Communication

As mentioned above, the Android emulator is not very flexible in the networking facilities
that it provides for communication between AVDs. Althoughset_redir.pyenables network-
ing among multiple AVDs, it is very different from a typical networking setup. When you
write your socket code, you will have the following restrictions:

- In your app, you can open _only one_ server socket, which must listen on port 10000
    regardless of which AVD your app is running on.
- The app onavd0can connect to the listening server socket of the app onavd1by con-
    necting to 10.0.2.2:11112 (that is, IP address 10.0.2.2, port number 11112).
- The app onavd1can connect to the listening server socket of the app onavd0by con-
    necting to 10.0.2.2:11108.
- Your app knows which AVD it is running on via the following code snippet. IfportStr
    is 5554 , then it isavd0. IfportStris 5556 , then it isavd1:
       TelephonyManager tel =
          (TelephonyManager)this.getSystemService(Context.TELEPHONY_SERVICE);
       String portStr =
          tel.getLine1Number ().substring(tel.getLine1Number ().length () - 4);

```
The project template already implements the above code, but you are expected to understand
the code as this is critical for the rest of the programming assignments.
```
- In general,set_redir.pycreates an emulated, port-redirected network like this (VR
    stands for “virtual router”):


## 3.3 Testing

We have testing programs to help you see how your code does with our grading criteria. If
you find rough edges in the testing programs, please report it so the teaching staff can fix it.
The instructions for using the testing programs are the following:

1. Download a testing program for your platform. If your platform does not run it, please
    report it.

```
(a) Windows: Tested on 64-bit Windows 8.
(b) Linux: Tested on 64-bit Debian 9 and 64-bit Ubuntu 17.10 (see below for important
information about 64-bit systems).
(c) Mac OS: Tested on 64-bit Mac OS 10.9 Mavericks.
```
2. Before you run the program, please make sure that you are running two AVDs (avd
    andavd1). You can usepython run_avd.py 2to start them.
3. Make sure that you have installed your SimpleMessenger application on both AVDs!
4. Run the testing program from the command line.
5. It may issue some warnings or errors during execution. Some of them are normal,
    some may indicate errors in your program. Examine them to find out!
6. At completion, it will give you one of three outputs:
    - No communication verified: The SimpleMessenger instances cannot communi-
       cate with each other. This would be worth 0 points.
    - One-way communication verified: The SimpleMessenger onavd0can send a mes-
       sage to the SimpleMessenger onavd1. This is worth 2 points.
    - Two-way communication verified: Both AVDs can communicate with each other.
       This is worth an additional 3 points, for a total of 5.

**Remember** that this is only a simple test against the grading criteria. It isnot a test for
development progress! You will want to create your own tests as you go along.

**Notes for 64-bit Linux:** The testing program is compiled 32-bit. If you get an error like the
following, install the 32-bitlibzfor your system:

./simplemessenger-grading.linux: error while loading shared libraries:
libz.so.1: cannot open shared object file: No such file or directory

OnDebian-baseddistributions, youcanaccomplishthiswiththecommandapt-get install
libz1g:i386as root (you may need to usesudoorsu). Ifapt-getreports an error about the
architecture or says the package is not found, you may need to enable multiarch. To do this,
rundpkg --add-architecture i386as root, then update your APT repositories withapt-get
updateas root. Once this is done, you should be able to install the 32-bitlibz.
For other distributions you will need to consult your distribution documentation.


## 3.4 Submission

We use UB CSE autograder for submission. You can find autograder athttps://autograder.
cse.buffalo.edu/, and log in with your UBITName and password.
Once again, _it is critical that you follow everything below exactly._ Failure to do so **will lead
to no credit for this assignment**.
ZipupyourentireAndroidStudioprojectsourcetreeinasinglezipfilenamedSimpleMessenger.
zip. Ensure that _all_ of the following are true:

1. You _did not_ create your zip file from _inside_ theSimpleMessengerdirectory.
2. The top-level directory in your zip file is _SimpleMessenger_ , and it containsbuild.gradle
and all of your sources.
3. You used a zip utility and _not any other compression or archive tool:_ this means no 7-Zip,
no RAR, no tar, _etc._

## 3.5 Deadline

This project is due 2018-02-05 11:59:00 AM. This is one hour before our class. This is a firm
deadline. If the timestamp on your submission is 11:59:01, it is a late submission. You are
expected to attend class on this day!

## 3.6 Grading

This assignment is 5% of your final grade. Credit for this assignment will be apportioned as
follows:

- 2%: Your messenger app can send one-way messages fromavd0toavd1.
- 3%: Your messenger app can send two-way messages betweenavd0andavd1.

Thus, an application that can send two-way messages will receive a total of 5%: 2% for
sending fromavd0toavd1, and 3% for also being able to send messages back.

# 4 Other Resources

In addition toAndroid Developersand thecourse Android documentation(which will be
updated with FAQs and fixes, so keep an eye on it!), I have prepared a few videos to help with
getting the development environment set up and preparing the handout code using GitHub
Classroom. You can find themon YouTube.


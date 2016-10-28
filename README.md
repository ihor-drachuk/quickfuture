Quick Future
============
[![Build Status](https://travis-ci.org/benlau/quickfuture.svg?branch=master)](https://travis-ci.org/benlau/quickfuture)


QuickFuture is a QML wrapper of QFuture. It allows user to access and listen from a QFuture object generated by a QObject class. So that QML could respond to the result of a threaded calculation.


**Example**

```
import Future 1.0

...

var future = FileActor.read(“tmp.txt”);
// FileActor is a C++ class registered as context property
// QFuture<QString> FileActor::read(const QString&file);
// It is not a part of the library

Future.onFinished(future, function(value) {
  // do something when it is finished.
});

Future.promise(future).then(function(value) {
  // Future.promise creates a promise object by using Quick Promise.
  // It is useful when you have multiple asynchronuous tasks pending.
});

...

```

Installation
============

    qpm install quick.future.pri


Custom Type Registration
========================

By default, QFuture<T> is not a standard type recognized by QML.
It must be registered as a QMetaType per template type in order to get rid the error message.

The same rule applies in Quick Future too.
Common types are pre-registered already.
For your own custom type, you can register it by:

```
#include <QFFuture>
Q_DECLARE_METATYPE(QFuture<CustomType>)

...

int main(int argc, char *argv[])
{

...
   QFFuture::registerType<CustomType>();
...

}
```

Pre-registered data type list: bool, int, qreal, QString, QByteArray, QVariantMap, void.




API
===

(More API will be added upon request)

**Future.isFinished(future)**

Returns true if the asynchronous computation represented by this future has finished; otherwise returns false.

**Future.onFinished(future, callback)**

The callback will be invoked when the watched future finishes.

**Future.promise(future)**

Create a promise object which will be resolved when the future has finished. It must have QuickPromise installed and setup properly before using this function.

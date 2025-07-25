---
created: 2024-12-14T18:23:15 (UTC -05:00)
tags: []
source: https://docs.python.org/3/library/logging.html
 
---

# logging — Logging facility for Python — Python 3.13.1 documentation

**Source code:** [Lib/logging/\_\_init\_\_.py](https://github.com/python/cpython/tree/3.13/Lib/logging/__init__.py)

___

This module defines functions and classes which implement a flexible event logging system for applications and libraries.

The key benefit of having the logging API provided by a standard library module is that all Python modules can participate in logging, so your application log can include your own messages integrated with messages from third-party modules.

Here’s a simple example of idiomatic usage:

```
# myapp.py
import logging
import mylib
logger = logging.getLogger(__name__)

def main():
    logging.basicConfig(filename='myapp.log', level=logging.INFO)
    logger.info('Started')
    mylib.do_something()
    logger.info('Finished')

if __name__ == '__main__':
    main()

```

```
# mylib.py
import logging
logger = logging.getLogger(__name__)

def do_something():
    logger.info('Doing something')

```

If you run _myapp.py_, you should see this in _myapp.log_:

```
INFO:__main__:Started
INFO:mylib:Doing something
INFO:__main__:Finished

```

The key feature of this idiomatic usage is that the majority of code is simply creating a module level logger with `getLogger(__name__)`, and using that logger to do any needed logging. This is concise, while allowing downstream code fine-grained control if needed. Logged messages to the module-level logger get forwarded to handlers of loggers in higher-level modules, all the way up to the highest-level logger known as the root logger; this approach is known as hierarchical logging.

For logging to be useful, it needs to be configured: setting the levels and destinations for each logger, potentially changing how specific modules log, often based on command-line arguments or application configuration. In most cases, like the one above, only the root logger needs to be so configured, since all the lower level loggers at module level eventually forward their messages to its handlers. [`basicConfig()`](https://docs.python.org/3/library/logging.html#logging.basicConfig "logging.basicConfig") provides a quick way to configure the root logger that handles many use cases.

The module provides a lot of functionality and flexibility. If you are unfamiliar with logging, the best way to get to grips with it is to view the tutorials (**see the links above and on the right**).

The basic classes defined by the module, together with their attributes and methods, are listed in the sections below.

-   Loggers expose the interface that application code directly uses.
    
-   Handlers send the log records (created by loggers) to the appropriate destination.
    
-   Filters provide a finer grained facility for determining which log records to output.
    
-   Formatters specify the layout of log records in the final output.
    

## Logger Objects[¶](https://docs.python.org/3/library/logging.html#logger-objects "Link to this heading")

Loggers have the following attributes and methods. Note that Loggers should _NEVER_ be instantiated directly, but always through the module-level function `logging.getLogger(name)`. Multiple calls to [`getLogger()`](https://docs.python.org/3/library/logging.html#logging.getLogger "logging.getLogger") with the same name will always return a reference to the same Logger object.

The `name` is potentially a period-separated hierarchical value, like `foo.bar.baz` (though it could also be just plain `foo`, for example). Loggers that are further down in the hierarchical list are children of loggers higher up in the list. For example, given a logger with a name of `foo`, loggers with names of `foo.bar`, `foo.bar.baz`, and `foo.bam` are all descendants of `foo`. In addition, all loggers are descendants of the root logger. The logger name hierarchy is analogous to the Python package hierarchy, and identical to it if you organise your loggers on a per-module basis using the recommended construction `logging.getLogger(__name__)`. That’s because in a module, `__name__` is the module’s name in the Python package namespace.

_class_ logging.Logger[¶](https://docs.python.org/3/library/logging.html#logging.Logger "Link to this definition")

name[¶](https://docs.python.org/3/library/logging.html#logging.Logger.name "Link to this definition")

This is the logger’s name, and is the value that was passed to [`getLogger()`](https://docs.python.org/3/library/logging.html#logging.getLogger "logging.getLogger") to obtain the logger.

Note

This attribute should be treated as read-only.

level[¶](https://docs.python.org/3/library/logging.html#logging.Logger.level "Link to this definition")

The threshold of this logger, as set by the [`setLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "logging.Logger.setLevel") method.

Note

Do not set this attribute directly - always use [`setLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "logging.Logger.setLevel"), which has checks for the level passed to it.

parent[¶](https://docs.python.org/3/library/logging.html#logging.Logger.parent "Link to this definition")

The parent logger of this logger. It may change based on later instantiation of loggers which are higher up in the namespace hierarchy.

Note

This value should be treated as read-only.

propagate[¶](https://docs.python.org/3/library/logging.html#logging.Logger.propagate "Link to this definition")

If this attribute evaluates to true, events logged to this logger will be passed to the handlers of higher level (ancestor) loggers, in addition to any handlers attached to this logger. Messages are passed directly to the ancestor loggers’ handlers - neither the level nor filters of the ancestor loggers in question are considered.

If this evaluates to false, logging messages are not passed to the handlers of ancestor loggers.

Spelling it out with an example: If the propagate attribute of the logger named `A.B.C` evaluates to true, any event logged to `A.B.C` via a method call such as `logging.getLogger('A.B.C').error(...)` will \[subject to passing that logger’s level and filter settings\] be passed in turn to any handlers attached to loggers named `A.B`, `A` and the root logger, after first being passed to any handlers attached to `A.B.C`. If any logger in the chain `A.B.C`, `A.B`, `A` has its `propagate` attribute set to false, then that is the last logger whose handlers are offered the event to handle, and propagation stops at that point.

The constructor sets this attribute to `True`.

Note

If you attach a handler to a logger _and_ one or more of its ancestors, it may emit the same record multiple times. In general, you should not need to attach a handler to more than one logger - if you just attach it to the appropriate logger which is highest in the logger hierarchy, then it will see all events logged by all descendant loggers, provided that their propagate setting is left set to `True`. A common scenario is to attach handlers only to the root logger, and to let propagation take care of the rest.

handlers[¶](https://docs.python.org/3/library/logging.html#logging.Logger.handlers "Link to this definition")

The list of handlers directly attached to this logger instance.

Note

This attribute should be treated as read-only; it is normally changed via the [`addHandler()`](https://docs.python.org/3/library/logging.html#logging.Logger.addHandler "logging.Logger.addHandler") and [`removeHandler()`](https://docs.python.org/3/library/logging.html#logging.Logger.removeHandler "logging.Logger.removeHandler") methods, which use locks to ensure thread-safe operation.

disabled[¶](https://docs.python.org/3/library/logging.html#logging.Logger.disabled "Link to this definition")

This attribute disables handling of any events. It is set to `False` in the initializer, and only changed by logging configuration code.

Note

This attribute should be treated as read-only.

setLevel(_level_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "Link to this definition")

Sets the threshold for this logger to _level_. Logging messages which are less severe than _level_ will be ignored; logging messages which have severity _level_ or higher will be emitted by whichever handler or handlers service this logger, unless a handler’s level has been set to a higher severity level than _level_.

When a logger is created, the level is set to [`NOTSET`](https://docs.python.org/3/library/logging.html#logging.NOTSET "logging.NOTSET") (which causes all messages to be processed when the logger is the root logger, or delegation to the parent when the logger is a non-root logger). Note that the root logger is created with level [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING").

The term ‘delegation to the parent’ means that if a logger has a level of NOTSET, its chain of ancestor loggers is traversed until either an ancestor with a level other than NOTSET is found, or the root is reached.

If an ancestor is found with a level other than NOTSET, then that ancestor’s level is treated as the effective level of the logger where the ancestor search began, and is used to determine how a logging event is handled.

If the root is reached, and it has a level of NOTSET, then all messages will be processed. Otherwise, the root’s level will be used as the effective level.

See [Logging Levels](https://docs.python.org/3/library/logging.html#levels) for a list of levels.

Changed in version 3.2: The _level_ parameter now accepts a string representation of the level such as ‘INFO’ as an alternative to the integer constants such as [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO"). Note, however, that levels are internally stored as integers, and methods such as e.g. [`getEffectiveLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.getEffectiveLevel "logging.Logger.getEffectiveLevel") and [`isEnabledFor()`](https://docs.python.org/3/library/logging.html#logging.Logger.isEnabledFor "logging.Logger.isEnabledFor") will return/expect to be passed integers.

isEnabledFor(_level_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.isEnabledFor "Link to this definition")

Indicates if a message of severity _level_ would be processed by this logger. This method checks first the module-level level set by `logging.disable(level)` and then the logger’s effective level as determined by [`getEffectiveLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.getEffectiveLevel "logging.Logger.getEffectiveLevel").

getEffectiveLevel()[¶](https://docs.python.org/3/library/logging.html#logging.Logger.getEffectiveLevel "Link to this definition")

Indicates the effective level for this logger. If a value other than [`NOTSET`](https://docs.python.org/3/library/logging.html#logging.NOTSET "logging.NOTSET") has been set using [`setLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "logging.Logger.setLevel"), it is returned. Otherwise, the hierarchy is traversed towards the root until a value other than [`NOTSET`](https://docs.python.org/3/library/logging.html#logging.NOTSET "logging.NOTSET") is found, and that value is returned. The value returned is an integer, typically one of [`logging.DEBUG`](https://docs.python.org/3/library/logging.html#logging.DEBUG "logging.DEBUG"), [`logging.INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO") etc.

getChild(_suffix_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.getChild "Link to this definition")

Returns a logger which is a descendant to this logger, as determined by the suffix. Thus, `logging.getLogger('abc').getChild('def.ghi')` would return the same logger as would be returned by `logging.getLogger('abc.def.ghi')`. This is a convenience method, useful when the parent logger is named using e.g. `__name__` rather than a literal string.

Added in version 3.2.

getChildren()[¶](https://docs.python.org/3/library/logging.html#logging.Logger.getChildren "Link to this definition")

Returns a set of loggers which are immediate children of this logger. So for example `logging.getLogger().getChildren()` might return a set containing loggers named `foo` and `bar`, but a logger named `foo.bar` wouldn’t be included in the set. Likewise, `logging.getLogger('foo').getChildren()` might return a set including a logger named `foo.bar`, but it wouldn’t include one named `foo.bar.baz`.

Added in version 3.12.

debug(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.debug "Link to this definition")

Logs a message with level [`DEBUG`](https://docs.python.org/3/library/logging.html#logging.DEBUG "logging.DEBUG") on this logger. The _msg_ is the message format string, and the _args_ are the arguments which are merged into _msg_ using the string formatting operator. (Note that this means that you can use keywords in the format string, together with a single dictionary argument.) No % formatting operation is performed on _msg_ when no _args_ are supplied.

There are four keyword arguments in _kwargs_ which are inspected: _exc\_info_, _stack\_info_, _stacklevel_ and _extra_.

If _exc\_info_ does not evaluate as false, it causes exception information to be added to the logging message. If an exception tuple (in the format returned by [`sys.exc_info()`](https://docs.python.org/3/library/sys.html#sys.exc_info "sys.exc_info")) or an exception instance is provided, it is used; otherwise, [`sys.exc_info()`](https://docs.python.org/3/library/sys.html#sys.exc_info "sys.exc_info") is called to get the exception information.

The second optional keyword argument is _stack\_info_, which defaults to `False`. If true, stack information is added to the logging message, including the actual logging call. Note that this is not the same stack information as that displayed through specifying _exc\_info_: The former is stack frames from the bottom of the stack up to the logging call in the current thread, whereas the latter is information about stack frames which have been unwound, following an exception, while searching for exception handlers.

You can specify _stack\_info_ independently of _exc\_info_, e.g. to just show how you got to a certain point in your code, even when no exceptions were raised. The stack frames are printed following a header line which says:

```
Stack (most recent call last):

```

This mimics the `Traceback (most recent call last):` which is used when displaying exception frames.

The third optional keyword argument is _stacklevel_, which defaults to `1`. If greater than 1, the corresponding number of stack frames are skipped when computing the line number and function name set in the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") created for the logging event. This can be used in logging helpers so that the function name, filename and line number recorded are not the information for the helper function/method, but rather its caller. The name of this parameter mirrors the equivalent one in the [`warnings`](https://docs.python.org/3/library/warnings.html#module-warnings "warnings: Issue warning messages and control their disposition.") module.

The fourth keyword argument is _extra_ which can be used to pass a dictionary which is used to populate the [`__dict__`](https://docs.python.org/3/reference/datamodel.html#object.__dict__ "object.__dict__") of the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") created for the logging event with user-defined attributes. These custom attributes can then be used as you like. For example, they could be incorporated into logged messages. For example:

```
FORMAT = '%(asctime)s %(clientip)-15s %(user)-8s %(message)s'
logging.basicConfig(format=FORMAT)
d = {'clientip': '192.168.0.1', 'user': 'fbloggs'}
logger = logging.getLogger('tcpserver')
logger.warning('Protocol problem: %s', 'connection reset', extra=d)

```

would print something like

```
2006-02-08 22:20:02,165 192.168.0.1 fbloggs  Protocol problem: connection reset

```

The keys in the dictionary passed in _extra_ should not clash with the keys used by the logging system. (See the section on [LogRecord attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes) for more information on which keys are used by the logging system.)

If you choose to use these attributes in logged messages, you need to exercise some care. In the above example, for instance, the [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") has been set up with a format string which expects ‘clientip’ and ‘user’ in the attribute dictionary of the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord"). If these are missing, the message will not be logged because a string formatting exception will occur. So in this case, you always need to pass the _extra_ dictionary with these keys.

While this might be annoying, this feature is intended for use in specialized circumstances, such as multi-threaded servers where the same code executes in many contexts, and interesting conditions which arise are dependent on this context (such as remote client IP address and authenticated user name, in the above example). In such circumstances, it is likely that specialized [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter")s would be used with particular [`Handler`](https://docs.python.org/3/library/logging.html#logging.Handler "logging.Handler")s.

If no handler is attached to this logger (or any of its ancestors, taking into account the relevant [`Logger.propagate`](https://docs.python.org/3/library/logging.html#logging.Logger.propagate "logging.Logger.propagate") attributes), the message will be sent to the handler set on [`lastResort`](https://docs.python.org/3/library/logging.html#logging.lastResort "logging.lastResort").

Changed in version 3.2: The _stack\_info_ parameter was added.

Changed in version 3.5: The _exc\_info_ parameter can now accept exception instances.

Changed in version 3.8: The _stacklevel_ parameter was added.

info(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.info "Link to this definition")

Logs a message with level [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO") on this logger. The arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

warning(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.warning "Link to this definition")

Logs a message with level [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING") on this logger. The arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

Note

There is an obsolete method `warn` which is functionally identical to `warning`. As `warn` is deprecated, please do not use it - use `warning` instead.

error(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.error "Link to this definition")

Logs a message with level [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR") on this logger. The arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

critical(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.critical "Link to this definition")

Logs a message with level [`CRITICAL`](https://docs.python.org/3/library/logging.html#logging.CRITICAL "logging.CRITICAL") on this logger. The arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

log(_level_, _msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.log "Link to this definition")

Logs a message with integer level _level_ on this logger. The other arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

exception(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.exception "Link to this definition")

Logs a message with level [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR") on this logger. The arguments are interpreted as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"). Exception info is added to the logging message. This method should only be called from an exception handler.

addFilter(_filter_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.addFilter "Link to this definition")

Adds the specified filter _filter_ to this logger.

removeFilter(_filter_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.removeFilter "Link to this definition")

Removes the specified filter _filter_ from this logger.

filter(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.filter "Link to this definition")

Apply this logger’s filters to the record and return `True` if the record is to be processed. The filters are consulted in turn, until one of them returns a false value. If none of them return a false value, the record will be processed (passed to handlers). If one returns a false value, no further processing of the record occurs.

addHandler(_hdlr_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.addHandler "Link to this definition")

Adds the specified handler _hdlr_ to this logger.

removeHandler(_hdlr_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.removeHandler "Link to this definition")

Removes the specified handler _hdlr_ from this logger.

findCaller(_stack\_info\=False_, _stacklevel\=1_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.findCaller "Link to this definition")

Finds the caller’s source filename and line number. Returns the filename, line number, function name and stack information as a 4-element tuple. The stack information is returned as `None` unless _stack\_info_ is `True`.

The _stacklevel_ parameter is passed from code calling the [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug") and other APIs. If greater than 1, the excess is used to skip stack frames before determining the values to be returned. This will generally be useful when calling logging APIs from helper/wrapper code, so that the information in the event log refers not to the helper/wrapper code, but to the code that calls it.

handle(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.handle "Link to this definition")

Handles a record by passing it to all handlers associated with this logger and its ancestors (until a false value of _propagate_ is found). This method is used for unpickled records received from a socket, as well as those created locally. Logger-level filtering is applied using [`filter()`](https://docs.python.org/3/library/logging.html#logging.Logger.filter "logging.Logger.filter").

makeRecord(_name_, _level_, _fn_, _lno_, _msg_, _args_, _exc\_info_, _func\=None_, _extra\=None_, _sinfo\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.Logger.makeRecord "Link to this definition")

This is a factory method which can be overridden in subclasses to create specialized [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instances.

hasHandlers()[¶](https://docs.python.org/3/library/logging.html#logging.Logger.hasHandlers "Link to this definition")

Checks to see if this logger has any handlers configured. This is done by looking for handlers in this logger and its parents in the logger hierarchy. Returns `True` if a handler was found, else `False`. The method stops searching up the hierarchy whenever a logger with the ‘propagate’ attribute set to false is found - that will be the last logger which is checked for the existence of handlers.

Added in version 3.2.

Changed in version 3.7: Loggers can now be pickled and unpickled.

## Logging Levels[¶](https://docs.python.org/3/library/logging.html#logging-levels "Link to this heading")

The numeric values of logging levels are given in the following table. These are primarily of interest if you want to define your own levels, and need them to have specific values relative to the predefined levels. If you define a level with the same numeric value, it overwrites the predefined value; the predefined name is lost.

| 
Level

 | 

Numeric value

 | 

What it means / When to use it

 |
| --- | --- | --- |
| 

logging.NOTSET[¶](https://docs.python.org/3/library/logging.html#logging.NOTSET "Link to this definition")



 | 

0

 | 

When set on a logger, indicates that ancestor loggers are to be consulted to determine the effective level. If that still resolves to `NOTSET`, then all events are logged. When set on a handler, all events are handled.

 |
| 

logging.DEBUG[¶](https://docs.python.org/3/library/logging.html#logging.DEBUG "Link to this definition")



 | 

10

 | 

Detailed information, typically only of interest to a developer trying to diagnose a problem.

 |
| 

logging.INFO[¶](https://docs.python.org/3/library/logging.html#logging.INFO "Link to this definition")



 | 

20

 | 

Confirmation that things are working as expected.

 |
| 

logging.WARNING[¶](https://docs.python.org/3/library/logging.html#logging.WARNING "Link to this definition")



 | 

30

 | 

An indication that something unexpected happened, or that a problem might occur in the near future (e.g. ‘disk space low’). The software is still working as expected.

 |
| 

logging.ERROR[¶](https://docs.python.org/3/library/logging.html#logging.ERROR "Link to this definition")



 | 

40

 | 

Due to a more serious problem, the software has not been able to perform some function.

 |
| 

logging.CRITICAL[¶](https://docs.python.org/3/library/logging.html#logging.CRITICAL "Link to this definition")



 | 

50

 | 

A serious error, indicating that the program itself may be unable to continue running.

 |

## Handler Objects[¶](https://docs.python.org/3/library/logging.html#handler-objects "Link to this heading")

Handlers have the following attributes and methods. Note that [`Handler`](https://docs.python.org/3/library/logging.html#logging.Handler "logging.Handler") is never instantiated directly; this class acts as a base for more useful subclasses. However, the `__init__()` method in subclasses needs to call [`Handler.__init__()`](https://docs.python.org/3/library/logging.html#logging.Handler.__init__ "logging.Handler.__init__").

_class_ logging.Handler[¶](https://docs.python.org/3/library/logging.html#logging.Handler "Link to this definition")

\_\_init\_\_(_level\=NOTSET_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.__init__ "Link to this definition")

Initializes the [`Handler`](https://docs.python.org/3/library/logging.html#logging.Handler "logging.Handler") instance by setting its level, setting the list of filters to the empty list and creating a lock (using [`createLock()`](https://docs.python.org/3/library/logging.html#logging.Handler.createLock "logging.Handler.createLock")) for serializing access to an I/O mechanism.

createLock()[¶](https://docs.python.org/3/library/logging.html#logging.Handler.createLock "Link to this definition")

Initializes a thread lock which can be used to serialize access to underlying I/O functionality which may not be threadsafe.

acquire()[¶](https://docs.python.org/3/library/logging.html#logging.Handler.acquire "Link to this definition")

Acquires the thread lock created with [`createLock()`](https://docs.python.org/3/library/logging.html#logging.Handler.createLock "logging.Handler.createLock").

release()[¶](https://docs.python.org/3/library/logging.html#logging.Handler.release "Link to this definition")

Releases the thread lock acquired with [`acquire()`](https://docs.python.org/3/library/logging.html#logging.Handler.acquire "logging.Handler.acquire").

setLevel(_level_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.setLevel "Link to this definition")

Sets the threshold for this handler to _level_. Logging messages which are less severe than _level_ will be ignored. When a handler is created, the level is set to [`NOTSET`](https://docs.python.org/3/library/logging.html#logging.NOTSET "logging.NOTSET") (which causes all messages to be processed).

See [Logging Levels](https://docs.python.org/3/library/logging.html#levels) for a list of levels.

Changed in version 3.2: The _level_ parameter now accepts a string representation of the level such as ‘INFO’ as an alternative to the integer constants such as [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO").

setFormatter(_fmt_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.setFormatter "Link to this definition")

Sets the [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") for this handler to _fmt_.

addFilter(_filter_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.addFilter "Link to this definition")

Adds the specified filter _filter_ to this handler.

removeFilter(_filter_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.removeFilter "Link to this definition")

Removes the specified filter _filter_ from this handler.

filter(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.filter "Link to this definition")

Apply this handler’s filters to the record and return `True` if the record is to be processed. The filters are consulted in turn, until one of them returns a false value. If none of them return a false value, the record will be emitted. If one returns a false value, the handler will not emit the record.

flush()[¶](https://docs.python.org/3/library/logging.html#logging.Handler.flush "Link to this definition")

Ensure all logging output has been flushed. This version does nothing and is intended to be implemented by subclasses.

close()[¶](https://docs.python.org/3/library/logging.html#logging.Handler.close "Link to this definition")

Tidy up any resources used by the handler. This version does no output but removes the handler from an internal list of handlers which is closed when [`shutdown()`](https://docs.python.org/3/library/logging.html#logging.shutdown "logging.shutdown") is called. Subclasses should ensure that this gets called from overridden [`close()`](https://docs.python.org/3/library/logging.html#logging.Handler.close "logging.Handler.close") methods.

handle(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.handle "Link to this definition")

Conditionally emits the specified logging record, depending on filters which may have been added to the handler. Wraps the actual emission of the record with acquisition/release of the I/O thread lock.

handleError(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.handleError "Link to this definition")

This method should be called from handlers when an exception is encountered during an [`emit()`](https://docs.python.org/3/library/logging.html#logging.Handler.emit "logging.Handler.emit") call. If the module-level attribute [`raiseExceptions`](https://docs.python.org/3/library/logging.html#logging.raiseExceptions "logging.raiseExceptions") is `False`, exceptions get silently ignored. This is what is mostly wanted for a logging system - most users will not care about errors in the logging system, they are more interested in application errors. You could, however, replace this with a custom handler if you wish. The specified record is the one which was being processed when the exception occurred. (The default value of [`raiseExceptions`](https://docs.python.org/3/library/logging.html#logging.raiseExceptions "logging.raiseExceptions") is `True`, as that is more useful during development).

format(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.format "Link to this definition")

Do formatting for a record - if a formatter is set, use it. Otherwise, use the default formatter for the module.

emit(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Handler.emit "Link to this definition")

Do whatever it takes to actually log the specified logging record. This version is intended to be implemented by subclasses and so raises a [`NotImplementedError`](https://docs.python.org/3/library/exceptions.html#NotImplementedError "NotImplementedError").

Warning

This method is called after a handler-level lock is acquired, which is released after this method returns. When you override this method, note that you should be careful when calling anything that invokes other parts of the logging API which might do locking, because that might result in a deadlock. Specifically:

-   Logging configuration APIs acquire the module-level lock, and then individual handler-level locks as those handlers are configured.
    
-   Many logging APIs lock the module-level lock. If such an API is called from this method, it could cause a deadlock if a configuration call is made on another thread, because that thread will try to acquire the module-level lock _before_ the handler-level lock, whereas this thread tries to acquire the module-level lock _after_ the handler-level lock (because in this method, the handler-level lock has already been acquired).
    

For a list of handlers included as standard, see [`logging.handlers`](https://docs.python.org/3/library/logging.handlers.html#module-logging.handlers "logging.handlers: Handlers for the logging module.").

## Formatter Objects[¶](https://docs.python.org/3/library/logging.html#formatter-objects "Link to this heading")

_class_ logging.Formatter(_fmt\=None_, _datefmt\=None_, _style\='%'_, _validate\=True_, _\*_, _defaults\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.Formatter "Link to this definition")

Responsible for converting a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") to an output string to be interpreted by a human or external system.

Parameters:

-   **fmt** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str")) – A format string in the given _style_ for the logged output as a whole. The possible mapping keys are drawn from the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") object’s [LogRecord attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes). If not specified, `'%(message)s'` is used, which is just the logged message.
    
-   **datefmt** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str")) – A format string in the given _style_ for the date/time portion of the logged output. If not specified, the default described in [`formatTime()`](https://docs.python.org/3/library/logging.html#logging.Formatter.formatTime "logging.Formatter.formatTime") is used.
    
-   **style** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str")) – Can be one of `'%'`, `'{'` or `'$'` and determines how the format string will be merged with its data: using one of [printf-style String Formatting](https://docs.python.org/3/library/stdtypes.html#old-string-formatting) (`%`), [`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format "str.format") (`{`) or [`string.Template`](https://docs.python.org/3/library/string.html#string.Template "string.Template") (`$`). This only applies to _fmt_ and _datefmt_ (e.g. `'%(message)s'` versus `'{message}'`), not to the actual log messages passed to the logging methods. However, there are [other ways](https://docs.python.org/3/howto/logging-cookbook.html#formatting-styles) to use `{`\- and `$`\-formatting for log messages.
    
-   **validate** ([_bool_](https://docs.python.org/3/library/functions.html#bool "bool")) – If `True` (the default), incorrect or mismatched _fmt_ and _style_ will raise a [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError "ValueError"); for example, `logging.Formatter('%(asctime)s - %(message)s', style='{')`.
    
-   **defaults** ([_dict_](https://docs.python.org/3/library/stdtypes.html#dict "dict")_\[_[_str_](https://docs.python.org/3/library/stdtypes.html#str "str")_,_ _Any__\]_) – A dictionary with default values to use in custom fields. For example, `logging.Formatter('%(ip)s %(message)s', defaults={"ip": None})`
    

Changed in version 3.2: Added the _style_ parameter.

Changed in version 3.8: Added the _validate_ parameter.

Changed in version 3.10: Added the _defaults_ parameter.

format(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Formatter.format "Link to this definition")

The record’s attribute dictionary is used as the operand to a string formatting operation. Returns the resulting string. Before formatting the dictionary, a couple of preparatory steps are carried out. The _message_ attribute of the record is computed using _msg_ % _args_. If the formatting string contains `'(asctime)'`, [`formatTime()`](https://docs.python.org/3/library/logging.html#logging.Formatter.formatTime "logging.Formatter.formatTime") is called to format the event time. If there is exception information, it is formatted using [`formatException()`](https://docs.python.org/3/library/logging.html#logging.Formatter.formatException "logging.Formatter.formatException") and appended to the message. Note that the formatted exception information is cached in attribute _exc\_text_. This is useful because the exception information can be pickled and sent across the wire, but you should be careful if you have more than one [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") subclass which customizes the formatting of exception information. In this case, you will have to clear the cached value (by setting the _exc\_text_ attribute to `None`) after a formatter has done its formatting, so that the next formatter to handle the event doesn’t use the cached value, but recalculates it afresh.

If stack information is available, it’s appended after the exception information, using [`formatStack()`](https://docs.python.org/3/library/logging.html#logging.Formatter.formatStack "logging.Formatter.formatStack") to transform it if necessary.

formatTime(_record_, _datefmt\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.Formatter.formatTime "Link to this definition")

This method should be called from [`format()`](https://docs.python.org/3/library/functions.html#format "format") by a formatter which wants to make use of a formatted time. This method can be overridden in formatters to provide for any specific requirement, but the basic behavior is as follows: if _datefmt_ (a string) is specified, it is used with [`time.strftime()`](https://docs.python.org/3/library/time.html#time.strftime "time.strftime") to format the creation time of the record. Otherwise, the format ‘%Y-%m-%d %H:%M:%S,uuu’ is used, where the uuu part is a millisecond value and the other letters are as per the [`time.strftime()`](https://docs.python.org/3/library/time.html#time.strftime "time.strftime") documentation. An example time in this format is `2003-01-23 00:29:50,411`. The resulting string is returned.

This function uses a user-configurable function to convert the creation time to a tuple. By default, [`time.localtime()`](https://docs.python.org/3/library/time.html#time.localtime "time.localtime") is used; to change this for a particular formatter instance, set the `converter` attribute to a function with the same signature as [`time.localtime()`](https://docs.python.org/3/library/time.html#time.localtime "time.localtime") or [`time.gmtime()`](https://docs.python.org/3/library/time.html#time.gmtime "time.gmtime"). To change it for all formatters, for example if you want all logging times to be shown in GMT, set the `converter` attribute in the `Formatter` class.

Changed in version 3.3: Previously, the default format was hard-coded as in this example: `2010-09-06 22:38:15,292` where the part before the comma is handled by a strptime format string (`'%Y-%m-%d %H:%M:%S'`), and the part after the comma is a millisecond value. Because strptime does not have a format placeholder for milliseconds, the millisecond value is appended using another format string, `'%s,%03d'` — and both of these format strings have been hardcoded into this method. With the change, these strings are defined as class-level attributes which can be overridden at the instance level when desired. The names of the attributes are `default_time_format` (for the strptime format string) and `default_msec_format` (for appending the millisecond value).

Changed in version 3.9: The `default_msec_format` can be `None`.

formatException(_exc\_info_)[¶](https://docs.python.org/3/library/logging.html#logging.Formatter.formatException "Link to this definition")

Formats the specified exception information (a standard exception tuple as returned by [`sys.exc_info()`](https://docs.python.org/3/library/sys.html#sys.exc_info "sys.exc_info")) as a string. This default implementation just uses [`traceback.print_exception()`](https://docs.python.org/3/library/traceback.html#traceback.print_exception "traceback.print_exception"). The resulting string is returned.

formatStack(_stack\_info_)[¶](https://docs.python.org/3/library/logging.html#logging.Formatter.formatStack "Link to this definition")

Formats the specified stack information (a string as returned by [`traceback.print_stack()`](https://docs.python.org/3/library/traceback.html#traceback.print_stack "traceback.print_stack"), but with the last newline removed) as a string. This default implementation just returns the input value.

_class_ logging.BufferingFormatter(_linefmt\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.BufferingFormatter "Link to this definition")

A base formatter class suitable for subclassing when you want to format a number of records. You can pass a [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") instance which you want to use to format each line (that corresponds to a single record). If not specified, the default formatter (which just outputs the event message) is used as the line formatter.

Return a header for a list of _records_. The base implementation just returns the empty string. You will need to override this method if you want specific behaviour, e.g. to show the count of records, a title or a separator line.

Return a footer for a list of _records_. The base implementation just returns the empty string. You will need to override this method if you want specific behaviour, e.g. to show the count of records or a separator line.

format(_records_)[¶](https://docs.python.org/3/library/logging.html#logging.BufferingFormatter.format "Link to this definition")

Return formatted text for a list of _records_. The base implementation just returns the empty string if there are no records; otherwise, it returns the concatenation of the header, each record formatted with the line formatter, and the footer.

## Filter Objects[¶](https://docs.python.org/3/library/logging.html#filter-objects "Link to this heading")

`Filters` can be used by `Handlers` and `Loggers` for more sophisticated filtering than is provided by levels. The base filter class only allows events which are below a certain point in the logger hierarchy. For example, a filter initialized with ‘A.B’ will allow events logged by loggers ‘A.B’, ‘A.B.C’, ‘A.B.C.D’, ‘A.B.D’ etc. but not ‘A.BB’, ‘B.A.B’ etc. If initialized with the empty string, all events are passed.

_class_ logging.Filter(_name\=''_)[¶](https://docs.python.org/3/library/logging.html#logging.Filter "Link to this definition")

Returns an instance of the [`Filter`](https://docs.python.org/3/library/logging.html#logging.Filter "logging.Filter") class. If _name_ is specified, it names a logger which, together with its children, will have its events allowed through the filter. If _name_ is the empty string, allows every event.

filter(_record_)[¶](https://docs.python.org/3/library/logging.html#logging.Filter.filter "Link to this definition")

Is the specified record to be logged? Returns false for no, true for yes. Filters can either modify log records in-place or return a completely different record instance which will replace the original log record in any future processing of the event.

Note that filters attached to handlers are consulted before an event is emitted by the handler, whereas filters attached to loggers are consulted whenever an event is logged (using [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"), [`info()`](https://docs.python.org/3/library/logging.html#logging.info "logging.info"), etc.), before sending an event to handlers. This means that events which have been generated by descendant loggers will not be filtered by a logger’s filter setting, unless the filter has also been applied to those descendant loggers.

You don’t actually need to subclass `Filter`: you can pass any instance which has a `filter` method with the same semantics.

Changed in version 3.2: You don’t need to create specialized `Filter` classes, or use other classes with a `filter` method: you can use a function (or other callable) as a filter. The filtering logic will check to see if the filter object has a `filter` attribute: if it does, it’s assumed to be a `Filter` and its [`filter()`](https://docs.python.org/3/library/logging.html#logging.Filter.filter "logging.Filter.filter") method is called. Otherwise, it’s assumed to be a callable and called with the record as the single parameter. The returned value should conform to that returned by [`filter()`](https://docs.python.org/3/library/logging.html#logging.Filter.filter "logging.Filter.filter").

Changed in version 3.12: You can now return a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instance from filters to replace the log record rather than modifying it in place. This allows filters attached to a [`Handler`](https://docs.python.org/3/library/logging.html#logging.Handler "logging.Handler") to modify the log record before it is emitted, without having side effects on other handlers.

Although filters are used primarily to filter records based on more sophisticated criteria than levels, they get to see every record which is processed by the handler or logger they’re attached to: this can be useful if you want to do things like counting how many records were processed by a particular logger or handler, or adding, changing or removing attributes in the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") being processed. Obviously changing the LogRecord needs to be done with some care, but it does allow the injection of contextual information into logs (see [Using Filters to impart contextual information](https://docs.python.org/3/howto/logging-cookbook.html#filters-contextual)).

## LogRecord Objects[¶](https://docs.python.org/3/library/logging.html#logrecord-objects "Link to this heading")

[`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instances are created automatically by the [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger") every time something is logged, and can be created manually via [`makeLogRecord()`](https://docs.python.org/3/library/logging.html#logging.makeLogRecord "logging.makeLogRecord") (for example, from a pickled event received over the wire).

_class_ logging.LogRecord(_name_, _level_, _pathname_, _lineno_, _msg_, _args_, _exc\_info_, _func\=None_, _sinfo\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.LogRecord "Link to this definition")

Contains all the information pertinent to the event being logged.

The primary information is passed in _msg_ and _args_, which are combined using `msg % args` to create the `message` attribute of the record.

Parameters:

-   **name** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str")) – The name of the logger used to log the event represented by this `LogRecord`. Note that the logger name in the `LogRecord` will always have this value, even though it may be emitted by a handler attached to a different (ancestor) logger.
    
-   **level** ([_int_](https://docs.python.org/3/library/functions.html#int "int")) – The [numeric level](https://docs.python.org/3/library/logging.html#levels) of the logging event (such as `10` for `DEBUG`, `20` for `INFO`, etc). Note that this is converted to _two_ attributes of the LogRecord: `levelno` for the numeric value and `levelname` for the corresponding level name.
    
-   **pathname** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str")) – The full string path of the source file where the logging call was made.
    
-   **lineno** ([_int_](https://docs.python.org/3/library/functions.html#int "int")) – The line number in the source file where the logging call was made.
    
-   **msg** ([_Any_](https://docs.python.org/3/library/typing.html#typing.Any "typing.Any")) – The event description message, which can be a %-format string with placeholders for variable data, or an arbitrary object (see [Using arbitrary objects as messages](https://docs.python.org/3/howto/logging.html#arbitrary-object-messages)).
    
-   **args** ([_tuple_](https://docs.python.org/3/library/stdtypes.html#tuple "tuple") _|_ [_dict_](https://docs.python.org/3/library/stdtypes.html#dict "dict")_\[_[_str_](https://docs.python.org/3/library/stdtypes.html#str "str")_,_ [_Any_](https://docs.python.org/3/library/typing.html#typing.Any "typing.Any")_\]_) – Variable data to merge into the _msg_ argument to obtain the event description.
    
-   **exc\_info** ([_tuple_](https://docs.python.org/3/library/stdtypes.html#tuple "tuple")_\[_[_type_](https://docs.python.org/3/library/functions.html#type "type")_\[_[_BaseException_](https://docs.python.org/3/library/exceptions.html#BaseException "BaseException")_\]__,_ [_BaseException_](https://docs.python.org/3/library/exceptions.html#BaseException "BaseException")_,_ [_types.TracebackType_](https://docs.python.org/3/library/types.html#types.TracebackType "types.TracebackType")_\]_ _|_ _None_) – An exception tuple with the current exception information, as returned by [`sys.exc_info()`](https://docs.python.org/3/library/sys.html#sys.exc_info "sys.exc_info"), or `None` if no exception information is available.
    
-   **func** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str") _|_ _None_) – The name of the function or method from which the logging call was invoked.
    
-   **sinfo** ([_str_](https://docs.python.org/3/library/stdtypes.html#str "str") _|_ _None_) – A text string representing stack information from the base of the stack in the current thread, up to the logging call.
    

getMessage()[¶](https://docs.python.org/3/library/logging.html#logging.LogRecord.getMessage "Link to this definition")

Returns the message for this [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instance after merging any user-supplied arguments with the message. If the user-supplied message argument to the logging call is not a string, [`str()`](https://docs.python.org/3/library/stdtypes.html#str "str") is called on it to convert it to a string. This allows use of user-defined classes as messages, whose `__str__` method can return the actual format string to be used.

Changed in version 3.2: The creation of a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") has been made more configurable by providing a factory which is used to create the record. The factory can be set using [`getLogRecordFactory()`](https://docs.python.org/3/library/logging.html#logging.getLogRecordFactory "logging.getLogRecordFactory") and [`setLogRecordFactory()`](https://docs.python.org/3/library/logging.html#logging.setLogRecordFactory "logging.setLogRecordFactory") (see this for the factory’s signature).

This functionality can be used to inject your own values into a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") at creation time. You can use the following pattern:

```
old_factory = logging.getLogRecordFactory()

def record_factory(*args, **kwargs):
    record = old_factory(*args, **kwargs)
    record.custom_attribute = 0xdecafbad
    return record

logging.setLogRecordFactory(record_factory)

```

With this pattern, multiple factories could be chained, and as long as they don’t overwrite each other’s attributes or unintentionally overwrite the standard attributes listed above, there should be no surprises.

## LogRecord attributes[¶](https://docs.python.org/3/library/logging.html#logrecord-attributes "Link to this heading")

The LogRecord has a number of attributes, most of which are derived from the parameters to the constructor. (Note that the names do not always correspond exactly between the LogRecord constructor parameters and the LogRecord attributes.) These attributes can be used to merge data from the record into the format string. The following table lists (in alphabetical order) the attribute names, their meanings and the corresponding placeholder in a %-style format string.

If you are using {}-formatting ([`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format "str.format")), you can use `{attrname}` as the placeholder in the format string. If you are using $-formatting ([`string.Template`](https://docs.python.org/3/library/string.html#string.Template "string.Template")), use the form `${attrname}`. In both cases, of course, replace `attrname` with the actual attribute name you want to use.

In the case of {}-formatting, you can specify formatting flags by placing them after the attribute name, separated from it with a colon. For example: a placeholder of `{msecs:03.0f}` would format a millisecond value of `4` as `004`. Refer to the [`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format "str.format") documentation for full details on the options available to you.

| 
Attribute name

 | 

Format

 | 

Description

 |
| --- | --- | --- |
| 

args

 | 

You shouldn’t need to format this yourself.

 | 

The tuple of arguments merged into `msg` to produce `message`, or a dict whose values are used for the merge (when there is only one argument, and it is a dictionary).

 |
| 

asctime

 | 

`%(asctime)s`

 | 

Human-readable time when the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") was created. By default this is of the form ‘2003-07-08 16:49:45,896’ (the numbers after the comma are millisecond portion of the time).

 |
| 

created

 | 

`%(created)f`

 | 

Time when the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") was created (as returned by [`time.time_ns()`](https://docs.python.org/3/library/time.html#time.time_ns "time.time_ns") / 1e9).

 |
| 

exc\_info

 | 

You shouldn’t need to format this yourself.

 | 

Exception tuple (à la `sys.exc_info`) or, if no exception has occurred, `None`.

 |
| 

filename

 | 

`%(filename)s`

 | 

Filename portion of `pathname`.

 |
| 

funcName

 | 

`%(funcName)s`

 | 

Name of function containing the logging call.

 |
| 

levelname

 | 

`%(levelname)s`

 | 

Text logging level for the message (`'DEBUG'`, `'INFO'`, `'WARNING'`, `'ERROR'`, `'CRITICAL'`).

 |
| 

levelno

 | 

`%(levelno)s`

 | 

Numeric logging level for the message ([`DEBUG`](https://docs.python.org/3/library/logging.html#logging.DEBUG "logging.DEBUG"), [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO"), [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING"), [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR"), [`CRITICAL`](https://docs.python.org/3/library/logging.html#logging.CRITICAL "logging.CRITICAL")).

 |
| 

lineno

 | 

`%(lineno)d`

 | 

Source line number where the logging call was issued (if available).

 |
| 

message

 | 

`%(message)s`

 | 

The logged message, computed as `msg % args`. This is set when [`Formatter.format()`](https://docs.python.org/3/library/logging.html#logging.Formatter.format "logging.Formatter.format") is invoked.

 |
| 

module

 | 

`%(module)s`

 | 

Module (name portion of `filename`).

 |
| 

msecs

 | 

`%(msecs)d`

 | 

Millisecond portion of the time when the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") was created.

 |
| 

msg

 | 

You shouldn’t need to format this yourself.

 | 

The format string passed in the original logging call. Merged with `args` to produce `message`, or an arbitrary object (see [Using arbitrary objects as messages](https://docs.python.org/3/howto/logging.html#arbitrary-object-messages)).

 |
| 

name

 | 

`%(name)s`

 | 

Name of the logger used to log the call.

 |
| 

pathname

 | 

`%(pathname)s`

 | 

Full pathname of the source file where the logging call was issued (if available).

 |
| 

process

 | 

`%(process)d`

 | 

Process ID (if available).

 |
| 

processName

 | 

`%(processName)s`

 | 

Process name (if available).

 |
| 

relativeCreated

 | 

`%(relativeCreated)d`

 | 

Time in milliseconds when the LogRecord was created, relative to the time the logging module was loaded.

 |
| 

stack\_info

 | 

You shouldn’t need to format this yourself.

 | 

Stack frame information (where available) from the bottom of the stack in the current thread, up to and including the stack frame of the logging call which resulted in the creation of this record.

 |
| 

thread

 | 

`%(thread)d`

 | 

Thread ID (if available).

 |
| 

threadName

 | 

`%(threadName)s`

 | 

Thread name (if available).

 |
| 

taskName

 | 

`%(taskName)s`

 | 

[`asyncio.Task`](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task "asyncio.Task") name (if available).

 |

Changed in version 3.1: _processName_ was added.

Changed in version 3.12: _taskName_ was added.

## LoggerAdapter Objects[¶](https://docs.python.org/3/library/logging.html#loggeradapter-objects "Link to this heading")

[`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter") instances are used to conveniently pass contextual information into logging calls. For a usage example, see the section on [adding contextual information to your logging output](https://docs.python.org/3/howto/logging-cookbook.html#context-info).

_class_ logging.LoggerAdapter(_logger_, _extra_, _merge\_extra\=False_)[¶](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "Link to this definition")

Returns an instance of [`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter") initialized with an underlying [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger") instance, a dict-like object (_extra_), and a boolean (_merge\_extra_) indicating whether or not the _extra_ argument of individual log calls should be merged with the [`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter") extra. The default behavior is to ignore the _extra_ argument of individual log calls and only use the one of the [`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter") instance

process(_msg_, _kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter.process "Link to this definition")

Modifies the message and/or keyword arguments passed to a logging call in order to insert contextual information. This implementation takes the object passed as _extra_ to the constructor and adds it to _kwargs_ using key ‘extra’. The return value is a (_msg_, _kwargs_) tuple which has the (possibly modified) versions of the arguments passed in.

manager[¶](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter.manager "Link to this definition")

Delegates to the underlying `manager` on _logger_.

\_log[¶](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter._log "Link to this definition")

Delegates to the underlying `_log()` method on _logger_.

In addition to the above, [`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter") supports the following methods of [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger"): [`debug()`](https://docs.python.org/3/library/logging.html#logging.Logger.debug "logging.Logger.debug"), [`info()`](https://docs.python.org/3/library/logging.html#logging.Logger.info "logging.Logger.info"), [`warning()`](https://docs.python.org/3/library/logging.html#logging.Logger.warning "logging.Logger.warning"), [`error()`](https://docs.python.org/3/library/logging.html#logging.Logger.error "logging.Logger.error"), [`exception()`](https://docs.python.org/3/library/logging.html#logging.Logger.exception "logging.Logger.exception"), [`critical()`](https://docs.python.org/3/library/logging.html#logging.Logger.critical "logging.Logger.critical"), [`log()`](https://docs.python.org/3/library/logging.html#logging.Logger.log "logging.Logger.log"), [`isEnabledFor()`](https://docs.python.org/3/library/logging.html#logging.Logger.isEnabledFor "logging.Logger.isEnabledFor"), [`getEffectiveLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.getEffectiveLevel "logging.Logger.getEffectiveLevel"), [`setLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "logging.Logger.setLevel") and [`hasHandlers()`](https://docs.python.org/3/library/logging.html#logging.Logger.hasHandlers "logging.Logger.hasHandlers"). These methods have the same signatures as their counterparts in [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger"), so you can use the two types of instances interchangeably.

Changed in version 3.2: The [`isEnabledFor()`](https://docs.python.org/3/library/logging.html#logging.Logger.isEnabledFor "logging.Logger.isEnabledFor"), [`getEffectiveLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.getEffectiveLevel "logging.Logger.getEffectiveLevel"), [`setLevel()`](https://docs.python.org/3/library/logging.html#logging.Logger.setLevel "logging.Logger.setLevel") and [`hasHandlers()`](https://docs.python.org/3/library/logging.html#logging.Logger.hasHandlers "logging.Logger.hasHandlers") methods were added to [`LoggerAdapter`](https://docs.python.org/3/library/logging.html#logging.LoggerAdapter "logging.LoggerAdapter"). These methods delegate to the underlying logger.

Changed in version 3.6: Attribute `manager` and method `_log()` were added, which delegate to the underlying logger and allow adapters to be nested.

Changed in version 3.13: The _merge\_extra_ argument was added.

## Thread Safety[¶](https://docs.python.org/3/library/logging.html#thread-safety "Link to this heading")

The logging module is intended to be thread-safe without any special work needing to be done by its clients. It achieves this though using threading locks; there is one lock to serialize access to the module’s shared data, and each handler also creates a lock to serialize access to its underlying I/O.

If you are implementing asynchronous signal handlers using the [`signal`](https://docs.python.org/3/library/signal.html#module-signal "signal: Set handlers for asynchronous events.") module, you may not be able to use logging from within such handlers. This is because lock implementations in the [`threading`](https://docs.python.org/3/library/threading.html#module-threading "threading: Thread-based parallelism.") module are not always re-entrant, and so cannot be invoked from such signal handlers.

## Module-Level Functions[¶](https://docs.python.org/3/library/logging.html#module-level-functions "Link to this heading")

In addition to the classes described above, there are a number of module-level functions.

logging.getLogger(_name\=None_)[¶](https://docs.python.org/3/library/logging.html#logging.getLogger "Link to this definition")

Return a logger with the specified name or, if name is `None`, return the root logger of the hierarchy. If specified, the name is typically a dot-separated hierarchical name like _‘a’_, _‘a.b’_ or _‘a.b.c.d’_. Choice of these names is entirely up to the developer who is using logging, though it is recommended that `__name__` be used unless you have a specific reason for not doing that, as mentioned in [Logger Objects](https://docs.python.org/3/library/logging.html#logger).

All calls to this function with a given name return the same logger instance. This means that logger instances never need to be passed between different parts of an application.

logging.getLoggerClass()[¶](https://docs.python.org/3/library/logging.html#logging.getLoggerClass "Link to this definition")

Return either the standard [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger") class, or the last class passed to [`setLoggerClass()`](https://docs.python.org/3/library/logging.html#logging.setLoggerClass "logging.setLoggerClass"). This function may be called from within a new class definition, to ensure that installing a customized [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger") class will not undo customizations already applied by other code. For example:

```
class MyLogger(logging.getLoggerClass()):
    # ... override behaviour here

```

logging.getLogRecordFactory()[¶](https://docs.python.org/3/library/logging.html#logging.getLogRecordFactory "Link to this definition")

Return a callable which is used to create a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord").

Added in version 3.2: This function has been provided, along with [`setLogRecordFactory()`](https://docs.python.org/3/library/logging.html#logging.setLogRecordFactory "logging.setLogRecordFactory"), to allow developers more control over how the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") representing a logging event is constructed.

See [`setLogRecordFactory()`](https://docs.python.org/3/library/logging.html#logging.setLogRecordFactory "logging.setLogRecordFactory") for more information about the how the factory is called.

logging.debug(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.debug "Link to this definition")

This is a convenience function that calls [`Logger.debug()`](https://docs.python.org/3/library/logging.html#logging.Logger.debug "logging.Logger.debug"), on the root logger. The handling of the arguments is in every way identical to what is described in that method.

The only difference is that if the root logger has no handlers, then [`basicConfig()`](https://docs.python.org/3/library/logging.html#logging.basicConfig "logging.basicConfig") is called, prior to calling `debug` on the root logger.

For very short scripts or quick demonstrations of `logging` facilities, `debug` and the other module-level functions may be convenient. However, most programs will want to carefully and explicitly control the logging configuration, and should therefore prefer creating a module-level logger and calling [`Logger.debug()`](https://docs.python.org/3/library/logging.html#logging.Logger.debug "logging.Logger.debug") (or other level-specific methods) on it, as described at the beginnning of this documentation.

logging.info(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.info "Link to this definition")

Logs a message with level [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO") on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

logging.warning(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.warning "Link to this definition")

Logs a message with level [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING") on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

Note

There is an obsolete function `warn` which is functionally identical to `warning`. As `warn` is deprecated, please do not use it - use `warning` instead.

logging.error(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.error "Link to this definition")

Logs a message with level [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR") on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

logging.critical(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.critical "Link to this definition")

Logs a message with level [`CRITICAL`](https://docs.python.org/3/library/logging.html#logging.CRITICAL "logging.CRITICAL") on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

logging.exception(_msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.exception "Link to this definition")

Logs a message with level [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR") on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"). Exception info is added to the logging message. This function should only be called from an exception handler.

logging.log(_level_, _msg_, _\*args_, _\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.log "Link to this definition")

Logs a message with level _level_ on the root logger. The arguments and behavior are otherwise the same as for [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug").

logging.disable(_level\=CRITICAL_)[¶](https://docs.python.org/3/library/logging.html#logging.disable "Link to this definition")

Provides an overriding level _level_ for all loggers which takes precedence over the logger’s own level. When the need arises to temporarily throttle logging output down across the whole application, this function can be useful. Its effect is to disable all logging calls of severity _level_ and below, so that if you call it with a value of INFO, then all INFO and DEBUG events would be discarded, whereas those of severity WARNING and above would be processed according to the logger’s effective level. If `logging.disable(logging.NOTSET)` is called, it effectively removes this overriding level, so that logging output again depends on the effective levels of individual loggers.

Note that if you have defined any custom logging level higher than `CRITICAL` (this is not recommended), you won’t be able to rely on the default value for the _level_ parameter, but will have to explicitly supply a suitable value.

Changed in version 3.7: The _level_ parameter was defaulted to level `CRITICAL`. See [bpo-28524](https://bugs.python.org/issue?@action=redirect&bpo=28524) for more information about this change.

logging.addLevelName(_level_, _levelName_)[¶](https://docs.python.org/3/library/logging.html#logging.addLevelName "Link to this definition")

Associates level _level_ with text _levelName_ in an internal dictionary, which is used to map numeric levels to a textual representation, for example when a [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") formats a message. This function can also be used to define your own levels. The only constraints are that all levels used must be registered using this function, levels should be positive integers and they should increase in increasing order of severity.

Note

If you are thinking of defining your own levels, please see the section on [Custom Levels](https://docs.python.org/3/howto/logging.html#custom-levels).

logging.getLevelNamesMapping()[¶](https://docs.python.org/3/library/logging.html#logging.getLevelNamesMapping "Link to this definition")

Returns a mapping from level names to their corresponding logging levels. For example, the string “CRITICAL” maps to [`CRITICAL`](https://docs.python.org/3/library/logging.html#logging.CRITICAL "logging.CRITICAL"). The returned mapping is copied from an internal mapping on each call to this function.

Added in version 3.11.

logging.getLevelName(_level_)[¶](https://docs.python.org/3/library/logging.html#logging.getLevelName "Link to this definition")

Returns the textual or numeric representation of logging level _level_.

If _level_ is one of the predefined levels [`CRITICAL`](https://docs.python.org/3/library/logging.html#logging.CRITICAL "logging.CRITICAL"), [`ERROR`](https://docs.python.org/3/library/logging.html#logging.ERROR "logging.ERROR"), [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING"), [`INFO`](https://docs.python.org/3/library/logging.html#logging.INFO "logging.INFO") or [`DEBUG`](https://docs.python.org/3/library/logging.html#logging.DEBUG "logging.DEBUG") then you get the corresponding string. If you have associated levels with names using [`addLevelName()`](https://docs.python.org/3/library/logging.html#logging.addLevelName "logging.addLevelName") then the name you have associated with _level_ is returned. If a numeric value corresponding to one of the defined levels is passed in, the corresponding string representation is returned.

The _level_ parameter also accepts a string representation of the level such as ‘INFO’. In such cases, this functions returns the corresponding numeric value of the level.

If no matching numeric or string value is passed in, the string ‘Level %s’ % level is returned.

Note

Levels are internally integers (as they need to be compared in the logging logic). This function is used to convert between an integer level and the level name displayed in the formatted log output by means of the `%(levelname)s` format specifier (see [LogRecord attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes)), and vice versa.

Changed in version 3.4: In Python versions earlier than 3.4, this function could also be passed a text level, and would return the corresponding numeric value of the level. This undocumented behaviour was considered a mistake, and was removed in Python 3.4, but reinstated in 3.4.2 due to retain backward compatibility.

logging.getHandlerByName(_name_)[¶](https://docs.python.org/3/library/logging.html#logging.getHandlerByName "Link to this definition")

Returns a handler with the specified _name_, or `None` if there is no handler with that name.

Added in version 3.12.

logging.getHandlerNames()[¶](https://docs.python.org/3/library/logging.html#logging.getHandlerNames "Link to this definition")

Returns an immutable set of all known handler names.

Added in version 3.12.

logging.makeLogRecord(_attrdict_)[¶](https://docs.python.org/3/library/logging.html#logging.makeLogRecord "Link to this definition")

Creates and returns a new [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instance whose attributes are defined by _attrdict_. This function is useful for taking a pickled [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") attribute dictionary, sent over a socket, and reconstituting it as a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instance at the receiving end.

logging.basicConfig(_\*\*kwargs_)[¶](https://docs.python.org/3/library/logging.html#logging.basicConfig "Link to this definition")

Does basic configuration for the logging system by creating a [`StreamHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.StreamHandler "logging.StreamHandler") with a default [`Formatter`](https://docs.python.org/3/library/logging.html#logging.Formatter "logging.Formatter") and adding it to the root logger. The functions [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"), [`info()`](https://docs.python.org/3/library/logging.html#logging.info "logging.info"), [`warning()`](https://docs.python.org/3/library/logging.html#logging.warning "logging.warning"), [`error()`](https://docs.python.org/3/library/logging.html#logging.error "logging.error") and [`critical()`](https://docs.python.org/3/library/logging.html#logging.critical "logging.critical") will call [`basicConfig()`](https://docs.python.org/3/library/logging.html#logging.basicConfig "logging.basicConfig") automatically if no handlers are defined for the root logger.

This function does nothing if the root logger already has handlers configured, unless the keyword argument _force_ is set to `True`.

Note

This function should be called from the main thread before other threads are started. In versions of Python prior to 2.7.1 and 3.2, if this function is called from multiple threads, it is possible (in rare circumstances) that a handler will be added to the root logger more than once, leading to unexpected results such as messages being duplicated in the log.

The following keyword arguments are supported.

| 
Format

 | 

Description

 |
| --- | --- |
| 

_filename_

 | 

Specifies that a [`FileHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.FileHandler "logging.FileHandler") be created, using the specified filename, rather than a [`StreamHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.StreamHandler "logging.StreamHandler").

 |
| 

_filemode_

 | 

If _filename_ is specified, open the file in this [mode](https://docs.python.org/3/library/functions.html#filemodes). Defaults to `'a'`.

 |
| 

_format_

 | 

Use the specified format string for the handler. Defaults to attributes `levelname`, `name` and `message` separated by colons.

 |
| 

_datefmt_

 | 

Use the specified date/time format, as accepted by [`time.strftime()`](https://docs.python.org/3/library/time.html#time.strftime "time.strftime").

 |
| 

_style_

 | 

If _format_ is specified, use this style for the format string. One of `'%'`, `'{'` or `'$'` for [printf-style](https://docs.python.org/3/library/stdtypes.html#old-string-formatting), [`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format "str.format") or [`string.Template`](https://docs.python.org/3/library/string.html#string.Template "string.Template") respectively. Defaults to `'%'`.

 |
| 

_level_

 | 

Set the root logger level to the specified [level](https://docs.python.org/3/library/logging.html#levels).

 |
| 

_stream_

 | 

Use the specified stream to initialize the [`StreamHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.StreamHandler "logging.StreamHandler"). Note that this argument is incompatible with _filename_ - if both are present, a `ValueError` is raised.

 |
| 

_handlers_

 | 

If specified, this should be an iterable of already created handlers to add to the root logger. Any handlers which don’t already have a formatter set will be assigned the default formatter created in this function. Note that this argument is incompatible with _filename_ or _stream_ - if both are present, a `ValueError` is raised.

 |
| 

_force_

 | 

If this keyword argument is specified as true, any existing handlers attached to the root logger are removed and closed, before carrying out the configuration as specified by the other arguments.

 |
| 

_encoding_

 | 

If this keyword argument is specified along with _filename_, its value is used when the [`FileHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.FileHandler "logging.FileHandler") is created, and thus used when opening the output file.

 |
| 

_errors_

 | 

If this keyword argument is specified along with _filename_, its value is used when the [`FileHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.FileHandler "logging.FileHandler") is created, and thus used when opening the output file. If not specified, the value ‘backslashreplace’ is used. Note that if `None` is specified, it will be passed as such to [`open()`](https://docs.python.org/3/library/functions.html#open "open"), which means that it will be treated the same as passing ‘errors’.

 |

Changed in version 3.2: The _style_ argument was added.

Changed in version 3.3: The _handlers_ argument was added. Additional checks were added to catch situations where incompatible arguments are specified (e.g. _handlers_ together with _stream_ or _filename_, or _stream_ together with _filename_).

Changed in version 3.8: The _force_ argument was added.

Changed in version 3.9: The _encoding_ and _errors_ arguments were added.

logging.shutdown()[¶](https://docs.python.org/3/library/logging.html#logging.shutdown "Link to this definition")

Informs the logging system to perform an orderly shutdown by flushing and closing all handlers. This should be called at application exit and no further use of the logging system should be made after this call.

When the logging module is imported, it registers this function as an exit handler (see [`atexit`](https://docs.python.org/3/library/atexit.html#module-atexit "atexit: Register and execute cleanup functions.")), so normally there’s no need to do that manually.

logging.setLoggerClass(_klass_)[¶](https://docs.python.org/3/library/logging.html#logging.setLoggerClass "Link to this definition")

Tells the logging system to use the class _klass_ when instantiating a logger. The class should define `__init__()` such that only a name argument is required, and the `__init__()` should call `Logger.__init__()`. This function is typically called before any loggers are instantiated by applications which need to use custom logger behavior. After this call, as at any other time, do not instantiate loggers directly using the subclass: continue to use the [`logging.getLogger()`](https://docs.python.org/3/library/logging.html#logging.getLogger "logging.getLogger") API to get your loggers.

logging.setLogRecordFactory(_factory_)[¶](https://docs.python.org/3/library/logging.html#logging.setLogRecordFactory "Link to this definition")

Set a callable which is used to create a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord").

Parameters:

**factory** – The factory callable to be used to instantiate a log record.

Added in version 3.2: This function has been provided, along with [`getLogRecordFactory()`](https://docs.python.org/3/library/logging.html#logging.getLogRecordFactory "logging.getLogRecordFactory"), to allow developers more control over how the [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") representing a logging event is constructed.

The factory has the following signature:

`factory(name, level, fn, lno, msg, args, exc_info, func=None, sinfo=None, **kwargs)`

> name:
> 
> The logger name.
> 
> level:
> 
> The logging level (numeric).
> 
> fn:
> 
> The full pathname of the file where the logging call was made.
> 
> lno:
> 
> The line number in the file where the logging call was made.
> 
> msg:
> 
> The logging message.
> 
> args:
> 
> The arguments for the logging message.
> 
> exc\_info:
> 
> An exception tuple, or `None`.
> 
> func:
> 
> The name of the function or method which invoked the logging call.
> 
> sinfo:
> 
> A stack traceback such as is provided by [`traceback.print_stack()`](https://docs.python.org/3/library/traceback.html#traceback.print_stack "traceback.print_stack"), showing the call hierarchy.
> 
> kwargs:
> 
> Additional keyword arguments.

## Module-Level Attributes[¶](https://docs.python.org/3/library/logging.html#module-level-attributes "Link to this heading")

logging.lastResort[¶](https://docs.python.org/3/library/logging.html#logging.lastResort "Link to this definition")

A “handler of last resort” is available through this attribute. This is a [`StreamHandler`](https://docs.python.org/3/library/logging.handlers.html#logging.StreamHandler "logging.StreamHandler") writing to `sys.stderr` with a level of `WARNING`, and is used to handle logging events in the absence of any logging configuration. The end result is to just print the message to `sys.stderr`. This replaces the earlier error message saying that “no handlers could be found for logger XYZ”. If you need the earlier behaviour for some reason, `lastResort` can be set to `None`.

Added in version 3.2.

logging.raiseExceptions[¶](https://docs.python.org/3/library/logging.html#logging.raiseExceptions "Link to this definition")

Used to see if exceptions during handling should be propagated.

Default: `True`.

If [`raiseExceptions`](https://docs.python.org/3/library/logging.html#logging.raiseExceptions "logging.raiseExceptions") is `False`, exceptions get silently ignored. This is what is mostly wanted for a logging system - most users will not care about errors in the logging system, they are more interested in application errors.

## Integration with the warnings module[¶](https://docs.python.org/3/library/logging.html#integration-with-the-warnings-module "Link to this heading")

The [`captureWarnings()`](https://docs.python.org/3/library/logging.html#logging.captureWarnings "logging.captureWarnings") function can be used to integrate [`logging`](https://docs.python.org/3/library/logging.html#module-logging "logging: Flexible event logging system for applications.") with the [`warnings`](https://docs.python.org/3/library/warnings.html#module-warnings "warnings: Issue warning messages and control their disposition.") module.

logging.captureWarnings(_capture_)[¶](https://docs.python.org/3/library/logging.html#logging.captureWarnings "Link to this definition")

This function is used to turn the capture of warnings by logging on and off.

If _capture_ is `True`, warnings issued by the [`warnings`](https://docs.python.org/3/library/warnings.html#module-warnings "warnings: Issue warning messages and control their disposition.") module will be redirected to the logging system. Specifically, a warning will be formatted using [`warnings.formatwarning()`](https://docs.python.org/3/library/warnings.html#warnings.formatwarning "warnings.formatwarning") and the resulting string logged to a logger named `'py.warnings'` with a severity of [`WARNING`](https://docs.python.org/3/library/logging.html#logging.WARNING "logging.WARNING").

If _capture_ is `False`, the redirection of warnings to the logging system will stop, and warnings will be redirected to their original destinations (i.e. those in effect before `captureWarnings(True)` was called).

See also

Module [`logging.config`](https://docs.python.org/3/library/logging.config.html#module-logging.config "logging.config: Configuration of the logging module.")

Configuration API for the logging module.

Module [`logging.handlers`](https://docs.python.org/3/library/logging.handlers.html#module-logging.handlers "logging.handlers: Handlers for the logging module.")

Useful handlers included with the logging module.

[**PEP 282**](https://peps.python.org/pep-0282/) - A Logging System

The proposal which described this feature for inclusion in the Python standard library.

[Original Python logging package](https://old.red-dove.com/python_logging.html)

This is the original source for the [`logging`](https://docs.python.org/3/library/logging.html#module-logging "logging: Flexible event logging system for applications.") package. The version of the package available from this site is suitable for use with Python 1.5.2, 2.1.x and 2.2.x, which do not include the [`logging`](https://docs.python.org/3/library/logging.html#module-logging "logging: Flexible event logging system for applications.") package in the standard library.

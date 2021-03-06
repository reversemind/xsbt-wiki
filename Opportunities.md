[API]: https://github.com/harrah/xsbt/tree/0.11/interface
[the email thread]: https://groups.google.com/group/simple-build-tool/browse_thread/thread/7761f8b2ce51f02c/129064ea836c9baf
[advanced test interface and runner]: https://groups.google.com/group/simple-build-tool/browse_thread/thread/f5a5fe06bbf3f006/d771009d407d5765

# Opportunites (Round 2)

Below is a running list of potential areas of contribution.  This list may become out of date quickly, so you may want to check on the mailing list if you are interested in a specific topic.

1. There are plenty of possible visualization and analysis opportunities.
    * 'compile' produces an Analysis of the source code containing
        - Source dependencies
        - Inter-project source dependencies
        - Binary dependencies (jars + class files)
        - data structure representing the [API] of the source code
      There is some code already for generating dot files that isn't hooked up, but graphing dependencies and inheritance relationships is a general area of work.
    * 'update' produces an [[Update Report]] mapping `Configuration/ModuleID/Artifact` to the retrieved `File`
    * Ivy produces more detailed XML reports on dependencies.  These come with an XSL stylesheet to view them, but this does not scale to large numbers of dependencies.  Working on this is pretty straightforward: the XML files are created in `~/.ivy2` and the `.xsl` and `.css` are there as well, so you don't even need to work with sbt.  Other approaches described in [the email thread]
    * Tasks are a combination of static and dynamic graphs and it would be useful to view the graph of a run
    * Settings are a static graph and there is code to generate the dot files, but isn't hooked up anywhere.
2. If you really like testing and bigger projects, a long term, involved project is a more [advanced test interface and runner] that can handle testing JNI code and forking tests.
3. There is support for dependencies on external projects, like on github.  To be more useful, this probably needs to support being able to update the dependencies.  It is also easy to extend this to svn or other ways of retrieving projects.
4. Dependency management is a general area.  Working on Apache Ivy itself is another way to help.  For example, I'm pretty sure Ivy is fundamentally single threaded.  Either a) it's not and you can fix sbt to take advantage of this or b) make Ivy multi-threaded and faster at resolving dependencies.
5. If you like parsers, sbt commands and input tasks are written using custom parser combinators that provide tab completion and error handling.  Among other things, the efficiency could be improved.
6. The javap task hasn't been reintegrated
7. Implement enhanced 0.11-style warn/debug/info/error/trace commands.  Currently, you set it like any other setting:
```scala
  set logLevel := Level.Warn
 or
  set logLevel in Test := Level.Warn
```
 You could make commands that wrap this, like:
```text
  warn test:run
```
 Also, trace is currently an integer, but should really be an abstract data type.
8. There is more aggressive incremental compilation in sbt 0.11.  I expect it to be more difficult to reproduce bugs.  It would be helpful to have a mode that generates a diff between successive compilations and records the options passed to scalac.  This could be replayed or inspected to try to find the cause.
9. Take the webstart support from 0.7 and make it a 0.11 plugin
10. Take ownership of the 0.7 installer plugin and make it an independent 0.11 plugin

# Changelog

## Version ?.?.?

Bugs fixed:
*

Improvements and output changes:
*

Miscellaneous:
*


## Version 2.5.0 (26 November 2017)

Bugs fixed:
* Handle non-ASCII, non-UTF8 filenames in `.cvsignore` files.
* `ExternalBlobGenerator`: Don't fail if no revisions are needed for a file.
* Fix the handling of symbol-matching regexps that include `|`.
* Handle excluded paths under `Attic` directories, too.
* cvs2git: fix logging in `process_post_commit()`.
* Prefer to break internal cycles at the _largest_ timestamp gaps.
* Don't try to delete a supposed "revision 1.1" if it has a predecessor.

Improvements and output changes:
* Various small documentation fixes and improvements.
* Make cvs2xxx runnable under PyPy.
* Avoid some allusions to Subversion when converting to another VCS.
* Use `tempfile.mkdtemp()` to choose the location for temporary files.
* Write all progress information to stderr rather than stdout.
* Write cvs2git and cvs2bzr output to stdout by default.
* Improve build reproducibility by respecting `$SOURCE_DATE_EPOCH`.
* cvs2git: don't add so much useless metadata to symbol commits.
* cvs2git: allow a file to be set executable in Git via `svn:executable`

Miscellaneous:
* Use `co --version` rather than the deprecated `co -V`.


## Version 2.4.0 (22 September 2012)

New features:
* Store CVS file descriptions in a Subversion property `cvs:description`.
* SVN: Optionally include empty directories from the CVS repository.
* Much faster cvs2git conversions possible via `--use-external-blob-generator`.
* Use file properties for more flexibility over keyword and EOL handling.
* Add a `ConditionalPropertySetter`.
* Allow CVS repository paths to be excluded from the conversion.
* Normalize EOLs in CVS log messages to LF.
* Ignore vendor branch declarations that refer to non-existent branches.

Bugs fixed:
* Issue #31: cvs2svn does not convert empty directories.
* Issue #127: Dead "file X added on branch Y" revisions not always dropped.
* Fix `--dry-run` for cvs2git and cvs2bzr.

Improvements and output changes:
* More aggressively omit unnecessary dead revisions.
* Consider it a failure if `cvs` or `co` writes something to stderr.
* Add concept of "file properties", which are only computed once per file.
* Refuse to accept a default branch that is not a top-level branch.
* Make check of illegal filename characters dependent on the target VCS.
* Improve error reporting for invalid date strings in CVS.
* Many documentation improvements.
* Allow grafting a branch onto a parent that has itself been grafted.
* Slightly improve choice of parent branch for vendor branches.
* Only import "database" if used, to avoid error if no DB module installed.
* Ignore newphrases in RCS files more robustly.
* Fix the expansion of the `$Source$` keyword for `Attic` files.
* Various other minor improvements and fixes.

Miscellaneous:
* Sort large files using Python to avoid dependency on GNU sort.


## Version 2.3.0 (22 August 2009)

New features:
* Add a `cvs2git` script for starting conversions to git (or Mercurial).
* Add a `cvs2bzr` script for starting conversions to Bazaar.
* Generate manual pages automatically via new `--man` option.
* Allow `--mime-types` and `--auto-props` options to be specified more than once.
* Support author transforms when converting to Subversion.
* Allow unlabeled branches to be renamed using `SymbolTransforms`.

Bugs fixed:
* cvs2git with non-inline blobs: a revision after a delete could be empty.
* Fix timezone handling under Windows (which does not respect `TZ` variable).
* Do path comparisions platform-independently in symbol transform classes.
* Fix https://bugs.launchpad.net/pld-linux/+bug/385920

Improvements and output changes:
* Output error message if a revision's deltatext is missing.
* Improve `contrib/verify-cvs2svn.py` (used for testing conversion accuracy).

Miscellaneous:
* Add an `IgnoreSymbolTransform` class, for ignoring symbols matching a regexp.
* Remove some `DeprecationWarnings` when running under newer Python versions.


## Version 2.2.0 (23 November 2008)

New features:
* cvs2git: Omit fixup branch if a tag can be copied from an existing revision.
* cvs2git: Add option to set the maximum number of merge sources per commit.
* Allow arbitrary SVN directories to be created when a project is created.
* Allow vendor branches to be excluded, grafting child symbols to trunk.
* By default, omit trivial import branches from conversion.
    - Add `--keep-trivial-imports` option to get old behavior.
* By default, don't include `.cvsignore` files in output (except as `svn:ignore`).
    - Add option `--keep-cvsignore` to get the old behavior.
* Allow the user to specify the form of cvs2svn-generated log messages.
* Allow file contents to be written inline in `git-fast-import` streams.
* `--create-option`: allow arbitrary options to be passed to `svnadmin create`.
* Improve handling of `auto-props` file:
    - Discard extraneous spaces where they don't make sense.
    - Warn if parts of the file might be commented out unintentionally.
    - Warn if the user appears to be trying to quote a property value.

Bugs fixed:
* Fix issue #81: Remove `svn:ignore` property when `.cvsignore` is deleted.
* Fix `svn dumpfile` conformance:
    - Don't include a leading `/` for `Node-path`.
    - Include the `Node-kind` field when copying nodes.
* Make symlink test create symlinks explicitly, to avoid packaging problems.
* Accept symbol references to revision numbers that end with `.0`.

Improvements and output changes:
* When `-v`, log reasons for symbol conversion choices (tag/branch/exclude).
* Log preferred parent determinations at verbose (rather than debug) level.
* Log symbol transformations at verbose (rather than warn) level.
* Log statistics about all symbol transformations at normal level.
* cvs2git: Generate lightweight rather than annotated tags.
* `contrib/destroy_repository.py`:
  - Allow symbols, files, and directories to be renamed.
  - Allow `CVSROOT` directory contents to be erased.
  - Specify what aspects of a repo to destroy via command-line options.

Miscellaneous:
* `cvs2svn` now requires Python version 2.4 or later.


## Version 2.1.1 (15 April 2008)

Bugs fixed:
* Make files that are to be sorted more text-like to fix problem on Windows.
* Fix comment in header of `--write-symbol-info` output file.

Miscellaneous
* Adjust test suite for upstream changes in the `svntest` code.


## Version 2.1.0 (19 February 2008)

New features:
* Allow conversion of a CVS repository to git (experimental).
    - Support mapping from cvs author names to git `Author <email>` form.
* Enhance symbol transform capabilities:
    - Add `SymbolMapper`, for transforming specific symbols in specific files.
    - Allow `SymbolTransforms` to cause a symbol to be discarded.
* Enhance symbol strategy capabilities:
    - Write each CVS branch/tag to be written to an arbitrary SVN path.
    - Choose which trunk/branch should serve as the parent of each branch/tag.
    - `--symbol-hints`: manually specify how symbols should be converted.
    - Make symbol strategy rules project-specific.
* `--write-symbol-info`: output info about CVS symbols.
* Add option `ctx.decode_apple_single` for handling AppleSingle-encoded files.
* Add a new, restartable pass that converts author and log_msg to Unicode.
* Allow properties to be left unset via `auto-props` using a leading `!`.

Bugs fixed:
* Fix issue #80: Empty `CVS,v` file not accepted.
* Fix issue #108: Create project TTB directories "just-in-time".
* Fix issue #112: Random test failures with Python 2.5.
* Fix issue #115: crash - Bad file descriptor.
* Fix the translation of line-end characters for eol-styles CR and CRLF.

Improvements and output changes:
* Create trunk/tags/branches directories for project when project is created.
* Improved conversion speed significantly, especially for large repositories.
* Ignore (with a warning) symbols defined to malformed revision numbers.
* Tolerate multiple definitions of a symbol to the same revision number.
* Handle RCS files that superfluously set the default branch to trunk.
* Allow `/` characters in CVS symbol names (creating multilevel SVN paths).
* Allow symbols to be transformed to contain `/` (allowing multilevel paths).
* Convert `\` characters to `/` (rather than `--`) in symbol names.
* Make encoding problems fatal; to resolve, restart at `CleanMetadataPass`.

Miscellaneous:
* Change the default symbol handling option to `--symbol-default=heuristic`.


## Version 2.0.1 (04 October 2007)

Bugs fixed:
* Fix problem with keyword expansion when using `--use-internal-co`.


## Version 2.0.0 (15 August 2007)

New features:
* Add `--use-internal-co` to speed conversions, and make it the default.
* Add `--retain-conflicting-attic-files` option.
* Add `--no-cross-branch-commits` option.
* Add `--default-eol` option and deprecate `--no-default-eol`.
* `RevisionRecorder` hook allows file text/deltas to be recorded in pass 1.
* `RevisionReader` hook allows file text to be retrieved from `RevisionRecorder`.
* Slightly changed the order that properties are set, for more flexibility.
* Don't set `svn:keywords` on files for which `svn:eol-style` is not set.
* Implement issue #53: Allow `--trunk=''` for `--trunk-only` conversions.

Bugs fixed:
* Fix issue #97: Follow symlinks within CVS repository.
* Fix issue #99: cvs2svn tries to create a file twice.
* Fix issue #100: cvs2svn doesn't retrieve the right version.
* Fix issue #105: Conflict between directory and `Attic` file causes crash.
* Fix issue #106: `SVNRepositoryMirrorParentMissingError`.
* Fix missing command-line handling of `--fallback-encoding` option.
* Fix issue #85: Disable symbol sanity checks when in `--trunk-only` mode.

Improvements and output changes:
* Analyze CVS revision dependency graph, giving a more robust conversion.
* Improve choice of symbol parents when CVS history is ambiguous.
* In the case of clock skew to the past, resync forwards, not backwards.
* Treat timestamps that lie in the future as bogus, and adjust backwards.
* Gracefully handle tags that refer to nonexistent revisions.
* Check and fail if revision header appears multiple times.
* Gracefully handle multiple deltatext blocks for same revision.
* Be more careful about only processing reasonable `*,v` files.
* Improve checks for illegal filenames.
* Check if a directory name conflicts with a filename.
* When file is imported, omit the empty revision 1.1.
* If a non-trunk default branch is excluded, graft its contents to trunk.
* Omit the initial 'dead' revision when a file is added on a branch.
* Require `--symbol-transform` pattern to match entire symbol name.
* Treat files as binary by default instead of as text, because it is safer.
* Treat `auto-props` case-insensitively; deprecate `--auto-props-ignore-case`.

Miscellaneous:
* Add a simple (nonportable) script to log cvs2svn memory usage.
* Allow `contrib/shrink_test_case.py` script to try deleting tags and branches.
* Add `--skip-initial-test` option to `contrib/shrink_test_case.py` script.


## Version 1.5.1 (28 January 2007)

Bugs fixed:
* Add missing import in `cvs2svn_lib/process.py`.
* Fix omission of parsing of the `--fallback-encoding` option.


## Version 1.5.0 (03 October 2006)

New features:
* Support multiproject conversions (each gets its own trunk, tags, branches).
* New `--options` option to allow run-time options to be defined via a file.
* `--co`, `--cvs`, and `--sort` options to specify the paths to executables.
* Add new `--fallback-encoding` option.

Bugs fixed:
* Fix issue #86: Support multiple project roots per repository.
* Fix issue #104: Allow path to `sort` executable to be specified.
* Fix issue #8: Allow multiple `--encoding` options.
* Fix issue #109: Improve handling of fallback encodings.

Improvements and output changes:
* Further reduce conversion time and temporary space requirements.

Miscellaneous:
* Deprecate the `--dump-only` option (it is now implied by `--dumpfile`).
* Add scripts to help isolate conversion problems and shrink test cases.
* Add a script to search for illegal filenames in a CVS repository.


## Version 1.4.0 (27 August 2006)

New features:
* Support multicomponent `--trunk`, `--tags`, and `--branches` paths (issue #7).
* New `--auto-props` option allows file properties to be set via file.
* `--force-branch` and `--force-tag` options now accept regular expressions.
* Add `--symbol-default` option.
* Support multiple, ordered `--encoding` options.

Bugs fixed:
* Fix issue #93: Tags with forbidden characters converted to branches.
* Fix issue #102: Branch file, deleted in CVS, is present in SVN.

Improvements and output changes:
* Print informative warning message if a required program is missing.
* Output an error if any CVS filenames contain control characters.
* Clean up temporary files even for pass-by-pass conversions.
* Improve handling of commit dependencies and multibranch commits.
* Implemented issue #50 (performance change).
* Reduced the amount of temporary disk space needed during the conversion.

Miscellaneous:
* cvs2svn now requires Python version 2.2 or later.
* cvs2svn has been broken up into many smaller python modules for clarity.


## Version 1.3.1 (24 May 2006)

Bugs fixed:
* Fix issue #67: malfunction caused by RCS branches rooted at revision 1.0.


## Version 1.3.0 (18 August 2005)

Bugs fixed:
* Fix cvs2svn's dumpfile output to work after Subversion's r12645.
* Fix issue #71: Avoid resyncing two consecutive CVS revs to same time.
* Fix issue #88: Don't allow resyncing to throw off CVS revision order.
* Fix issue #89: Handle initially dead branch revisions acceptably.
* Fix some branch-filling bugs (r1429, r1444).

Improvements and output changes:
* Better `--encoding` support when iconv_codec module is available.
* Speedups to pass 8 (r1421)
* Use standard `rNNN` syntax when printing Subversion revisions.


## Version 1.2.1 (14 February 2005)

Bugs fixed:
* Fix cvs2svn's dumpfile output to work after Subversion's r12645.


## Version 1.2.0 (11 January 2005)

New features:
* `--fs-type=TYPE`: make it possible to specify the filesystem type.

Bugs fixed:
* Convert files with `svn:eol-style` to have LF end of lines only.
* Fix hang in pass 8 for files that ended with a CR.
* Import unexpanded keywords into the repository.
* Fix the handling of the `$Revision$` keyword.
* Fix bug in branch/tag creation edge case.


## Version 1.1.0 (15 September 2004)

New features:
* `--symbol-transform`: change tag and branch names using regular expressions.
* Flush log after writing, for better feedback when using `tee`.

Bugs fixed:
* Issue 74: No longer attempt to change non-existent files.
* Allow the Subversion repository created to have spaces in its name.
* Avoid erroring when using a `svnadmin` that uses FSFS by default.


## Version 1.0.0 (25 August 2004)

* The cvs2svn project comes of age.
